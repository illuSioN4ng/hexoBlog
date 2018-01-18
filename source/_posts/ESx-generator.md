title: Generator函数
date: 2018-01-18 21:08:33
tags: [ESx,JavaScript]
categories: ESx
---
&emsp;&emsp;今天在看node.js相关内容的时候，遇到了async/await函数相关，然后想到了promise、generator函数，发现有点记忆混乱了，遂准备整理一下ES6相关的东西。（实际业务中用不到的东西真的是太容易遗忘了，虽然是好东西，能够带来极大的便利，可是业务上需要兼容低版本IE的限制，真的太大了 Orz~~）
## 基本概念
&emsp;&emsp;`Generator` 函数是ES6语法提供的一种异步编程解决方案，语法行为和传统函数完全不同。从语法上来看，可以简单把 `Generator` 函数理解成一个状态机，封装多个内部状态。执行 `Generator` 函数会返回一个迭代器对象，所以 `Generator` 函数除了是一个状态机还是一个迭代对象生成函数，返回的迭代器可以一次访问 `Generator` 函数内部的所有状态。    
&emsp;&emsp;形式上， `Generator` 是一个普通函数，但是有两个特征：1、function命令与函数名之间有一个星号；2、函数体内部使用yield语句定义不同的内部状态（ `yield` 在英语中的意思就是 “产出”的意思）。    
```js
function* helloWorldGenerator () {
  yield 'hello';
  yield 'world';
  return 'over';
}

let hw = helloWorldGenerator();
hw.next(); // {value: "hello", done: false}
hw.next(); // {value: "world", done: false}
hw.next(); // {value: "over", done: true}
hw.next(); // {value: undefined, done: true}
hw.next(); // {value: undefined, done: true}
```

&emsp;&emsp;上面代码定义了一个简单的 `Generator` 函数 - `helloWorldGenerator` ，它的内部有两个 `yield` 语句 “hello” 和 “world” ,即该函数具有三个状态： hello 、 world 和 return语句。
&emsp;&emsp; `Generator` 函数和一般函数的调用方式相同，都是在函数名之后加上一对圆括号，不同之处在于，调用 `Generator` 函数之后该函数并不执行，返回的也不是函数的运行结果，而是一个指向内部状态的指针对象，也就是迭代器对象（Iterator Object）。    
&emsp;&emsp; `Generator` 接下来就是执行迭代器对象的 `next` 方法，使得指针指向下一个状态，换句话说，就是每次调用`next` 方法内部指针都会从函数头部或者当前指针位置开始执行，直到遇到下一个 `yield` 语句（或者到 `return` 语句）为止。换言之， `Generator` 函数是分段执行的，`yield` 语句是暂停执行的标记，而 `next` 方法可以恢复函数的执行。    
&emsp;&emsp;由上面的代码可以看出，当 `Generator` 函数所有状态都执行完毕之后 `done` 属性会置为 `true` ，继续执行的话，`value` 属性的值会返回 `undefned` , `done` 属性依然为 `true` 。    
&emsp;&emsp;在ES6 中并没有规定function 关键字和函数名之间的星号写在哪个位置，这导致一下几种写法都能通过：    
```js
function* foo () {}
function * foo () {}
function *foo () {}
function*foo () {}
```
&emsp;&emsp;由于 `Generator` 函数仍然是普通函数，所以一般的写法推荐是上面的第一种，即星号紧跟在function关键字后面。    
## yield语句
&emsp;&emsp;由于 `Generator` 函数返回的遍历器对象只有调用 `next` 方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield` 语句就是暂停标志。    
&emsp;&emsp;遍历器对象的 `next` 方法的运行逻辑如下。    
1. 遇到 `yield` 语句就暂停执行后面的操作，并将紧跟在 `yield` 后的表达式的值作为返回的对象的value属性值。    
2. 下一次调用next方法时再继续往下执行，执行遇到下一条yield语句。
3. 如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值作为返回值作为对象的value属性值。
4. 如果该函数没有return语句，则返回的value值为undefined。
&emsp;&emsp;需要注意的是，yield语句后面的表达式，只有当调用next方法，内部指针指向该语句时才会执行，因此等于为JavaScript提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。    

```js
function* gen () {
    yield 111 + 222
} 
```
&emsp;&emsp;上述代码中，yield后面的表达式 '111 + 222' 不会立即求职，只会在next方法将指针移到这一句的时候才会执行并求职。    
&emsp;&emsp;yield 和 return的相似之处在于都能返回紧跟在语句后的表达式的值。区别在于每次遇到yield函数暂停执行，下一次会继续从该位置执行，而return不具备位置记忆功能。一个函数只可以执行一次return函数，但是可以执行多个yield状态，因此 `Generator` 函数可以返回多个状态值。        
&emsp;&emsp; `Generator` 可以不用yield语句，这时就变成了一个单纯的暂缓执行函数。**另外注意**，yield不能用于普通函数中，否则会报错。（SyntaxError: Unexpected number）    
## next 方法的参数
&emsp;&emsp;yield语句本身没有返回值，或者说总返回undefined。next方法可以带一个参数，该参数会被当作上一条yield语句的返回值。    
&emsp;&emsp; `Generator` 函数从暂停状态到恢复运行，其上下文状态（context）是不变，通过next方法的参数就有办法在 `Generator` 运行后继续向函数体内注入值。可以在 `Generator` 函数运行的不同阶段，从外部向内部注入不同值，从而调整整个函数行为。可以看下下面的例子：    
```js
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(6) // { value:4, done:false }
b.next(4) // { value:21, done:true }
```