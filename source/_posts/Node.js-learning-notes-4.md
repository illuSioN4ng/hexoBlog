title: 七天学会node.js 2（转）
date: 2015-11-24 21:15:25
tags: [Node.js,JavaScript]
categories: Node.js
toc: true 
---
## 文件系统 fs
fs 模块是文件操作的封装，它提供了文件的读取、写入、更名、删除、遍历目录、链接等 POSIX 文件系统操作。与其他模块不同的是，fs 模块中所有的操作都提供了异步的和同步的两个版本， 例如读取文件内容的函数有异步的 fs.readFile() 和同步的fs.readFileSync()。
### fs.readFile
fs.readFile(filename,[encoding],[callback(err,data)])是最简单的读取文件的函数。它接受一个必选参数 filename，表示要读取的文件名。第二个参数 encoding是可选的，表示文件的字符编码。callback 是回调函数，用于接收文件的内容。如果不指定 encoding，则 callback 就是第二个参数。回调函数提供两个参数 err 和 data，err 表示有没有错误发生，data 是文件内容。如果指定了 encoding，data 是一个解析后的字符串，否则 data 将会是以 Buffer 形式表示的二进制数据。
例如以下程序，我们从 test.txt 中读取数据，但不指定编码：
<!--more-->

	文件目录解耦股如下：
	- 
		fs.readFile.js
		test.txt
	```
	var fs = require('fs');
	fs.readFile('test.txt', function(err, data) {
	    if (err) {
	        console.error(err);
	    } else {
	        console.log(data);
	    }
	});
	```
假设 test.txt 中的内容是 UTF-8 编码的: "Text 文本文件示例"，运行结果如下图：
![结果1](http://img.blog.csdn.net/20151124210111533)
这个程序以二进制的模式读取了文件的内容，data 的值是 Buffer 对象。如果我们给fs.readFile 的 encoding 指定编码：
```
var fs = require('fs');
fs.readFile('test.txt', 'utf-8', function(err, data) {
    if (err) {
        console.error(err);
    } else {
        console.log(data);
    }
});
```
那么运行结果则是：
![结果2](http://img.blog.csdn.net/20151124210424159)
如果我们把文件名输错，文件不存在的话，代码会报错，如下图：
![报错1](http://img.blog.csdn.net/20151124210548192)

### fs.readFileSync
fs.readFileSync(filename, [encoding])是 fs.readFile 同步的版本。它接受的参数和 fs.readFile 相同，而读取到的文件内容会以函数返回值的形式返回。如果有错误发生，fs 将会抛出异常，你需要使用 try 和 catch 捕捉并处理异常。