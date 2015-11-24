title: Node.js(2) -- node+express+mongodb建站相关（express4）
date: 2015-09-24 09:08:29
tags: [Node.js,JavaScript]
categories: Node.js
toc: true 
---
昨天在[慕课网](http://www.imooc.com/view/75)上学习node+mongodb+express建站，中间遇到了很多问题，大部分是由于版本升级引起的。现在整理下。

### node+express+mongodb
看到一个大神关于这三个货的简单介绍，现在直接拷贝过来用下~~~~(>_<)~~~~。
1. node：
是运行在服务器端的程序语言，表面上看过去就是javascript一样的东西，但是呢，确实就是服务器语言，个人觉得在一定层次上比c灵活，java就不提了。反正你只要认为node可以干很多事就行了，绝对不只是web开发。
2. express：
这货呢，就是node的一种框架，node有很多的开源框架，express是一个大神开发的（这尊神已经移驾到go语言的开发去了）。express可以让你更方便的操作node（因为原生的node写起来比较麻烦，而且因为node是事件驱动的，所以有很多异步回调，写多了就看着晕...）
3. mongodb：这是一种非关系数据库（nosql），太深的东西我也不清楚，反正这货也有很强大的地方，缺点就是不适合数据一致性要求高的比如金融方面的开发。但是优点就快。
总结：也就是说node和mongodb组合起来特别适合一个应用场景——速度快，处理量大的情况。

### node+express的安装
node去[官网](https://nodejs.org/en/)下载，安装好后进cmd敲node，如若不行，即是系统环境变量path没有自动配置好，需手动配置下。
express的安装，我就遇到了问题，旧版本的安装直接是 npm instal -g express 就可以了，新版本4.X以上还需要安装express-generator，即敲入npm install -g express-generator。
测试express是否安装好只需要敲入 express -V即可（PS：V是大写，否则报错）
然后按照慕课网视频中的步骤新建express项目。需要注意的的就是node的启动，npm start 启动项目（也可以是node ./bin/www，旧版本直接node app.js，因为具体要看package.json里的启动配置了），昨天我就是被这个坑了半天，还以为自己哪里做错了呢。。。
