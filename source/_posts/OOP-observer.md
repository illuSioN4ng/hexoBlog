title: 观察者模式
date: 2018-01-07 15:19:46
tags: [OOP,JavaScript]
categories: [OOP]
---
&emsp;&emsp;**观察者模式又叫发布订阅模式（Publish/Subscribe）**，它定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象，这个主题对象的状态发生变化时就会通知所有的观察者对象，使得它们能够自动更新自己。    
&emsp;&emsp;使用观察者模式的好处：    
1. 支持简单的广播通信，自动通知所有已经订阅过的对象。
1. 页面载入后目标对象很容易与观察者存在一种动态关联，增加了灵活性。
1. 目标对象与观察者之间的抽象耦合关系能够单独扩展以及重用。

&emsp;&emsp;下面看一个例子：    
```js
//通用代码
var observer = {
    //订阅
    addSubscriber: function (callback) {
        this.subscribers[this.subscribers.length] = callback;
    },
    //退订
    removeSubscriber: function (callback) {
        for (var i = 0; i < this.subscribers.length; i++) {
            if (this.subscribers[i] === callback) {
                delete (this.subscribers[i]);
            }
        }
    },
    //发布
    publish: function (what) {
        for (var i = 0; i < this.subscribers.length; i++) {
            if (typeof this.subscribers[i] === 'function') {
                this.subscribers[i](what);
            }
        }
    },
    // 将对象o具有观察者功能
    make: function (o) { 
        for (var i in this) {
            o[i] = this[i];
            o.subscribers = [];
        }
    }
};
```
&emsp;&emsp;然后订阅2个对象blogger和user，使用observer.make方法将这2个对象具有观察者功能，代码如下：    
```js
var blogger = {
    writeBlogPost:function () {
        var content = 'Today is ' + new Date();
        this.publish(content);
    }
};
var la_times = {
    newIssue:function () {
        var paper = 'Martians have landed on Earth!';
        this.publish(paper);
    }
};

observer.make(blogger);
observer.make(la_times);
```
&emsp;&emsp;使用方法就比较简单了，订阅不同的回调函数，以便可以注册到不同的观察者对象里（也可以同时注册到多个观察者对象里）：
```js
var jack = {
    read:function (what) {
        console.log('I just read that ' + what)
    }
};
var jill = {
    gossip:function (what) {
        console.log('You didn\'t hear it from me, but ' + what)
    }
};
blogger.addSubscriber(jack.read);
blogger.addSubscriber(jill.gossip);
blogger.writeBlogPost();    // I just read that Today is Sun Jan 07 2018 15:38:54 GMT+0800 (中国标准时间)
// You didn't hear it from me, but Today is Sun Jan 07 2018 15:38:54 GMT+0800 (中国标准时间)
blogger.removeSubscriber(jill.gossip);
blogger.writeBlogPost();    // I just read that Today is Sun Jan 07 2018 15:39:46 GMT+0800 (中国标准时间)
la_times.addSubscriber(jill.gossip);
la_times.newIssue();    // You didn't hear it from me, but Martians have landed on Earth!
```
&emsp;&emsp;观察者的使用场合就是：当一个对象的改变需要同时改变其它对象，并且它不知道具体有多少对象需要改变的时候，就应该考虑使用观察者模式。    
&emsp;&emsp;总的来说，观察者模式所做的工作就是在解耦，让耦合的双方都依赖于抽象，而不是依赖于具体。从而使得各自的变化都不会影响到另一边的变化。    

> 参考: 
> 1. [JavaScript设计模式与开发实践](https://book.douban.com/subject/26382780/) 
> 1. [JavaScript设计模式](https://book.douban.com/subject/26589719/) 
> 1. [https://github.com/shichuan/javascript-patterns/blob/master/design-patterns/observer.html](https://github.com/shichuan/javascript-patterns/blob/master/design-patterns/observer.html)
> 1. [//www.cnblogs.com/TomXu/archive/2012/03/02/2355128.html](//www.cnblogs.com/TomXu/archive/2012/03/02/2355128.html)
