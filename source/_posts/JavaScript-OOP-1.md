title: 【JavaScript面向对象精要】（1）-原始类型和引用类型
date: 2016-11-07 12:22:12
tags: [JavaScript]
categories: JavaScript 
---
   原始类型是简单的数据值，引用类型则保存为对象，其本质是指向内存位置的引用。
   其他编程语言用栈存储原始类型，用堆存储引用类型。JavaScript则完全不同：它使用一个变量对象来追踪变量的生存期。原始值被直接保存在变量对象内，而引用值则作为一个指针保存在变量对象内，该指针指向实际对象在内存中的存储位置。
## 原始类型
   即按照原样保存的一些简单数据。有5种原始类型：    
 
 |原始类型|描述|
 |---|---|
 |boolean|布尔值，值为ture或false|
 |number|数字，值为整型或者浮点型|
 |string|字符串，值为单引号或者双引号扩出的单个或者多个连续字符（不区分字符类型）|
 |null|空类型|
 |undefined|未定义|
   在JavaScript中，原始类型的变量直接保存原始值（而不是一个指向对象的指针）。当你将原始值赋给一个变量时，该值将被复制到变量中。也就是说，如果令一个变量等于另一个变量时，每个变量有它自己的一份数据拷贝。
   ```
   var color1 = 'red';
   var color2 = color1;
   console.log(color1);//'red'
   console.log(color2);//'red'
   
   color1 = 'blue';
   console.log(color1);//'blue'
   console.log(color2);//'red'
   ```
   
### 鉴别原始类型
   - `typeof`操作符可以返回原始数据类型。然而`typeof null`返回`object`，判断一个值是否为空类型的方式可以直接与`null`比较（`===`非强制转换比较）
   - **个人觉得`Object.prototype.toString.call()`方法更加有效**    
 
 
### 原始方法
   字符串，数字以及`boolean`值拥有方法，但是他们并不是对象。    
##引用类型
   引用值是引用类型的的实例。对象是属性的无序列表，属性包含键值对，键始终是字符串，如果一个属性的值是函数，则这个属性即为方法。
### 创建对象
   JavaScript创建对象的方法有几种：
 - `new`操作符和构造函数
   构造函数就是通过new操作符创建对象的函数——任何函数都可以是构造函数，不过根据JavaScript编码规范，JavaScript中的构造函数需要首字母大写来与非构造函数来区分。
 ```
 var object = new Object();
 ```
   因为引用类型不在变量中直接保存对象，所以上例中的object变量实际上保存的是一个纸箱内存中实际对象所在位置的指针，即引用，而不是保存一个对象的实例。这是对象（引用值）和原始值之间的一个基础差别，原始值是直接保存在变量中的。
   当你讲一个对象赋值给变量时，实际是复制给这个变量一个指针。这意味着，将一个变量（指向一个对象的引用）赋值给另一个变量的时候，两个变量其实都是相同的一个指针，指向内存中的同一个对象。
 - 字面形式（内建类型实例化中会细说）
 
### 对象引用解除
   JavaScript语言有垃圾收集的功能，因此当你使用引用类型的时候无需担心内存分配。但最好还是在不使用对象的时候将其引用接触，让垃圾收集器对那块内存进行释放。解除引用的最佳手段是将对象变量设置为null。（在那些使用几百万对象的巨型程序中尤为重要）
## 内建类型实例化
 object类型只是JavaScript提供的少量内建引用类型之一。内建引用类型有：
 
|内建类型|描述|
|---|---|
|Array|数组类型，以数字为索引的一组有序列表|
|Date|日期和时间类型|
|Error|运行期错误类型|
|Function|函数类型|
|object|通用对象类型|
|RegExp|正则表达式|
### 字面形式创建对象
 JavaScript允许不使用new操作符和构造函数显示创建对象的情况下生成引用值。
```
 /*对象*/
 var book1 = {
   "name": "something",
   "year": 2016
 }
 
 var book2 = new Object();
 book2.name = "something";
 book2.year = 2016;
 /*数组*/
 var color1 = ["red", "blue", "green"];
 
 var color2 = new Array("red", "bue", "green");
 /*函数*/
 function reflect(value){
   return value;
 }
 
 var reflect2 = new Function("value", "return value");
 /*正则表达式*/
 var numbers = /\d+/g;
 
 var numbers = new RegExp('\\d', 'g');
 /**
 *正则表达式中，使用字面形式比较方便的一个原因是可以忽略转义字符。
 **/
```
 
## 访问属性
   属性是对象中保存的键值对，点号是JavaScript中访问属性的最通用做法，不过也可以通过中括号访问对象的属性值。(中括号形式可以访问有空格的属性等)
## 鉴别引用类型
   `typeof`和`instanceof`（鉴别继承类型）。所有对象都是Object的人实例，因为所有的引用类型都是继承于Object。
## 鉴别Array
   一种特殊情况：JavaScript的值可以在一个网页的不同框架之间传来传去。每一个页面都拥有它自己的全局上下文——Object、Array、以及其它内建类型的版本。在这种情况下，`instanceof`无法识别，因为数组是来自不同框架的Array实例。
   在ES5中应用Array.isArray()可以用来明确鉴别一个值是否为数组。
## 原始封装类型
   有三种：boolean、String、Number。
 当读取字符串、数字、布尔值的时候，原始封装类型会被自动创建。（Talk is cheap, show me the code）
   举个例子：
 ```
 var name = "illuSioN4ng";
 var firstChar  = name.charAt(0);
 console.log(firstChar);//"i"
 ```
   背后发生的是：
 ```
 var name = "illuSioN4ng";
 var temp = new String(name);//临时对象
 var firstChar  = temp.charAt(0);
 temp =null;//临时对象被销毁
 console.log(firstChar);
 ```
   另外一个例子：
 ```
 var name = "illuSioN4ng";
 name.last = "Leon";
 console.log(name.last);//undefined
 ```
   背后发生的是
 ```
 var name = "illuSioN4ng";
 var temp = new String(name);//临时对象temp
 var temp.last = "Leon";
 temp =null;//临时对象temp被销毁
 var temp1 = new String(name);//临时对象temp1
 console.log(temp1.last);//undefined
 temp1 =null;//临时对象temp1被销毁
 ```
   虽然原始封装类型会总被创建，在这些值上进行`instanceof`检查对应类型的返回值却都是返回false。这是因为临时对象仅在读取的时候被创建，读取完立刻被销毁了，`instanceof`并没有真的读取到任何东西，也就没有临时对象的创建，于是返回false。
   手动创建原始封装类型会导致一些副作用，`typeof`操作符会返回“object”。**尽量避免手动创建原始封装类型，避免产生不必要的误解，从而避免导致错误。**
 **  一个对象在条件判断语句中总被认为是真，无论该对象的值是否等于false。比如在jQuery中不能直接以jQuery对象为条件，需要以jQuery对象中的`length`属性的值是否为0来作为判断是否有Dom存在的条件**
## 总结
   JavaScript中虽然没有类，但是有类型。每一个变量或者数据都有一个对应的原始类型或者引用类型。5中原始类型（字符串、数字、boolean、null、undefined）的值会被直接保存在变量对象中。除了null之外都可以用`typeof`来鉴别。空类型必须直接与null进行比较才能判断。（****个人觉得`Object.prototype.toString.call()`方法更加有效****）
   引用类型是JavaScript中最近类的东西，而对象则是引用类型的实例。可以用new操作符和构造函数或者字面形式创建新的对象。通常可以用点号访问属性和方法，也可以用中括号。函数在JavaScript中也是对象，可以用`typeof`来鉴别。至于其他的引用类型，你可以使用`instanceof`和一个构造函数来判断。
   为了让原始类型看上去更加像是引用类型，JavaScript提供了三种原始封装类型：Number、String、Boolean。JavaScript会在背后创建这些对象使得你能够像普通对象那样使用原始值和他们的一些方法。不过这些临时对象会在使用结束后立即销毁。虽然你也可以手动创建原始封装类型，但是它们太容易产生误解，所以尽量避免这样做。 