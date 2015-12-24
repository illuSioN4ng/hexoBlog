title: JavaScript(5) - 相等操作符（==和===）
date: 2015-12-17 10:57:15
tags: [JavaScript]
categories: JavaScript 
---
## ==比较符
不同类型的值用相等操作符比较后的结果如下表：

|类型（x）|类型（y）|结果|
|---|---|---|
|undefined|null|true|
|null|undefined|true|
|数字|字符串|x == toNumber(y)|
|字符串|数字|toNumber(x) == y|
|布尔值|任何类型|toNumber(x) == y|
|任何类型|布尔值|x == toNumber(y)|
|字符串或数字|对象|x == toPrimitive(y)|
|对象|字符串或数字|toPrimitive(x) == y|

如果x和y是相同类型，JavaScript会比较他们的值或者对象值。其他没有列在上述表格中的情况都会返回false。
toNumber和toPrimitive方法都是内部的。

toNumber方法对不同类型返回的结果如下：

|数值类型|结果|
|---|---|
|undefined|NaN|
|null|0|
|布尔值|**true**是1，**false**是0|
|字符串|将字符串解析成数字。如果字符串中包含字母，返回NaN；如果是数字字符组成的，转换成数字。|
|对象|Number（toPrimitive（obj））|

toPrimitive方法对不同类型返回的结果如下：

|数值类型|结果|
|---|---|
|对象|如果对象的valueOf方法的结果是原始值，返回原始值；如果对象的toString方法返回原始值，就返回这个值；其他返回错误|

## ===比较符
如果比较的两个值类型不同，比较的结果就是false；如果比较的两个值类型相同，则如下表：

|类型|值|结果|
|---|---|---|
|数字|x、y的数值相同（但不是NaN）|true|
|字符串|x、y是相同的字符串|true|
|布尔值|x、y同为ture或false|true|
|对象|x、y引用的同一个对象|true|

