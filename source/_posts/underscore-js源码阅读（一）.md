title: underscore.js源码阅读（一）
date: 2017-10-13 10:27:18
tags: [underscore.js]
categories: JavaScript
---

&emsp;&emsp;最近开始看一些传统前端框架和库的源码，包括`Bootstrap underscore`等，准备在2017年刚步入工作的这半年里面能够有时间阅读完，同时能够将阅读过程中的收获记录下来。

    阅读一些著名框架类库的源码，就好像和一个个大师对话，你会学到很多。为什么是 underscore？
    最主要的原因是 underscore 简短精悍（约 1.5k 行），封装了 100 多个有用的方法，耦合度低，
    非常适合逐个方法阅读，适合楼主这样的 JavaScript 初学者。从中，你不仅可以学到用 void 0 
    代替 undefined 避免 undefined 被重写等一些小技巧 ，也可以学到变量类型判断、函数节流&
    函数去抖等常用的方法，还可以学到很多浏览器兼容的 hack，更可以学到作者的整体设计思路以及 
    API 设计的原理（向后兼容）。    
    
&emsp;&emsp;上面这段话引自[hanzichi](https://github.com/hanzichi/underscore-analysis/issues)，以后关于underscore.js的源码阅读的过程应该会借鉴他的文章。当然也不会仅仅局限于他的文章啦(#^.^#)。    
&emsp;&emsp;这篇文章首先来说说`underscore.js`的整体架构，然后再来说说`undefined`为何被`void 0`替代，最后再来说说`underscore.js`中的`_.keys`函数中的对于`for in`在IE9以下浏览器中的特殊处理。

## `underscore.js`的整体架构
&emsp;&emsp;首先`underscore.js`用一个立即执行函数将所有的代码包裹起来，形成一个独立的作用域，来防止其他代码对于underscore代码的影响，同时避免全局变量的污染。
```
//     Underscore.js 1.8.3
//     http://underscorejs.org
//     (c) 2009-2017 Jeremy Ashkenas, DocumentCloud and Investigative Reporters & Editors
//     Underscore may be freely distributed under the MIT license.

(function() {
    //all codes here
    
    
}());

```


&emsp;&emsp;核心代码的第一部分，就是一些基本的配置。
&emsp;&emsp;首先建立root对象，在客户端（浏览器）中建立`window`(`self`)，在服务端建立（node环境下）`global`,或者一些虚拟机中建立`this`。为了支持`webWorker`，作者用`self`代替`window`进行配置。


```
// Baseline setup
    基本配置
  // --------------

  // Establish the root object, `window` (`self`) in the browser, `global`
  // on the server, or `this` in some virtual machines. We use `self`
  // instead of `window` for `WebWorker` support.
  var root = typeof self == 'object' && self.self === self && self ||
            typeof global == 'object' && global.global === global && global ||
            this ||
            {};
            
```

 
&emsp;&emsp;之后将全局环境中的`_`变量赋值给`previousUnderscore`,用于后面的`noConflict`函数来解决冲突.   

 
```
// Save the previous value of the `_` variable.
  var previousUnderscore = root._;
  
// 使用noConflict方法返回自身
  _.noConflict = function() {
    root._ = previousUnderscore;
    return this;
  };
  
```


&emsp;&emsp;接下来做了一些变量缓存，为的是减少代码压缩的体积，当然这里的代码压缩不是指的的`gzip`压缩，当然，代码缓存还包含了快速引用，减少js引擎在原型链中查找的长度，提高代码效率。    

```
// Save bytes in the minified (but not gzipped) version:
  var ArrayProto = Array.prototype, ObjProto = Object.prototype;
  var SymbolProto = typeof Symbol !== 'undefined' ? Symbol.prototype : null;

// Create quick reference variables for speed access to core prototypes.
  var push = ArrayProto.push,
      slice = ArrayProto.slice,
      toString = ObjProto.toString,
      hasOwnProperty = ObjProto.hasOwnProperty;
```


&emsp;&emsp;接下里定义了一些变量去引用`ES5`中的一些方法，如果环境允许使用，则`underscore`会优先使用它们。  
  

```
// All **ECMAScript 5** native function implementations that we hope to use
  // are declared here.
  var nativeIsArray = Array.isArray,
      nativeKeys = Object.keys,
      nativeCreate = Object.create;
```


&emsp;&emsp;之后声明一个`Ctor`变量，用于后面`baseCreate`函数来兼容老版本 JavaScript 的继承,即用来实现 Object.create 函数。 
   

```
// Naked function reference for surrogate-prototype-swapping.
  var Ctor = function(){};
  
// An internal function for creating a new object that inherits from another.
  var baseCreate = function(prototype) {
    if (!_.isObject(prototype)) return {};
    if (nativeCreate) return nativeCreate(prototype);
    Ctor.prototype = prototype;
    var result = new Ctor;
    Ctor.prototype = null;
    return result;
  };
```


&emsp;&emsp;接下来声明`_`构造函数，函数内部做了一步优化处理,用于检测用户是否使用了`new`关键字调用，若果没有，则返回一个new实例。    


```
// Create a safe reference to the Underscore object for use below.
  var _ = function(obj) {
    if (obj instanceof _) return obj;
    if (!(this instanceof _)) return new _(obj);
    this._wrapped = obj;
  };
```


接下来就是，在node.js环境下将underscore作为一个模块使用，并向后兼容旧版的模块API，即require。如果是浏览器环境下则暴露在全局环境下。 

   

```
// Export the Underscore object for **Node.js**, with
  // backwards-compatibility for their old module API. If we're in
  // the browser, add `_` as a global object.
  // (`nodeType` is checked to ensure that `module`
  // and `exports` are not HTML elements.)
  if (typeof exports != 'undefined' && !exports.nodeType) {
    if (typeof module != 'undefined' && !module.nodeType && module.exports) {
      exports = module.exports = _;
    }
    exports._ = _;
  } else {
    root._ = _;
  }

// Current version.
  _.VERSION = '1.8.3';
```


&emsp;&emsp;关于`underscore`的架构先写这么多，之后有坑再来填吧。

## `undefined`为何被`void 0`替代
&emsp;&emsp;注意到`underscore`函数中有一个函数`isUndefined`是用来判断undefined的，代码如下：   
```
// Is a given variable undefined?
  _.isUndefined = function(obj) {
    return obj === void 0;
  };
```


为什么需要用`void 0`来代替`undefined`呢？查阅了相关的资料，最终在[《You-Dont-Know-JS》](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch2.md#void-operator)中找到答案。    
&emsp;&emsp;`undefined`是一个内置的标识符，不过它是可以被赋值的，可以从以下代码中看出来。    

```js
function foo() {
	undefined = 2; // really bad idea!
}

foo();
```

```js
function foo() {
	"use strict";
	undefined = 2; // TypeError!
}

foo();
```


&emsp;&emsp;在严格模式非严格模式下，我们都可以声明一个名为`undefined`的局部变量。正是因为如此，`underscore`才会定义`isUndefined`来特殊处理`undefined`。    

```js
function foo() {
	"use strict";
	var undefined = 2;
	console.log( undefined ); // 2
}

foo();
```


&emsp;&emsp;表达式`void _`没有返回值，因此返回值为`undefined`。同时，`void`并不会改变表达式的返回值，只是让函数不返回值。按照惯例我们使用`void 0`来获取`undefined`（这种习惯来自于C语言）。    

```
var a = 42;

console.log( void a, a ); // undefined 42
```


## `_.keys`和`_.allKeys`函数

```
// Retrieve the names of an object's own properties.
     // Delegates to **ECMAScript 5**'s native `Object.keys`.
     _.keys = function(obj) {
       if (!_.isObject(obj)) return [];
       if (nativeKeys) return nativeKeys(obj);
       var keys = [];
       for (var key in obj) if (_.has(obj, key)) keys.push(key);
       // Ahem, IE < 9.
       if (hasEnumBug) collectNonEnumProps(obj, keys);
       return keys;
     };
   
     // Retrieve all the property names of an object.
     _.allKeys = function(obj) {
       if (!_.isObject(obj)) return [];
       var keys = [];
       for (var key in obj) keys.push(key);
       // Ahem, IE < 9.
       if (hasEnumBug) collectNonEnumProps(obj, keys);
       return keys;
     };
```


&emsp;&emsp;注意到`_.keys`和`_.allKeys`对于对于IE9 以下的环境做了一个BUG修复。我们就来看下`hasEnumBug`和`collectNonEnumProps`函数是怎样实现的。   

```
 // Keys in IE < 9 that won't be iterated by `for key in ...` and thus missed.
  var hasEnumBug = !{toString: null}.propertyIsEnumerable('toString');
  var nonEnumerableProps = ['valueOf', 'isPrototypeOf', 'toString',
                      'propertyIsEnumerable', 'hasOwnProperty', 'toLocaleString'];

  var collectNonEnumProps = function(obj, keys) {
    var nonEnumIdx = nonEnumerableProps.length;
    var constructor = obj.constructor;
    var proto = _.isFunction(constructor) && constructor.prototype || ObjProto;

    // Constructor is a special case.
    var prop = 'constructor';
    if (_.has(obj, prop) && !_.contains(keys, prop)) keys.push(prop);

    while (nonEnumIdx--) {
      prop = nonEnumerableProps[nonEnumIdx];
      if (prop in obj && obj[prop] !== proto[prop] && !_.contains(keys, prop)) {
        keys.push(prop);
      }
    }
  };
```


&emsp;&emsp;IE < 9下不能用`for key in ... `来枚举对象的某些 key比如重写了对象的 `toString` 方法，这个 key 值就不能在 IE < 9 下用 `for in` 枚举到IE < 9，`{toString: null}.propertyIsEnumerable('toString')`返回 false,所以IE < 9，重写的 `toString` 属性被认为不可枚举，所以在`underscore`中定义了`hasEnumBug`函数利用`propertyIsEnumerable `方法来判断是否有这个bug。    
&emsp;&emsp;IE < 9 下不能用 for in 来枚举的 key 值集合有`['valueOf', 'isPrototypeOf', 'toString', 'propertyIsEnumerable', 'hasOwnProperty', 'toLocaleString']`。    
&emsp;&emsp;proto 变量保存了原型，一个对象的原型可以通过 `obj.constructor.prototype` 获取，但是如果重写了 `constructor` 很显然就无法这样获取了，则用 `Object.prototype` 替换。这样比如说重写了 `toString`，我们只需要比较 `obj.toString` 是否和 `proto.toString `引用相同即可。    
&emsp;&emsp;至于`if (prop in obj && obj[prop] !== proto[prop] && !_.contains(keys, prop))`中对于`prop in obj`的判断，我觉得是有必要的，有一种情况如果不加结果就会不同：    

```
var o = {}
var keys = []
collectNonEnumProps(o, keys)
// keys = []

o.__proto__ = null
// or Object.setPrototypeOf(o, null)
collectNonEnumProps(o, keys)
// keys = ["toLocaleString", "hasOwnProperty", "propertyIsEnumerable", "toString", "isPrototypeOf", "valueOf"]
```


&emsp;&emsp;起始今天有点晕晕的，文章写到这也差不多写完了，后面有时间应该也会来重新看看，如果有错的地方也会自行修改。