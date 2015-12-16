title: 七天学会node.js 3 - path（转）
date: 2015-12-14 09:52:00
tags: [Node.js,JavaScript]
categories: Node.js 
---
## path.normalize
将传入的路径转换为标准路径，具体讲的话，除了解析路径中的.与..外，还能去掉多余的斜杠。如果有程序需要使用路径作为某些数据的索引，但又允许用户随意输入路径时，就需要使用该方法保证路径的唯一性。以下是一个例子：
```
var path = require('path');
var cache = {};

function store(key, value) {
    cache[path.normalize(key)] = value;
}

store('foo/bar', 1);
console.log(cache); //=> { 'foo\bar': 1 }
store('foo//baz//../bar', 2);
console.log(cache);  // => { 'foo\bar': 2 }
```

**注意：**标准化之后的路径里的斜杠在Windows系统下是\，而在Linux系统下是/。如果想保证任何系统下都使用/作为路径分隔符的话，需要用.replace(/\\/g, '/')再替换一下标准路径。

## path.join
将传入的多个路径拼接为标准路径。该方法可避免手工拼接路径字符串的繁琐，并且能在不同系统下正确使用相应的路径分隔符。以下是一个例子：
```
console.log(path.join('foo/', 'baz/', '../bar'));//=> foo\bar
```

## path.extname
当我们需要根据不同文件扩展名做不同操作时，该方法就显得很好用。以下是一个例子：
```
console.log(path.extname('foo/bar.js'));    //  =>  .js
```

## path.basename(p[, ext])
返回路径的最后一部分。例子如下：
```
console.log(path.basename('/foo/bar/baz/asdf/quux.html'));    //  =>  quux.html
console.log(path.basename('/foo/bar/baz/asdf/quux.html', '.html'));    //  =>  quux
```

## path.format(pathObject)
从一个对象返回一个路径字符串。例子如下：
```
var path1 = path.format({
    root : "\\",
    dir : "\\home\\user\\dir",
    base : "file.txt",
    ext : ".txt",
    name : "file"
});
console.log(path1); //  =>  \home\user\dir\file.txt(注意windows下反斜杠需要转义)
```

## path.parse(pathString)
从路径字符串返回一个对象。例子如下：
```
var path2 = path.parse('/home/user/dir/file.txt');
console.log(path2);
//return
//{ root: '/',
//  dir: '/home/user/dir',
//  base: 'file.txt',
//  ext: '.txt',
//  name: 'file' }
```

**其他相关路径操作请看[官方文档](https://nodejs.org/api/path.html)**