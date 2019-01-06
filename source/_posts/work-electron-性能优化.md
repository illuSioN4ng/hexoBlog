title: electron 性能优化
tags:
  - electron
categories:
  - Performance Optimization
date: 2019-01-06 16:31:13
---


&emsp;&emsp;最近一直在进行课堂互动的开发工作，已经到达稳定阶段，同时也开始进行在一个班级60-80个左右同学的情况下，大规模测试，发现了许多许多问题，现在进行总结一下。    
&emsp;&emsp;课堂互动主要分为两大类：1. 客观题答题，2. 主观题书写。当进行课堂互动的时候，同屏时，发现整个渲染进程已经卡死，计时器展示、页面交互均不能进行。经过分析，主要问题集中在以下三点：    
1. Vuex状态耦合
2. 渲染进程计算效率低
3. 某些数据状态计算，算法时间复杂度及其低下
4. 主进程笔迹分发渲染卡顿

## Vuex状态耦合
&emsp;&emsp;在原有的服务架构下，微云（超脑）服务器会每秒发送一个 `connectstatus` 指令给教师端应用，携带已连接的学生信息，针对新增需求，添加了部分字段，数据结构如下所示：    
```json
{
	"sortid": "connectstatus",
	"data": {
		"studentlist": [{
      "userid": "",
      "name": "",
      "avatar": "",
      "no": "2",
      "hwid": "",
      "mac": "",
      "mactype": "1",
      "barcode": "",
      "classid": "",
      "rssi": "",
      "iswrite":"0",
      "isanswer":"0",
      "state": "2",
      "writestate": 0
    }],
		"aptype": "1",
		"votertype": "2",
		"device": "t9w"
	}
}
```

&emsp;&emsp;根据此数据中学生连接状态，我们在 `Vuex` 中衍生出来一系列数据,主要包括已连接学生列表、所有学生列表、已书写学生列表、客观题已答学生列表、主观题已书写学生列表。    
```js
"mac" 是mac地址
"userid" 是学生id
"iswrite" 是否有书写过
"isanswer" 是否有答过客观题
"state" 是否在线[0: 未连接 1: 尝试连接中  2: 已连接]
"writestate" 是否正在书写
```
&emsp;&emsp;起初小伙伴们采用的逻辑是，主进程每秒收到微云服务器发来的心跳信息，计算学生列表中是否有新加入学生、是否有学生书写状态变化、是否有学生答题状态变化，如果有，则推送给渲染进程，由 `Vuex` 进行状态管理。    
&emsp;&emsp;如上述方案，有个很严重的性能问题，不论是学生连接数变化还是书写或者答题状态变化，我们都会讲连接学生列表发送给渲染进程，再由 `Vuex` 自行分发状态，底层计算状态是否有变化，DOM 是否需要更新等一系列操作，然而，当连接学生数变化的时候，可能学生书写状态及答题状态并没有变化，可是在 `Vuex` 中均会进行重新的响应式变化，这里会有很严重的性能问题。    
&emsp;&emsp;所以我们采取了状态抽离的方案，计算均放到主进程中进行计算（实际我们在主进程中使用了 `node` 中的 `child_process` 进行了笔迹及状态是否改变的计算，防止计算及socket通信被阻塞），同时 `Vuex` 中 `state` 状态中由原先的仅有 `studentsListInClassroom` ，由 `store.getters` 进行衍生出其他的状态，变为  `currentClassId(当前班级ID)` 、`connectedStusListInClass（当前班级已连接学生列表）` 、 `writtenStusListInClass（当前班级已书写学生列表）` 、 `answeredStusListInClass（当前班级已回答客观题列表）` 、 `classListInClassroom（当前班级已链接学生所在班级信息）` 等。以这种方案，将 `Vuex` 的繁重状态计算，移至主进程中 `node` 环境下进行，`node` 环境的运算效率也远高于渲染进程中的计算效率，下面会提到。

## 渲染进程计算效率低
&emsp;&emsp;在定位卡顿问题的时候，我们进行使用 `console.time` 的方式进行了代码执行时间的测试，发现 `Vuex` 中 `commit` 及 某些 `getter` 非常耗时，在 `Macbook Pro` 下可能需要几百到一千毫秒，在 `windows` 环境下甚至会飙升到 `2s` 左右，还是在小规模测试环境下，除了状态耦合严重的情况，我们也觉得代码执行效率也会导致页面响应迟钝。    
&emsp;&emsp;因此我们也分别在 `electron` 主进程、渲染进程以及 `chrome` 环境下进行了简单的代码运算效率的测试。代码块就是简单输出十万个数，如下图：    

![测试代码](/img/electron-2018-12-28/计算效率Code.png)

&emsp;&emsp;主进程、渲染进程、chrome环境下的运行时间如下图所示：    
![测试代码](/img/electron-2018-12-28/main-process.time.png)

![测试代码](/img/electron-2018-12-28/renderer-process.time.png)

![测试代码](/img/electron-2018-12-28/chrome.time.png)

&emsp;&emsp;同时，在渲染进程和chrome进行运算的时候，chrome的CPU占用会飙升的非常严重，因此，我们才会考虑将计算放到node环境中计算，同时为了不阻塞主进程的事件机制，我们采取了 `child_process` 来计算，得出结果之后再通知主进程，再由主进程通知渲染进程。    
![测试代码](/img/electron-2018-12-28/进程CPU.png)

## 提升算法效率
&emsp;&emsp;由于应用卡顿，我们进行了关键计算逻辑的代码 `Review`，主要涉及，获取当前班级，已连接学生信息、分组作答已连接学生所在班级信息等，发现业务逻辑中，计算的时间复杂度非常高，有一些计算甚至达到了 O(N<sup>3</sup>) ，对于这一部分的代码，我们牺牲了一小部分的空间复杂度，来将时间复杂度降到了 `O(N)`。    
&emsp;&emsp;如根据微云返回已连接学生列表信息与教师所带班级信息比对，获取当前班级ID( `getCurrentClassId` )的优化方案。    
&emsp;&emsp;重要数据数据结构如下：    
- 微云返回连接学生列表信息

```json
[{
  "userid":"2190000159000067546",
  "name":"%E6%B5%8B%E8%AF%95%E5%85%AD%E5%8D%81%E5%9B%9B",
  "avatar":"",
  "no":"2",
  "hwid":"",
  "mac":"",
  "mactype":"1",
  "barcode":"",
  "classid":"",
  "rssi":"",
  "state":"2",
  "iswrite":"0",
  "isanswer":"0",
  "writestate":"0",
  "wificonnect":1,
  "paperId":"0",
  "pageId":"0",
  "isWrt":[],
  "praised":false
}]
```
&emsp;&emsp;其中主要是根据 `state` 字段进行判断学生连接状态。
```shell
"userid": "" // 是学生id
"iswrite": "0" // 是否有书写过
"isanswer": "0" // 是否有答过客观题
"state": "2" // 是否在线[0: 未连接 1: 尝试连接中  2: 已连接]
"writestate": 0  // 是否正在书写
```

- 班级信息    

```json
{
  "2000000030000000944": {
    "total":60,
    "data":[{
      "id":"",
      "loginName":"",
      "userName":"",
      "userPhoto":"",
      "banStatus":1,
      "banStatusList":null
    }]
  }
}
```

&emsp;&emsp;需要根据业务策略用上述两个数据进行对比获得当前班级信息。

&emsp;&emsp;优化前 `getCurrentClass` 代码：    
![getCurrentClass-old](/img/electron-2018-12-28/getCurrentClassId.png)    
&emsp;&emsp;显而易见，函数内部的多重循环嵌套最多会导致三重循环，是非常抵消的行为。    

&emsp;&emsp;该方法是放在渲染进程中的 `vuex` 中执行的，当前测试环境为微云连接学生为3人，班级列表有7个班级 ，测试结果如下:

![渲染进程耗时](/img/electron-2018-12-28/getCurrentClassId-time.png)

&emsp;&emsp;通过测试结果发现在该法在当前微云连接人数比较少的时候耗时就有623ms，在大规模测试该耗时会更多。    

&emsp;&emsp;因此我们采取了一下几个方式去优化：
1、	将获取当前班级ID计算方法从渲染进场移到主进程，再将计算结果反馈给渲染进程（***原因见上文***）；
2、	调整数据结构，降低计算复杂度。

&emsp;&emsp;我们现将教师所带学生信息进行了重新存储，数据格式改为：    
```json
{
  "<userid>": {
    "<classId>": "",
    "<stuInfo>": {}
  }
}
```
&emsp;&emsp;这样获取在线学生所在班级ID的时间复杂度就会降为1。（代码逻辑见下图）    
![teacherAllStus](/img/electron-2018-12-28/formate-teacher-all-stus.png)    

&emsp;&emsp;因此优化后的比对函数如下：      
![getCurrentClass-update](/img/electron-2018-12-28/getCurrentClassId-update.png)    

&emsp;&emsp;测试后的时间消耗如下图：    
![getCurrentClassId-update-time](/img/electron-2018-12-28/getCurrentClassId-update-time.png)    

通过获取当前班级Id方法 `getCurrentClassId` 对比，优化后的耗时明显降低，耗时很低，即使在大规模测试时耗时也很少，占用CPU资源也降低很多。    
在优化方案中，将获取当前班级，已连接学生信息、分组作答已连接学生所在班级信息等涉及到复杂运算的都作了相应处理，转移到主进程；在主进程中将涉及运算的放到子进程中，降低运算维度；并在数据有更新时才会通知渲染进程更新。