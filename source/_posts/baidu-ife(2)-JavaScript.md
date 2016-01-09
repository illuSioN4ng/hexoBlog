title: 百度web-ife前端学院-task2 JavaScript基础学习笔记
date: 2016-01-07 20:26:29
tags: [Learning Notes,JavaScript]
categories: Learning Notes 
---
## 判断各种数据类型的方法
判断arr是否为一个数组，返回一个bool值;判断fn是否为一个函数，返回一个bool值;实现代码如下：
	```
	// 判断arr是否为一个数组，返回一个bool值
		function isArray1(arr) {//不推荐使用，因为可能会有多窗体(frame)存在，这样每一个窗口都有一个自己的 JavaScript 环境，有自己的全局对象。并且每个全局对象都有自己的一组构造函数。因此一个窗体中的对象不可能是另外窗体中的构造函数的实例。
		    // your implement
		    console.log(arr instanceof Array);
		    return arr instanceof Array;
		}
		
	function isArray2(arr) {
	    // your implement
	    console.log(Object.prototype.toString.call(arr));
	    console.log(Object.prototype.toString.call(arr) == '[object Array]');
	    return Object.prototype.toString.call(arr) == '[object Array]';
	}

	function isArray3(arr) {//ES5提供的Array.isArray() 方法
	    // your implement
	    console.log(Array.isArray(arr));
	    return Array.isArray(arr);
	}

	// 判断fn是否为一个函数，返回一个bool值
	function isFunction(fn) {
	    console.log(typeof fn === "function");
	    return typeof fn === "function";
	}
	
	var a = [1, 2, 3],
	    b = 3,
	    c = [{ abac : 1, abc : 2 }];
	isArray1(a);//true
	isArray1(b);//false
	isArray1(c);//true
	isArray2(a);//[object Array] true
	isArray2(b);//[object Number] false
	isArray2(c);//[object Array] true
	isArray3(a);//true
	isArray3(b);//false
	isArray3(c);//true
	isFunction(a);//false
	isFunction(d);//true
	```
## 值类型和引用类型
- 值类型
	存储在栈（stack）中的简单数据段，也就是说，它们的值直接存储在变量访问的位置。
- 引用类型
	存储在堆（heap）中的对象，也就是说，存储在变量处的值是一个指针（point），指向存储对象的内存处。
如果一个值是引用类型的，那么它的存储空间将从堆中分配。由于引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。相反，放在变量的栈空间中的值是该对象存储在堆中的地址。地址的大小是固定的，所以把它存储在栈中对变量性能无任何负面影响。如下图所示：
![堆栈示意](http://img.blog.csdn.net/20160107202053718)

