title: Promise对象
date: 2018-01-21 15:20:08
tags: [ESx,JavaScript]
categories: ESx
---
## 概要
&emsp;&emsp;`Promise` 网上有很多的JavaScript实现，ES6中将它写进了语言标准，统一了语言写法，并提供了原生的 `Promise` 对象的支持。    
&emsp;&emsp;所谓 `Promise` 就是一个对象用来传递异步操作的信息。 `Promise` 对象有以下两个特点：    
1. 对象的状态不受外界的影响。 `Promise` 代表一种异步操作，它一共有三种状态：Pending（未完成）、Resolved（已完成，又叫fulfilled）、Rejected（已失败）。只有异步操作的结果可以决定当前是哪一种状态，任何操作都不可以改变这个状态；
2. 一旦状态改变之后就不能再次修改，任何时候都可以得到这个结果状态。 `Promise`对象的状态改变一共有两种方式：从Pending变为Resolved，或者从Pending变为Rejected。只要其中一种发生，状态就会凝固，不会再发生改变，Promise会一直保持这个状态。

&emsp;&emsp;先简单说下 `Promise` 的优缺点，有点上面已经提到了，就是可以将异步操作流程按照同步操作的流程表达出来，避免了层层嵌套的回调写法，此外， `Promise` 提供了统一的接口，使伊布流程控制更加简单。不过同样有一些缺点，比如： `Promise` 无法取消，一旦新建它就会一直执行下去，无法中途停止，其次，如果不设置回调函数的话， `Promise` 的错误不会立即被捕获，在这 `Promise` 在Pending状态的时候，我们无法得知该状态是刚开始还是即将完成，换句话说，无法得知目前状态的具体进展。    
## 基本用法
&emsp;&emsp;ES6中规定 `Promise` 是一个构造函数，用来构建 `Promise` 实例对象。下面是一个简单的例子：    
```js
var promise = new Promise(function (resolve, reject) {
  // some code here
  if (/* 异步操作成功 */) {
    resolve(value)
  } else {
    reject(error)
  }
})
```

&emsp;&emsp; `Promise` 构造函数接受一个函数作为参数，该函数接受两个参数分别代表 `Resolved`和 `Rejected` 状态时需要执行的函数。当 `Promise` 从Pending状态变为Resolved的时候调用resolve函数，并将异步调用的结果作为参数传递给resolve函数；当从pending状态变为Rejected状态的时候执行reject函数，并将错误信息传递给reject函数。    
&emsp;&emsp; `Promise` 实例生成之后可以使用then方法来指定Resolved状态和Rejected函数的回调函数：    
```js
promise.then(function (value) {
  // do somthing
}, function (error) {
  // handle the error
})
```

&emsp;&emsp;如果调用resolve函数和reject函数时带有参数，那么这些参数可以被传递给回调函数。reject函数通常参数是Error对象的实例，表示抛出错误；resolve函数的参数除了正常的值外，还可能是一个新的promise对象，因为异步操作的结果可能是一个值也可能是另外一个异步操作，具体可以看下下面的例子：    
```js
var p1 = new Promise(function (resolve, reject) {
  // do somthing
})
var p2 = new Promise(function (resolve, reject) {
  // do somthin
  resolve(p1)
})
```

&emsp;&emsp;上面的代码中，p1和p2都是Promise的实例对象，可以看到p2中的resolve函数的参数是p1，即一个异步操作返回另一个异步操作，这种情况下，p1的状态就传递给p2。也就是说p1的状态决定p2的状态，如果p1的状态是Pending的话，那么p2的回调函数就会等待p1的状态改变；如果p1的状态已经由Pending转变为Resolved或者Rejected的话，那么p2的回调就会立即执行。    
## Promise.prototype.then() 函数
&emsp;&emsp;`then` 函数是为Promise实例添加状态改变时的毁掉函数。前面说过then函数有两个参数分别是Resolve和Reject（可选）状态的回调函数。`then` 函数返回的是一个新的Promise实例，因此可以采用链式写法，即then方法后面继续调用另外一个then方法。采用链式的then写法可以指定一组按照次序调用的函数，这时前面的的函数可能返回的还是一个Promise对象（即有异步操作），而后一个Promise实例会等待该Promise对象状态发生改变的时候再被调用。    
## Promise.prototype.catch() 函数
&emsp;&emsp;Promise.prototype.catch()函数其实是.then(null, rejection)的别名，用来指定发生错误时的回调函数。    
```js
getJson('url.your.site/data.json')
  .then(function (data) {
    // do something
  })
  .catch(function (err) {
    // handle the error
    console.log(err)
  })

// 等价于
getJson('url.your.site/data.json')
  .then(function (data) {
    // do something
  })
  .then(
    null,
    function (err) {
      // handle the error
      console.log(err)
    }
  )
```

&emsp;&emsp;一般来说，不要在then函数中定义Rejected状态的回调函数，应该在then链式调用的最后使用catch方法来做。
> catch方法返回的还是一个Promise对象，因此后面还是可以链式调用then方法

## Promise.all()方法和Promise.race()方法
&emsp;&emsp;Promise.all方法用于将多个Promise实例对象包装成一个新的Promise实例。    
```js
var p = Promise.all([p1, p2, p3])
```

&emsp;&emsp;上述代码中 Promise.all方法接受一个数组对象作为参数，数组里面所有的元素都是一个Promise实例对象。显然p的状态是由p1、p2、p3共同决定的，分为以下两种情况：    
1. 只有三者状态都转变为Fulfilled，p的状态才会变为Fulfilled，此时p1、p2、p3三者的返回值组成一个数组，返回给p的回调函数去处理；
2. 只要三者之间有一个是Rejected状态，p的状态就会变成Rejected，此时第一个被Rejected的实例的返回值就会返回给p的回调函数。    

&emsp;&emsp;Promise.race()方法和Promise.all()方法的功能不同之处在于，race方法是只需要参数数组中最先状态状态转变为Fulfilled的Promise实例的返回值，而不是等参数数组中所有的Promise对象都转变完成之后再传递。    

&emsp;&emsp;当然Promise还是有一些其他的方法，这里就不在赘述了，如果还有兴趣，可以去查找相应的文档，深入的了解一下。    

> 参阅 [ES 6标准入门 Promise 相关](http://es6.ruanyifeng.com/#docs/promise)