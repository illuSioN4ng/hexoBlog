title: HTTP 方法：GET 对比 POST 
date: 2016-07-03 16:21:08
tags: [HTTP]
categories: Learning Notes 
---
上个月做绩效系统的过程中遇到了get传送请求url过长的问题，想想http中，get和post之间的区别一直说要好好整理一下，一直被耽搁了，这次就来归纳下吧。
先来看[w3cschool](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)上面的总结吧：    
## 什么是http？
超文本传输协议（HTTP）的设计目的是保证客户机与服务器之间的通信。    
HTTP 的工作方式是客户机与服务器之间的请求-应答协议。    
web 浏览器可能是客户端，而计算机上的网络应用程序也可能作为服务器端。    
举例：客户端（浏览器）向服务器提交 HTTP 请求；服务器向客户端返回响应。响应包含关于请求的状态信息以及可能被请求的内容。    
## 两种 HTTP 请求方法：GET 和 POST
在客户机和服务器之间进行请求-响应时，两种最常被用到的方法是：GET 和 POST。    
- GET - 从指定的资源请求数据。    
- POST - 向指定的资源提交要被处理的数据    

下面是360的屈屈老师在360星计划中提到的部分内容：    
- get ：对URL长度有限制，无正文    
- post： 有正文    
浏览器在处理不同请求的处理不同。    
查询类get（无害）    
数据处理类post（有副作用的）    数据多、无处理的话只是不显示，并没有加密，抓包还是可以看到明文    

### GET 方法
**请注意，查询字符串（名称/值对）是在 GET 请求的 URL 中发送的：**
```
/test/demo_form.asp?name1=value1&name2=value2
```
有关 GET 请求的其他一些注释：    
- GET 请求可被缓存    
- GET 请求保留在浏览器历史记录中    
- GET 请求可被收藏为书签    
- GET 请求不应在处理敏感数据时使用    
- GET 请求有长度限制    
- GET 请求只应当用于取回数据    


### POST 方法
**查询字符串（名称/值对）是在 POST 请求的 HTTP 消息主体中发送的：**
```
POST /test/demo_form.asp HTTP/1.1
Host: w3schools.com
name1=value1&name2=value2
```
有关 POST 请求的其他一些注释：    
- POST 请求不会被缓存    
- POST 请求不会保留在浏览器历史记录中    
- POST 不能被收藏为书签    
- POST 请求对数据长度没有要求     

### 比较 GET 与 POST

![比较 GET 与 POST](/img/getVsPost/get vs post.png)

### 其他http请求方法
![比较 GET 与 POST](/img/getVsPost/httpOthers.png)