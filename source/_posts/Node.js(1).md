title: Node.js(1)
date: 2015-09-17 10:19:29
tags: [Node.js,JavaScript]
categories: Node.js
---
最近开始学习nodejs，将学习到的东西都记下来吧。

### 一个基础的服务器

```
var http = require("http");  
http.createServer(function(req, res) {  
  res.writeHead( 200 , {"Content-Type":"text/html"});  
  res.write("<h1>Node.js</h1>");  
  res.write("<p>Hello World</p>");  
  res.end("<p>beyondweb.cn</p>");  
}).listen(3000, "127.0.0.1");  
console.log("HTTP server is listening at port 3000.");  
```
上述代码包含如下的事件：
1. 从Node.js的核心请求HTTP模块并赋予一个变量http，以便以后在脚本中使用。
2. 使用http.createServer创建了新的web服务器对象。
3. 脚本将一个匿名函数传递给web服务器，告诉web服务器对象每当其接收到请求时该如何去处理。在上述代码中是显示一个html页面。
4. .listen（port, hostname）定义了web服务器的端口和主机。上述代码中可以通过http：//127.0.0.1:3000来访问服务器。
5. 在控制台打印相应信息。

### Node.js重定向 
```
var http = require("http");  
http.createServer(function(req, res) {  
  res.writeHead( 301 , {  
    "Location":"//www.baidu.com"  
  });  
  res.end("");  
}).listen(3000, "127.0.0.1");  
console.log("HTTP server is listening at port 3000.");  
```
重定向的准则如下：
1. 给客户发送301响应代码，告诉客户：资源已经移到另一个位置。
2. 发送一个位置头（Location Header)告诉客户重定向到哪里。上述代码中重定向到百度首页。

### 响应不同的请求

使用URL模块的方法：
1. 请求URL模块：
    ```
    var url = require('url');  
    ```
2. 定义请求URL或者获取请求URL：
    ```
    var requestURL   
    ```
3. 获取主机名称、端口、路径：
    ```
    url.parse(requestURL).hostname;  
    url.parse(requestURL).port;  
    url.parse(requestURL).pathname; 
    ```
一个给服务器加入路由的示例:
```
var http = require('http'),  
    url = require('url');  
  
http.createServer(function (req, res) {  
  var pathname = url.parse(req.url).pathname;  
  
  if (pathname === '/') {  
      res.writeHead(200, {  
      'Content-Type': 'text/plain'  
    });  
    res.end('Home Page\n');  
  } else if (pathname === '/about') {  
      res.writeHead(200, {  
      'Content-Type': 'text/plain'  
    });  
    res.end('About Us\n');  
  } else if (pathname === '/redirect') {  
      res.writeHead(301, {  
      'Location': '/'  
    });  
    res.end();  
  } else {  
      res.writeHead(404, {  
      'Content-Type': 'text/plain'  
    });  
    res.end('Page not found\n');  
  }  
}).listen(3000, "127.0.0.1");  
console.log('Server running at //127.0.0.1:3000/'); 
```
运行这段代码，并结合FireFox的Live HTTP Headers检查相应的HTTP响应头，可以验证代码。