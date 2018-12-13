title: 七天学会node.js 1（转）
date: 2015-11-24 19:10:05
tags: [Node.js,JavaScript]
categories: Node.js
---
找到了阿里大牛写的七天学会node.js，边学边记录吧，多打一遍，总归会有点好处的。
## 模块
编写稍大一点的程序时一般都会将代码模块化。在NodeJS中，一般将代码合理拆分到不同的JS文件中，每一个文件就是一个模块，而文件路径就是模块名。
在编写每个模块时，都有require、exports、module三个预先定义好的变量可供使用。

-  require函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以./开头），或者是绝对路径（以/或C:之类的盘符开头）。另外，模块名中的.js扩展名可以省略。以下是一个例子。

```
var foo1 = require('./foo');
var foo2 = require('./foo.js');
var foo3 = require('/home/user/foo');
var foo4 = require('/home/user/foo.js');

// foo1至foo4中保存的是同一个模块的导出对象。
```

另外，可以使用以下方式加载和使用一个JSON文件。

```
var data = require('./data.json');
```

 - exports对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过require函数使用当前模块时得到的就是当前模块的exports对象。以下例子中导出了一个公有方法。
 
```
exports.hello = function () {
    console.log('Hello World!');
};
```

 - 通过module对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。例如模块导出对象默认是一个普通对象，如果想改成一个函数的话，可以使用以下方式。
 
```
module.exports = function () {
    console.log('Hello World!');
};
```

以上代码中，模块默认导出对象被替换为一个函数。

### 模块初始化
一个模块中的JS代码仅在模块第一次被使用时执行一次，并在执行过程中初始化模块的导出对象。之后，缓存起来的导出对象被重复利用。
### 主模块
通过命令行参数传递给NodeJS以启动程序的模块被称为主模块。主模块负责调度组成整个程序的其它模块完成工作。
### 完整示例
文件目录如下：

```
-/
	counter.js
	test.js
```

其中counter.js内容如下：

```
var i = 0;
function counter(){
    return ++i;
}

exports.count = counter;
```

该模块定义了一个私有变量i，并在exports对象道出了一个公有方法count。
主模块test.js内容如下：

```
var counter1 = require('./counter');
var counter2 = require('./counter');

console.log(counter1.count());
console.log(counter2.count());
console.log(counter1.count());
```

运行结果如下（我的结果是在webstorm中运行）：
![运行结果1](//img.blog.csdn.net/20151124191941336)
从结果中可以看到，counter.js并没有因为被require了两次而初始化两次。
## 二进制模块
虽然一般我们使用JS编写模块，但NodeJS也支持使用C/C++编写二进制模块。编译好的二进制模块除了文件扩展名是.node外，和JS模块的使用方式相同。虽然二进制模块能使用操作系统提供的所有功能，拥有无限的潜能，但对于前端同学而言编写过于困难，并且难以跨平台使用，因此不在本教程的覆盖范围内。
## 小结一下
- NodeJS是一个JS脚本解析器，任何操作系统下安装NodeJS本质上做的事情都是把NodeJS执行程序复制到一个目录，然后保证这个目录在系统PATH环境变量下，以便终端下可以使用node命令。

- 终端下直接输入node命令可进入命令交互模式，很适合用来测试一些JS代码片段，比如正则表达式。

- NodeJS使用CMD模块系统，主模块作为程序入口点，所有模块在执行过程中只初始化一次。

- 除非JS模块不能满足需求，否则不要轻易使用二进制模块，否则你的用户会叫苦连天。

