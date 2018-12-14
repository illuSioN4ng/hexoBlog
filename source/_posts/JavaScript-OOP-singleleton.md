title: 单例模式
date: 2017-12-03 16:11:45
tags: [OOP,JavaScript]
categories: [OOP]
---

&emsp;&emsp;单例模式的定义为：保证一个类只有一个实例，并提供一个访问它的全局访问点。    
&emsp;&emsp;单例模式的经典实现方式是，创建一个类，这个类包含一个方法，这个方法在没有对象存在的情况下，将会创建一个新的实例对象。如果对象存在，这个方法只是返回这个对象的引用。    
&emsp;&emsp;单例模式是一种常用的模式，有一些对象我们往往只允许一个存在，例如：线程池、全局缓存、浏览器中的window对象和node中的global对象等等。还有实际开发中的某个登录框弹框，并非所有用户都是需要登录的，所以我们并不见得需要将弹出框布局写死在HTML中，只需要在JavaScript中第一次登录请求的时候的时时候写入且仅写入一次。    
&emsp;&emsp;下面我们来看一下最简单的一种单例模式：    
```
var mySingleton = (function () {

  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // 单例

    // 私有方法和变量
    function privateMethod(){
        console.log( "I am private" );
    }

    var privateVariable = "Im also private";

    var privateRandomNumber = Math.random();

    return {

      // 共有方法和变量
      publicMethod: function () {
        console.log( "The public can see me!" );
      },

      publicProperty: "I am also public",

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {

    // 如果存在获取此单例实例，如果不存在创建一个单例实例
    getInstance: function () {

      if ( !instance ) {
        instance = init();
      }

      return instance;
    }

  };

})();
```
&emsp;&emsp;返回的`getInstance`函数中判断`if ( !instance )`这步操作就是判断实例是否存在，从而控制实例只有一个。如果存在实例的话就返回该实例的引用。   

> 参考: 
> 1. [JavaScript设计模式与开发实践](https://book.douban.com/subject/26382780/) 
> 1. [JavaScript设计模式](https://book.douban.com/subject/26589719/) 
