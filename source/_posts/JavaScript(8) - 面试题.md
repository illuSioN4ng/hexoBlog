title: 面试题总结
date: 2016-01-08 16:14:15
tags: [JavaScript]
categories: JavaScript 
---
1. 使用` typeof bar === "object" `判断 `bar` 是不是一个对象有神马潜在的弊端？如何避免这种弊端？
	使用 `typeof `的弊端是显而易见的(这种弊端同使用 `instanceof`)：
```
	var obj = {};
	var arr = [];

	console.log(typeof obj === 'object');  //true
	console.log(typeof arr === 'object');  //true
	console.log(typeof null === 'object');  //true
	```
从上面的输出结果可知，`typeof bar === "object" `并不能准确判断 `bar` 就是一个 Object。可以通过 `Object.prototype.toString.call(bar) === "[object Object]"` 来避免这种弊端：
	```
	let obj = {};
	let arr = [];
	
	console.log(Object.prototype.toString.call(obj));  //[object Object]
	console.log(Object.prototype.toString.call(arr));  //[object Array]
	console.log(Object.prototype.toString.call(null));  //[object Null]
```
2. 下面的代码会在 console 输出神马？
```
	(function(){
	  var a = b = 3;
	})();
	
	console.log("a defined? " + (typeof a !== 'undefined'));   //a defined? false
	console.log("b defined? " + (typeof b !== 'undefined'));//b defined? true
```
所以 `b `成了全局变量，而 `a` 是自执行函数的一个局部变量。
3. 下面的代码会在 console 输出神马？
```
	var myObject = {
		foo: "bar",
		func: function() {
		    var self = this;
		    console.log("outer func:  this.foo = " + this.foo);
		    console.log("outer func:  self.foo = " + self.foo);
		    (function() {
		        console.log("inner func:  this.foo = " + this.foo);
		        console.log("inner func:  self.foo = " + self.foo);
		    }());
		}
	};
```
第一个和第二个的输出不难判断，在 ES6 之前，JavaScript 只有函数作用域，所以` func` 中的 IIFE 有自己的独立作用域，并且它能访问到外部作用域中的 `self`，所以第三个输出会报错，因为` this `在可访问到的作用域内是` undefined`，第四个输出是` bar`。