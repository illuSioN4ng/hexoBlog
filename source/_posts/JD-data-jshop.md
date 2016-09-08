title: JSHOP-数据中心二期相关总结
date: 2016-09-02 10:24:36
tags: [JD实习]
categories: Learning Notes
---
本文主要描述JSHOP数据中心相关的总结。    
## data-jshop工程目录相关
因为涉及到的目录很多，大概举例说明下就好。    
vm均是保存到相对应的栏目的目录下，css文件以及js文件也均在对应的根目录下建立了相应的目录
![vm目录](/img/data-jshop/vm.png)
![css目录](/img/data-jshop/css.png)
![js目录](/img/data-jshop/js.png)
![libs目录](/img/data-jshop/libs.png)
js目录中，正常情况下一个页面分成了两个js文件，一个model.js（主要是异步请求相关的调用），一个index.js（页面的主要逻辑）。    
js引入的库中，主要是`Sea.js + ECharts + Moment.js + DateRangePicker + jQuery-tmpl`。

## 数据中心相关细节
### 跨域相关
之前讨论的M端实时数据的链接直接取线上活动的链接插入iframe，不过因为跨域不能操作iframe。解决办法：    
1. CORS，需要其他部门同事合作去对我们这边data-jshop开启相关验证头部信息等（推动起来会很慢，否掉）；
2. 直接采用历史数据中的快照，去最新一次的快照来解决，这样就不会设计到跨域的问题（不过快照和线上页面并不是百分百一致）

### 埋点的定位
后端返回的`clickId`形如：`Jshop_ProductID_2288828_6`;英文部分为实例相关前缀， `2288828` 为实例ID， `6` 为实例中第几个包含 `href`元素的标签。
 自定义埋点 `href` 为 `#//`(比如用户设置了滑块之类的，只需要把 `a` 标签的 `href` 设置成这样就可以统计信息了)。    
 根据上述信息找到对应的dom节点，一般是 `a` 标签或者图片热区（用图片和map实现，图片会有一个 `usemap` 属性和 `map` 的 `ID` 对应）。    
 埋点的定位是直接插入到标签的所有子元素的前面（图片和map需要给图片加一个包裹层再去定位），然后再绝对定位（**注意**需要取消默认事件+事件冒泡，否则页面可能会触发 `a` 标签跳转，从而导致跨域）。    
 
### 项目目录的构建
 江哥的思路是一个页面除了公用组件都是一起引入外，另外还有该页面的业务逻辑是单独的一个模块（index.js），另外业务逻辑中的所有异步请求均放在一个model.js中来处理，业务逻辑需要在index.js中用回调函数来处理。
 
### 使用到的组件
1. [jquery-tmpl(模板渲染)](https://github.com/BorisMoore/jquery-tmpl)
[详细API](http://web.archive.org/web/20120920065217/http://api.jquery.com/category/plugins/templates/)
2. [moment.js（时间插件）](http://momentjs.cn/)
3. [bootstrap-daterangepicker(时间选择器)](https://github.com/dangrossman/bootstrap-daterangepicker)    
[详细API](http://www.daterangepicker.com/)
4. [echarts2.0](http://echarts.baidu.com/echarts2/)

### Seajs的学习
#### seajs.use原理
seajs.use("index.js")=Module.use(["index.js"],undefined,"http://xxxx.com/xxx/xxx/_use_i")    
1. 通过Module.get创建模块mod，
2. 通过Module.prototype.load加载mod模块，为mode定义history、remain、callback属性，设置_entry    
 
**Module.prototype.load**中：
1. 通过Module.prototype.resolve函数获取模块的依赖模块uri数组。
Module.prototype.resolve会遍历mod对象的dependencies中的模块id,挨个通过Module.resolve函数获取其uri，最后返回数组
2. 遍历上一步获取的uri，通过Module.get获取模块，给mod的deps属性设置值，key为依赖模块id，value为依赖模块对象
3. 通过Module.prototype.fetch 获取所有依赖模块的模块文件
4. 对子模块重复以上过程
5. mod模块加载完成后，执行onload回调，在onload中执行Module.use的回调callback，在callback中执行Module.prototype.exec来运行该模块
 
 
 