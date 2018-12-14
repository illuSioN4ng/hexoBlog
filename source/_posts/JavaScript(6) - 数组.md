title: JavaScript(6) - 数组
date: 2015-12-18 15:57:15
tags: [JavaScript]
categories: JavaScript 
---
## 创建和初始化数组
用JavaScript创建和初始化数组很简单：

- 用Array创建数组
```js
var daysOfWeek1 = new Array();
var daysOfWeek2 = new Array(7);
var daysOfWeek3 = new Array('Sunday', 'Monday', 'TuesDay', 'Wednesday', 'Thrsday', 'Friday', 'Saturday');
```

- 用中括号（[]）
```js
var daysOfWeek4 = [];
var daysOfWeek5 = ['Sunday', 'Monday', 'TuesDay', 'Wednesday', 'Thrsday', 'Friday', 'Saturday'];
```

## 添加和删除元素

1. 在数组的末尾添加元素
	1. 直接赋值
```js
var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
numbers[numbers.length] = 10;
console.log(numbers);//[ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
```
	2. push方法（能添加任意个元素）
```js
numbers.push(11);
console.log(numbers);//[ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 ]
numbers.push(12, 13);
console.log(numbers);//[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 
```
2. 在数组的首位添加元素
unshift方法：
```js
numbers.unshift(-1);
console.log(numbers);//[ -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 ]
numbers.unshift(-3, -2);
console.log(numbers);//[ -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 ]
```
3. 删除数组尾部的元素
pop方法(删除最后的一个元素)：
```js
numbers.pop();
console.log(numbers);//[ -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 ]
```
4. 删除数组中第一个元素
shift方法（删除数组第一个元素）：
```js
numbers.shift();
console.log(numbers);//[ -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 ]
```
5. 在数组的任意位置删除或者添加元素
splice方法：
```js
arrayObject.splice(index, howmany, item1, ..., itemX);
```
- **index**:必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
- **howmany**:必需。要删除的项目数量。如果设置为 0，则不会删除项目。
- **item1, ..., itemX:**可选。向数组添加的新项目。

```js
numbers.splice(6,3, 'a', 'a', 'a', 'a');
console.log(numbers);//[ -2, -1, 0, 1, 2, 3, 'a', 'a', 'a', 'a', 7, 8, 9, 10, 11, 12 ]
```

## 数组方法参考
下面表格中讲述了数组的一些和新方法：

| 方法名      | 描述                                                                   |
| ----------- | ---------------------------------------------------------------------- |
| concat      | 连接两个或者更多个数组，并返回结果                                     |
| every       | 对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true |
| some        | 对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true   |
| filter      | 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组       |
| forEach     | 对数组中的每一项运行给定函数。没有返回值                               |
| join        | 将所有数组元素连接成一个字符串                                         |
| indexOf     | 返回第一个与给定参数相等的数组元素的索引，没有找到返回-1               |
| lastIndexOf | 返回在数组中搜索到的与给定参数相等的元素的索引里最大的值               |
| map         | 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组         |
| reverse     | 颠倒数组元素顺序                                                       |
| slice       | 传入索引值，将数组里对应索引范围内的元素作为信数组返回                 |
| sort        | 排序                                                                   |
| toString    | 将数组作为字符串返回                                                   |
| valueOf     | 与toString相似，将数组作为字符串返回                                   |

### indexOf
包含两个参数，第一个是要检索的字符串值，第二个是可选参数，表示开始检索的位置，注：必须是合法值(0,length-1);返回值是该字符串首次出现的位置，找不到则返回-1；

```js
//numbers = [ -2, -1, 0, 1, 2, 3, 'a', 'a', 'a', 'a', 7, 8, 9, 10, 11, 12 ];
var i = numbers.indexOf("a");
console.log(i);        // 返回6

var i = numbers.indexOf("a",9);
console.log(i);        // 返回9

var i = numbers.indexOf("a",10);
console.log(i);        // 返回-1
```

找出重复元素索引的方法：

```js
var oArr = [];    // 定义一个存放索引的数组
var j = numbers.indexOf("a");    // 索引位置

while(j>-1){    // 用while循环，直到找不到索引为止
    oArr.push(j);
    j = numbers.indexOf("a",j+1);
}
console.log(oArr);//元素a的索引分别为[ 6, 7, 8, 9 ]
```

### lastIndexOf
跟indexOf，基本一样，不同的是，这个方法是从后往前检索；

```js
var i = numbers.lastIndexOf("a");
console.log(i);        // 返回9
```

### every
对数组的每个元素都进行函数运行，如果每个都是true，则返回true，否则，返回false.

### filter
对数组的每个元素都进行给定函数运行，返回这个函数会返回true的项组成的数组。

```js
var newArray = numbers.filter(function(item){
    return typeof item=="string";    
})
console.log(newArray);    // [ 'a', 'a', 'a', 'a' ]
```

### forEach
对数组的每个元素都进行函数运行，注：该方法没有返回值
```js
var arr = {                            // 定义一个对象字面量
    num: [[1,3,5],[2,3,8],[9,6,1]],    // 一个二维数组
    numR:function(o)                // 处理函数
    {
        o.reverse();        // 数组反向
    }
};

arr.num.forEach(function(item){
    arr.numR(item);
})
console.log(arr.num);//[ [ 5, 3, 1 ], [ 8, 3, 2 ], [ 1, 6, 9 ] ]
```

### some
对数组的每个元素都进行函数运行，对任一项返回为true，则返回为true.

### map
对数组的每个元素都进行函数运行，返回每次函数调用的结果组成的数组.

```js
var arr = [1,3,5];
var res = arr.map(function(item){
    return item+item;
});
console.log(res);    // 返回2,6,10
```

### reduce
array1.reduce(callback[, initialValue])
reduce方法接收一个函数作为参数，这个函数有四个参数：previousValue, currentValue, currentIndex, array；

| 回调参数      | 定义                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------- |
| previousValue | 通过上一次调用回调函数获得的值。如果向 reduce 方法提供，initialValue，则在首次调用函数时，previousValue 为 initialValue。 |
| currentValue  | 当前数组元素的值。                                                                                                        |
| currentIndex  | 当前数组元素的数字索引。                                                                                                  |
| array1        | 包含该元素的数组对象。                                                                                                    |

```js
var arr = [1,3,5];
var res = arr.reduce(function(prev,cur,index,array){
	return prev+cur;
});
console.log(res);    // 返回9
```