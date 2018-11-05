title: 最近工作的一些总结
date: 2018-11-05 19:37:52
tags: [electron, electron-vue, Vue, JavaScript]  
categories: [electron]
---
最近很久很久都没有认真写过总结了，自从从中学作业组抽身出来，就投入到新课堂互动组，就行 `electron-vue` 相关的开发中来，遇到了许许多多的问题，也针对业务进行可以也sdk或者组件的分装，后面有机会可以写一下，这篇文章，我想总结一下前段时间我遇到的一些问题，及解决方案。    
## 临时文件的存放
之前很长一段时间都是在做 `PC Web` 相关的开发，第一次接触基于 `electron-vue` 的桌面应用的开发，期间开发了一套基于 `electron` 的桌面截屏插件 `short-capture`，因为团队内只有我一个人使用 `Mac` 所以有一些地方需要单独兼容 `Mac` ，只能自己去摸索，比如截图文件的存储地址，后来经过查阅相关资料了解到，`Windows` 系统中一般将软件的临时数据等信息存在 `C:\Users\Admin(用户名)\AppData` 中，`Mac` 中则存在 `~/Library/Application\ Support` 这种。

## 跨平台截图绝对路径展示 mac失效问题
因为本地截图成功后会返回一个本地绝对路径给回调函数，但是 `Mac` 系统中会直接将这个绝对地址附在本地 `localhost:8020` 后，导致本地图片加载不出来，这个问题纠结了我很久，后来灵光一闪，既然文件路径错了，走 `HTTP` 协议不行，为何不用** `file` **协议，配合 `Vue` 的 `filters` 很容易就可以实现了：    
![prefixer code](https://ws1.sinaimg.cn/large/8c55dc23ly1fwxfrk1nozj20vk0b6dgu.jpg)    

## JavaScript中对一个数取反
互动组中有一个学生讲和纸笔直播的功能，当中涉及到画笔的切换和板擦的切换。当中画笔和板擦的颜色切换，安卓端直接发给我一个负数（如：-16777216），当时也纳闷了很久，后来才想明白，其实只需要做一侧补码操作就可以得到解码后的十六进制字符串，代码如下：    
```js
// * colorNum = -16777216
(colorNum + parseInt('ffffffff', 16) + 1)
// * ff000000 前两位为透明度
```

## 编码后字符串长度
这个项目中我这边还写了一套消息队列的js sdk，当中有一些消息长度的封装，因为一开始只涉及到英文指令，我直接用了 `String.prototype.length`，后来加上中文字符之后，就会导致 `mq-server` 解析消息长度就解析错误，因为消息经由 `utf8` 之后中文长度就会变了（`utf8`是变长编码），这时候就需要使用 `EcmaScript` 中的 `Buffer.byteLength`,返回一个字符串的实际字节长度。 这与 `String.prototype.length` 不同，因为那返回字符串的字符数。