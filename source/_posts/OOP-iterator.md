title: 迭代器模式
date: 2018-01-06 15:49:56
tags: [OOP,JavaScript]
categories: [OOP]
---
&emsp;&emsp;**迭代器模式(Iterator)**：提供一种方法顺序一个聚合对象中各个元素，而又不暴露该对象内部表示。    
&emsp;&emsp;迭代器的几个特点是：    
1. 访问一个聚合对象的内容而无需暴露它的内部表示。
1. 为遍历不同的集合结构提供一个统一的接口，从而支持同样的算法在不同的集合结构上进行操作。
1. 遍历的同时更改迭代器所在的集合结构可能会导致问题（比如C#的foreach里不允许修改item）。    

&emsp;&emsp;一般的迭代，至少要有2个方法，hasNext()和Next()，这样才做做到遍历所有对象，可以看下面的例子：    
```js
var agg = (function () {
    var index = 0,
    data = [1, 2, 3, 4, 5],
    length = data.length;

    return {
        next: function () {
            var element;
            if (!this.hasNext()) {
                return null;
            }
            element = data[index];
            index = index + 2;
            return element;
        },

        hasNext: function () {
            return index < length;
        },

        rewind: function () {
            index = 0;
        },

        current: function () {
            return data[index];
        }

    };
} ());
```
&emsp;&emsp;使用方法如下：   
```js
// 迭代的结果是：1,3,5
while (agg.hasNext()) {
    console.log(agg.next());
}
```
&emsp;&emsp;也可以调用重置方法：    
```js
// 重置
agg.rewind();
console.log(agg.current()); // 1
```
&emsp;&emsp;迭代器的使用场景是：对于集合内部结果常常变化各异，我们不想暴露其内部结构的话，但又响让客户代码透明底访问其中的元素，这种情况下我们可以使用迭代器模式。

> 参考: 
> 1. [JavaScript设计模式与开发实践](https://book.douban.com/subject/26382780/) 
> 1. [JavaScript设计模式](https://book.douban.com/subject/26589719/) 