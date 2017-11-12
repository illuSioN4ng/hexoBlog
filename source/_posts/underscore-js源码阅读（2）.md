title: underscore.js源码阅读（2）
date: 2017-10-23 21:52:47
tags: [underscore.js]
categories: JavaScript
---
&emsp;&emsp;今天看了下underscore中的restArgs函数和createAssigner函数    
```
// Similar to ES6's rest param 
//(http://ariya.ofilabs.com/2013/03/es6-and-rest-parameter.html)
// This accumulates the arguments passed into an array, after a given index.
// 和ES6中的rest特性相似，根据给定参数的索引位置，将参数填充到索引之后的位置上
//（参见 https://github.com/jashkenas/underscore/issues/2542）
  var restArgs = function(func, startIndex) {
    startIndex = startIndex == null ? func.length - 1 : +startIndex; 
    return function() {
      var length = Math.max(arguments.length - startIndex, 0),
          rest = Array(length),
          index = 0;
      for (; index < length; index++) {
        rest[index] = arguments[index + startIndex];
      }
      switch (startIndex) {
        case 0: return func.call(this, rest);
        case 1: return func.call(this, arguments[0], rest);
        case 2: return func.call(this, arguments[0], arguments[1], rest);
      }
      var args = Array(startIndex + 1);
      for (index = 0; index < startIndex; index++) {
        args[index] = arguments[index];
      }
      args[startIndex] = rest;
      return func.apply(this, args);
    };
  };
```
&emsp;&emsp;`createAssigner`函数主要是用在下面三个地方：
```
 // Extend a given object with all the properties in passed-in object(s).
  _.extend = createAssigner(_.allKeys);

  // Assigns a given object with all the own properties in the passed-in object(s).
  // (https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
  _.extendOwn = _.assign = createAssigner(_.keys);
  
// Fill in a given object with default properties.
  _.defaults = createAssigner(_.allKeys, true);
```
&emsp;&emsp; `_.extend` 函数是用来干扩展对象属性的函数；而 `_.extendOwn` 函数则只会对象自身已有属性； `_.defaults` 函数则是，如果 key 相同，后面的不会覆盖前面的，取第一次出现某 key 的 value，为 key-value 键值对。    
&emsp;&emsp;除此之外，三个方法都能接受 >= 1 个参数，以 .extend 为例，.extend(a, b, c) 将会将 b，c 两个对象的键值对分别覆盖到 a 上。    

```
// An internal function for creating assigner functions.
// 用于实现分配器功能的内部函数
  var createAssigner = function(keysFunc, defaults) {
  // 返回函数
    // 经典闭包（defaults 参数在返回的函数中被引用）
    // 返回的函数参数个数 >= 1
    // 将第二个开始的对象参数的键值对 "继承" 给第一个参数
    return function(obj) {
      var length = arguments.length;
      // 只传入了一个参数（或者 0 个？）
      // 或者传入的第一个参数是 null
      if (defaults) obj = Object(obj);
      if (length < 2 || obj == null) return obj;
      // 枚举第一个参数除外的对象参数
      // 即 arguments[1], arguments[2] ...
      for (var index = 1; index < length; index++) {
        var source = arguments[index],
        // 提取对象参数的 keys 值
        // keysFunc 参数表示 _.keys 
        // 或者 _.allKeys
            keys = keysFunc(source),
            l = keys.length;
        // 遍历该对象的键值对
        for (var i = 0; i < l; i++) {
            var key = keys[i];
            // _.extend 和 _.extendOwn 方法
            // 没有传入 defaults 参数，即 !defaults 为 true
            // 即肯定会执行 obj[key] = source[key] 
            // 后面对象的键值对直接覆盖 obj
            // ==========================================
            // _.defaults 方法，defaults 参数为 true
            // 即 !defaults 为 false
            // 那么当且仅当 obj[key] 为 undefined 时才覆盖
            // 即如果有相同的 key 值，取最早出现的 value 值
            // _.defaults 中有相同 key 的也是一样取首次出现的
            if (!defaults || obj[key] === void 0) obj[key] = source[key];
        }
      }
      // 返回已经继承后面对象参数属性的第一个参数对象
      return obj;
    };
  };
```