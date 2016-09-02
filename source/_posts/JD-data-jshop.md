title: JSHOP-数据中心相关总结
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
 埋点的定位是直接插入到标签的所有子元素的前面（图片和map需要给图片加一个包裹层再去定位），然后再绝对定位（注意需要取消默认事件+事件冒泡，否则页面可能会触发 `a` 标签跳转，从而导致跨域）。    
 