title: http性能与优化 - 屈光宇（ququ）  
date: 2016-04-23 10:00:14 
tags: [360Star]
categories: Learning Notes 
---
屈屈大大下午来给我们讲http性能优化相关，还是蛮激动的，对于http优化这一方面的东西还是有点欠缺的，需要好好补补。

## WebRTC
WebRTC，网页实时通信（Web Real-Time Communication）的缩写，是一个支持网页浏览器进行实时语音对话或视频对话的技术，是谷歌2010年以6820万美元收购Global IP Solutions公司而获得的一项技术。2011年5月开放了工程的源代码，在行业内得到了广泛的支持和应用，成为下一代视频通话的标准。    
WebRTC（Web Real-Time Communication）项目的最终目的主要是让Web开发者能够基于浏览器（Chrome\FireFox\...）轻易快捷开发出丰富的实时多媒体应用，而无需下载安装任何插件，Web开发者也无需关注多媒体的数字信号处理过程，只需编写简单的Javascript程序即可实现，W3C等组织正在制定Javascript 标准API，目前是WebRTC 1.0版本，Draft状态；另外WebRTC还希望能够建立一个多互联网浏览器间健壮的实时通信的平台，形成开发者与浏览器厂商良好的生态环境。同时，Google也希望和致力于让WebRTC的技术成为HTML5标准之一，可见Google布局之深远。    
WebRTC提供了视频会议的核心技术，包括音视频的采集、编解码、网络传输、显示等功能，并且还支持跨平台：windows，linux，mac，android。    

## 经典面试题——URL输入到页面显示的全过程

1. 浏览器需要从URL中解析出服务器的主机名
2. 浏览器将从主机名转换成服务器IP（DNS解析）
3. 从URL解析端口号，默认80
4. 建立TCP链接
5. 浏览器向服务器发送http请求报文
6. 服务器向浏览器返回http响应报文
7. 关闭连接，浏览器显示文档
8. 浏览器显示的具体过程：
    - 根据得到的响应报文，处理HTML标签，构建dom树；
    - 处理css标记，构建cssOM；
    - 由DOM树和cssOM生成render树；
    - 在render树上进行布局阶段，计算每个可见节点的几何图形（计算布局新信息）；
    - 绘制每个独立的节点到屏幕上
    
## GET/POST
get ：对URL长度有限制，无正文    
post： 有正文    
浏览器在处理不同请求的处理不同。
- 查询类get（无害）    
- 数据处理类post（有副作用的）    
    数据多、无处理的话只是不显示，并没有加密，抓包还是可以看到明文   
    
## Cookie（cookie和session相关）
Set-Cookie来设置    
session用户标记？    
Size/Content？ Size代表占用网络带宽，Content代表资源本身的大小。Size>Content（请求图片）和Content>Size（gzip压缩）都可以。



## HTTP性能优化相关
### HTTP请求响应模型：
一个TCP链接上只能有一个请求/响应。    
keep-alive：第一次请求有两次往返延时；后续的请求只有一次往返延时；减少tcp的慢启动带来的影响。    
### 优化相关
- 同域并发限制    
以前是两个链接限制是六个链接    
- 域名散列
将页面静态资源分散在多个子域下加载。多域名能提高并发连接数，也会带来很多问题，例如：
    - 如果同一资源在不同页面被散列到不同子域下，会导致无法利用之前的 HTTP 缓存；
	- 每个域名的第一个连接都要经历 DNS 解析的过程，这在移动端可能需要耗费几百毫秒；
	- 更多的并发连接 + Keep-Alive 机制，会显著增加服务端和客户端的负担；
-  协议开销
- 模块拆分导致更多请求
- 合并请求
    - 异步接口合并
	- 图片合并，css sprite
	- css、js合并
	- css、js内联
	- 图片、音频内联（Data URI）
- 压缩
	- 代码压缩（html、css、js压缩）
	- 图片压缩（有损、无损）
	- 服务器开启Gzip(文本类资源）
	- 具体业务的压缩（去除冗余代码，精简异步借口）
- 缓存
    - 强缓存
	- 协商缓存
	    - Last-Modified：浏览器If-Modified-Since；服务器：Not-Modified
		- Etag 摘要。浏览器If-None-Match：hashcode?；服务器返回Not-Modified或者new hash-code 
		- Expires 失效时间
		- cache-control 相对时间



### 浏览器请求三种方式：
1. 在地址栏回车、点击链接   
使用全部缓存（200 from cache）
2. F5/cmd+r    
忽略Expires和cache-control，发起协商请求(304)
3. Ctrl+F5/cmd+shift+r    
忽略所有缓存(200)

### 渲染阻塞
1. 样式内联
2. 感知缓存的样式内联
3. 页面加载时间点
    - window.performance.timing来取，性能优化关注的几个时间点！（DNS/TCP/TTFB）    
    - 首屏/可用时间    
    - 白屏时间

### 用户心理
1. 让用户更快看到主题内容：   异步加载/按需加载，BigPipe；
2. 让用户知道当前状态：         加载提示、进度条、
3. 让用户知道自己更快：        占位图、预读、行为预判