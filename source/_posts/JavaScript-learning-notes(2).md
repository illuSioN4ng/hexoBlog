title: JavaScript(1)
date: 2015-09-23 12:23
tags: [JavaScript]
categories: JavaScript
toc: true 
---
### JavaScript中的==与===操作符的区别

#### 操作符：

要是两个值类型不同，返回false 
要是两个值都是number类型，并且数值相同，返回true 
要是两个值都是stirng，并且两个值的String内容相同，返回true 
要是两个值都是true或者都是false，返回true 
要是两个值都是指向相同的Object，Arraya或者function，返回true 
要是两个值都是null或者都是undefined，返回true
#### 操作符：
如果两个值具有相同类型，会进行===比较，返回===的比较值 
如果两个值不具有相同类型，也有可能返回true 
如果一个值是null另一个值是undefined，返回true 
如果一个值是string另个是number，会把string转换成number再进行比较 
如果一个值是true，会把它转成1再比较，false会转成0 
如果一个值是Object，另一个是number或者string，会把Object利用 valueOf()或者toString()转换成原始类型再进行比较 

### JavaScript中的sort排序
```
<body>  
<div>  
sort()对数组排序，不开辟新的内存，对原有数组元素进行调换  
</div>  
<div id="showBox">  
    1、简单数组简单排序  
<script type="text/javascript">  
var arrSimple=new Array(1,8,7,6);  
arrSimple.sort();  
document.writeln(arrSimple.join());  
</script>  
</div>  
<div>  
2、简单数组自定义排序  
<script type="text/javascript">  
var arrSimple2=new Array(1,8,7,6);  
arrSimple2.sort(function(a,b){  
    return b-a});  
document.writeln(arrSimple2.join());  
</script>  
PS：a,b表示数组中的任意两个元素，若return > 0 b前a后；reutrn < 0 a前b后；a=b时存在浏览器兼容  
简化一下：a-b输出从小到大排序，b-a输出从大到小排序。  
</div>  
<div>  
3、简单对象List自定义属性排序  
<script type="text/javascript">  
var objectList = new Array();  
function Person(name,age){  
    this.name=name;  
    this.age=age;  
}  
objectList.push(new Person('jack',20));  
objectList.push(new Person('tony',25));  
objectList.push(new Person('stone',26));  
objectList.push(new Person('mandy',23));  
//按年龄从小到大排序  
objectList.sort(function(a,b){  
    return a.age-b.age});  
for(var i=0;i<objectList.length;i++){  
    document.writeln('<br />age:'+objectList[i].age+' name:'+objectList[i].name);  
}  
</script>  
</div>  
<div>  
</body>
```