title: 【JavaScript面向对象精要】（3）-理解对象
date: 2016-11-24 09:51:09
tags: [JavaScript]
categories: JavaScript 
---
## 定义属性。

&emsp;&emsp;**创建对象的方式有两种：使用Object构造函数或使用对象的字面形式**    
```
var person1 = {
    name: 'GodYao'
},
    person2 = new Object();

person2.name = 'Yao47';

person1.age = 24;
person2.age = 24;
console.log(person1);
console.log(person2);
```
&emsp;&emsp;当一个属性第一次添加给对象的时候，JavaScript在对对象上调用一个名为`[[Put]]`的内部方法。`[[Put]]`方法会在对象上创建一个新节点来保存属性，就想第一次在哈希表上添加一个键一样。
&emsp;&emsp;调用`[[Put]]`方法的结果是在对象上创建了一个自有属性。一个自有属性表明仅仅该指定的的对象实例拥有该属性。该属性被直接保存在该实例中，对该属性的所有操作都必须通过该对象进行。
&emsp;&emsp;当一个已有属性被赋值的时候，调用的是一个名为`[[Set]]`的方法，该方法把当前属性值替换为新值。

## 属性探测
&emsp;&emsp;由于对象的属性可以在任何时候添加，所以有的时候需要对对象进行检查是否已有一个属性。
```
//一个错误的例子
if(person1.name) {
    //do somthing
    console.log(person1.age);
}
```
&emsp;&emsp;在JavaScript中类型的强制转换会影响上述模式的输出结果。当if判断的值是一个对象、非空字符、非零数字以及true时，判断为真；而当值为null、undefined、0、false、NaN或空字符串的时候判断为假。由于一个对象的属性值可能包含这些假值，从而导致上述代码错误。
&emsp;&emsp;in操作符在给定的对象中查找一个给定名称的属性，如果找到则为返回true。即便属性是方法也可以用这种方法判断。
```
console.log('name' in person1);//true
console.log('age' in person1);//true
console.log('sex' in person1);//false
```
&emsp;&emsp;然而在某些情况下，你可能希望仅当一个属性是自有属性才检查其是否存在。in操作符会检查自有属性和原型属性，所以在合格时候就需要使用hasOwnProperty()方法该方法在给定的属性存在且为自有属性的时候返回true。
```
var person3 = {
    name: 'GodYaoyaoyao',
    sayName: function() {
        console.log(this.name);
    }
}
console.log('name' in person3);//true
console.log(person3.hasOwnProperty('sayName'));//true

console.log('toString' in person3);//true
console.log(person3.hasOwnProperty('toString'));//false
```

## 删除属性
&emsp;&emsp;正如属性可以在任何时候被添加到对象上，它们也可以在任何时候被移除。但是设置一个属性值为null并不能从对象中彻底删除这个属性，只是调用了[[Set]]内部方法用null替代了原来的属性值而已。所以彻底删除一个属性需要调用delete操作符。
&emsp;&emsp;delete操作符针对单个对象属性调用名为[[Delete]]的内部方法。你可以理解成删除哈希表中的键值对。当delete操作成功时，返回true。
```
var person4 = {
    name: 'illuSioN4ng'
};

console.log("name" in person4);//true
console.log(delete person4.name);//true
console.log("name" in person4);//false
```

## 属性枚举
&emsp;&emsp;所有你所添加的属性都是默认可枚举的，也就说你可以用for-in来遍历它们。可枚举属性的内部特性[[enumerable]]都被设置为true。for-in循环会枚举一个对象所有的可枚举属性并将属性名赋给一个变量。
```
var obj = {
    "1": 1,
    "2": 2,
    "3": 3
},
    property;
for(property in obj) {
    console.log('Name: ' + property + '; Value: ' + obj[property]);
}
```
&emsp;&emsp;ECMAScript5 中引入了一个Object.keys()方法，它可以获取可枚举属性(自有属性)的数组。
```
var properties = Object.keys(obj);
console.log(properties);
for(var i = 0, len = properties.length; i < len; i++) {
    console.log('Name: ' + properties[i] + '; Value: ' + obj[properties[i]]);
}
```
&emsp;&emsp;并不是所有的属性都是可枚举的。实际上大部分原生方法的[[enumerable]]特征都被设置为false，你可以使用propertyIsEnumerable()方法来检查属性的可枚举性，每个对象都拥有该方法。
```
console.log("length" in properties);//true
console.log(properties.propertyIsEnumerable('length'));//false
```

## 属性类型
&emsp;&emsp;属性有两种类型：数据类型和访问器类型。数据属性包含一个值。[[Put]]方法默认行为是创建数据属性。访问器属性不包含值而定义了一个值被读取的时候调用的函数（成为getter）和一个当属性被写入时调用的函数（称为setter）。在对象字面形式中定义访问器属性有特殊的语法，如下例。
```
var person5 = {
    _name: 'GodYaoyaoyao',

    get name() {
        console.log('reading name...');
        return this._name;
    },
    set name(value) {
        console.log('setting name to %s', value);
        this._name = value;
    }
};
console.log(person5.name);  //reading name...
                            //GodYaoyaoyao
person5.name = 'GodYao';    //setting name to GodYao
console.log(person5.name);  //GodYao
```

## 通用特征
有两个属性特征是数据和访问器属性所共有的。一个是[[enumerable]]，决定你是否可以遍历该属性；一个是[[configurable]]，决定该属性是否可配置。你可以使用delete删除一个可配置的属性或者随时改变它。（**你声明的所有属性默认都是可枚举可配置的。**）
如果你想要改变属性特征，可以使用Object.defineProperty()方法。该方法接收三个参数：拥有该属性的对象、属性名、包含需要配置属性特征的描述对象。
```
var person1 = {
    name: 'illuSioN4ng'
};
Object.defineProperty(person1, 'name', {
    enumerable: false
});
console.log('name' in person1);//true
console.log(person1.propertyIsEnumerable('name'));//false
console.log('******************************');

var properties = Object.keys(person1);
console.log(properties.length);//0
console.log('******************************');

Object.defineProperty(person1, 'name', {
    configurable: false
});

delete person1.name;//严格模式下回报错
console.log('name' in person1);//true
console.log(person1.name);//'illuSioN4ng'
console.log('******************************');

//Object.defineProperty(person1, 'name', {
//    configurable: true
//});// Cannot redefine property: name
```

## 数据属性特征
数据属性拥有两个访问器属性不具备的特性。
[[Value]]，包含属性的值；[[Writable]]，布尔值，指示该该属性是否可以写入。（所有属性都是默认可写的，除非你手动制定。）
```
var person2 = {},
    person3 = {
        name: 'illuSioN4ng3'
    };
Object.defineProperty(person2, 'name', {
    value: 'illuSioN4ng2',
    enumerable: true,
    configurable: true,
    writable: true
});//使用这种方式定义新的属性的时候一定要为所有的特征指定一个值，否则均会默认为false。
console.log(person2);
console.log(person3);
```

## 访问器属性特征
访问器属性也有两个额外的特征。[[Get]]和[[Set]]，内涵getter,setter函数。
```
var person4 = {
    _name: 'illuSioN4ng 4',

    get name() {
        console.log('reading name...');
        return this._name;
    },
    set name(value) {
        console.log('setting name to %s', value);
        this._name = value;
    }
};//可以用以下方式来写

var person5 = {
    _name: 'illuSioN4ng 5'
};
Object.defineProperty(person5, 'name', {
    get: function(){
        console.log('reading name...');
        return this._name;
    },
    set: function(value){
        console.log('setting name to %s', value);
        this._name = value;
    },
    enumerable: true,
    configurable: true
});
console.log(person4);
console.log(person5);
console.log(person4.name);
console.log(person5.name);
```

## 定义多重属性
Object.defineProperties()方法来定义多重属性。接收两个参数，一个是需要改变的对象，一个是包含所有属性信息的对象。

## 获取属性。
Object.getOwnPropertyDescriptor()方法。
```
console.log(Object.getOwnPropertyDescriptor(person4, 'name'));
    /*{ get: [Function: name],
     set: [Function: name],
     enumerable: true,
     configurable: true }*/
console.log(Object.getOwnPropertyDescriptor(person5, 'name'));
    /*{ get: [Function],
     set: [Function],
     enumerable: true,
     configurable: true }*/
```

## 禁止修改对象
[[extensible]]禁止修改对象，布尔值    
三种方法：    
 1、Object.preventExtensions()创建不可扩展对象，无法添加新属性
 2、Object.seal()创建被封印对象，不可扩展且属性不可配置
 3、Object.freeze()创建被冻结属性，被封印且数据属性不可写