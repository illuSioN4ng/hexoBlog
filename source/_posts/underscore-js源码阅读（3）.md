title: underscore-js源码阅读（3）
date: 2017-11-12 15:16:28
tags: [underscore.js]
categories: JavaScript
---
&emsp;今天准备来写一下 `underscore.js` 里面判断两个参数相等的 `_.isEqual` 函数。这里的相等的含义，简单举例来说就是 `1` 和 `new Number(1)` 被认为是 `equal`，`[1]` 和 `[1]` 也被认为是 `equal`（尽管它们的引用并不相同），当然，两个引用相同的对象肯定是 equal 的了。    
```
// Perform a deep comparison to check if two objects are equal.
  _.isEqual = function(a, b) {
    return eq(a, b);
  };
```
&emsp;由上面的代码可以看出，主要的实现是在 `eq` 函数中。    
```
eq = function(a, b, aStack, bStack) {
    // Identical objects are equal. `0 === -0`, but they aren't identical.
    // See the [Harmony `egal` proposal](//wiki.ecmascript.org/doku.php?id=harmony:egal).
    //这里恒等判断中要注意 0 === -0，但是我们认为他们是不相等的
    if (a === b) return a !== 0 || 1 / a === 1 / b;
    // `null` or `undefined` only equal to itself (strict comparison).
    //这里是判断 a 或者 b 中有 `null` 或者 `undefined` 的情况，直接返回 `false`
    if (a == null || b == null) return false;
    // `NaN`s are equivalent, but non-reflexive.
    // `NaN` 在js中是不相等的，但是我们在这里视为相等
    if (a !== a) return b !== b;
    // Exhaust primitive checks
    // 这里这部分没太懂，以后来填坑
    var type = typeof a;
    if (type !== 'function' && type !== 'object' && typeof b != 'object') return false;
    return deepEq(a, b, aStack, bStack);
  };
```
&emsp;接下来返回一个 `deepEq` 函数利用 `Object.prototype.toString.call` 来判断对象类型，如下所示：    
```
// Internal recursive comparison function for `isEqual`.
  deepEq = function(a, b, aStack, bStack) {
    // Unwrap any wrapped objects.
    if (a instanceof _) a = a._wrapped;
    if (b instanceof _) b = b._wrapped;
    // Compare `[[Class]]` names.
    var className = toString.call(a);
    if (className !== toString.call(b)) return false;
    switch (className) {
      // Strings, numbers, regular expressions, dates, and booleans are compared by value.
      case '[object RegExp]':
      // RegExps are coerced to strings for comparison (Note: '' + /a/i === '/a/i')
      // 将正则表达式转换成字符串进行比较
      case '[object String]':
        // Primitives and their corresponding object wrappers are equivalent; thus, `"5"` is
        // equivalent to `new String("5")`.
        return '' + a === '' + b;
      case '[object Number]':
        // `NaN`s are equivalent, but non-reflexive.
        // Object(NaN) is equivalent to NaN.
        if (+a !== +a) return +b !== +b;
        // An `egal` comparison is performed for other numeric values.
        return +a === 0 ? 1 / +a === 1 / b : +a === +b;
      case '[object Date]':
      case '[object Boolean]':
        // Coerce dates and booleans to numeric primitive values. Dates are compared by their
        // millisecond representations. Note that invalid dates with millisecond representations
        // of `NaN` are not equivalent.
        // 数字和boolean均转换成数字进行比较
        return +a === +b;
        // 这里还新增了对于 `Symbol` 的支持
      case '[object Symbol]':
        return SymbolProto.valueOf.call(a) === SymbolProto.valueOf.call(b);
    }

    //数组和对象的比较，就运用到了递归的运用了，有些许复杂，用心去理解就好啦
    var areArrays = className === '[object Array]';
    if (!areArrays) {
      if (typeof a != 'object' || typeof b != 'object') return false;

      // Objects with different constructors are not equivalent, but `Object`s or `Array`s
      // from different frames are.
      var aCtor = a.constructor, bCtor = b.constructor;
      if (aCtor !== bCtor && !(_.isFunction(aCtor) && aCtor instanceof aCtor &&
                               _.isFunction(bCtor) && bCtor instanceof bCtor)&& 
                              ('constructor' in a && 'constructor' in b)) {
        return false;
      }
    }
    // Assume equality for cyclic structures. The algorithm for detecting cyclic
    // structures is adapted from ES 5.1 section 15.12.3, abstract operation `JO`.

    // Initializing stack of traversed objects.
    // It's done here since we only need them for objects and arrays comparison.
    aStack = aStack || [];
    bStack = bStack || [];
    var length = aStack.length;
    while (length--) {
      // Linear search. Performance is inversely proportional to the number of
      // unique nested structures.
      if (aStack[length] === a) return bStack[length] === b;
    }

    // Add the first object to the stack of traversed objects.
    aStack.push(a);
    bStack.push(b);

    // Recursively compare objects and arrays.
    if (areArrays) {
      // Compare array lengths to determine if a deep comparison is necessary.
      length = a.length;
      if (length !== b.length) return false;
      // Deep compare the contents, ignoring non-numeric properties.
      while (length--) {
        if (!eq(a[length], b[length], aStack, bStack)) return false;
      }
    } else {
      // Deep compare objects.
      var keys = _.keys(a), key;
      length = keys.length;
      // Ensure that both objects contain the same number of properties before comparing deep equality.
      if (_.keys(b).length !== length) return false;
      while (length--) {
        // Deep compare each member
        key = keys[length];
        if (!(_.has(b, key) && eq(a[key], b[key], aStack, bStack))) return false;
      }
    }
    // Remove the first object from the stack of traversed objects.
    aStack.pop();
    bStack.pop();
    return true;
  };
```
&emsp;这部分的内容基本上也就说完了，仅仅记录一下自己注意到的东西，也有很多不太清楚的细节处理，慢慢积累吧，以后会回来补坑的。