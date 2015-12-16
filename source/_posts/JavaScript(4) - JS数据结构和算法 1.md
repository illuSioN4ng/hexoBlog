title: JS数据结构和算法 1
date: 2015-12-16 11:28:15
tags: [JavaScript]
categories: JavaScript 
---
## JavaScript中的真假值
先来看一个表格：

|数值类型|转化成布尔值|
|---|---|
|undefined|false|
|null|false|
|布尔值|**true**是true，**false**是false|
|数字|+0，-0，NaN都是false，其他都是true|
|字符串|如果字符串是空的（长度为0）即为false，其他都是true|
|对象|true|

下面一段代码：
```
    var myObj = {
        name: 'illuSioN4ng'
    };
    console.log(myObj);
    
    function testTruthy(val){
        return val ? console.log('true') : console.log('false');
    }
    
    console.log('==================');
    testTruthy(true);   //      true
    testTruthy(false);  //      false
    testTruthy(new Boolean(false));//   对象始终为true
    
    console.log('==================');
    testTruthy('');     //      false
    testTruthy('illuSioN4ng');//true
    testTruthy(new String(''));//   对象始终为true
    
    console.log('==================');
    testTruthy(1);      //      true
    testTruthy(-1);     //      true
    testTruthy(0);      //      false
    testTruthy(-0);     //      false
    testTruthy(NaN);    //      false
    testTruthy(new Number(NaN));//   对象始终为true
    
    console.log('==================');
    testTruthy({});//   对象始终为true
    testTruthy(myObj);  //      true
    testTruthy(myObj.name);//   true
    testTruthy(myObj.height);// false
    console.log('==================');
```