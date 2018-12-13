title: 从一行CSS调试代码中学到的JavaScript知识
date: 2016-11-30 16:25:29
tags: [JavaScript]
categories: JavaScript 
---
&emsp;&emsp;话不多说，直接上代码：
```
[].forEach.call($$("*"),function(a){
  a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
});
```
&emsp;&emsp;&emsp;&emsp;话不多说，直接上代码：
```
[].forEach.call($$("*"),function(a){
    a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
});
```
&emsp;&emsp;`$$('*')`等价于`document.querySelectorAll('*')`。[JavaScript中的$$(*)代表什么和$选择器的由来](//ourjs.com/detail/54ab768a5695544119000007)
&emsp;&emsp;`1<<24`返回的是16777216（2^24）,所以`Math.random()*(1<<24)`可以得到一个0 到 16777216之间的值;`~`操作符（按位取反操作）,通过两次取返就可以得到纯整数部，我们还可以将`~~`视为`parseInt`的简写，所以`~~(Math.random()*(1<<24))`返回的是一个整数。最后颜色就是`#000000~#FFFFFF`。