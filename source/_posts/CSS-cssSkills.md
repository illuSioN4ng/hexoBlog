title: 12 个 CSS 高级技巧
date: 2016-04-05 10:26:51
tags: [Learning Notes,CSS]
categories: CSS 
---
## 使用:not()在菜单上应用/取消应用边框
一般做法：先给每一个菜单添加边框
```
/* add border */
.nav li {
  border-right: 1px solid #666;
}
```
然后再去除最后一个元素    
```
// remove border /

.nav li:last-child {
  border-right: none;
}
```

高级技巧：
直接使用`:not()`伪类来应用元素    
```
.nav li:not(:last-child) {
  border-right: 1px solid #666;
}
```
这样代码就干净易读了。

## 给body添加行高
你不需要分别添加 line-height 到每个 `<p>`，`<h*>`等。只要添加到 body 即可：    
```
body {
  line-height: 1;
}
```
这样文本元素就可以很容易地从 body 继承。

## 所有一切都垂直居中

```
html, body {
  height: 100%;
  margin: 0;
}

body {
  -webkit-align-items: center;  
  -ms-flex-align: center;  
  align-items: center;
  display: -webkit-flex;
  display: flex;
}
```
**注：在IE11中要小心flexbox**

## 逗号分隔的列表
让HTML列表项看起来像一个真正用逗号分隔的列表：
```
ul > li:not(:last-child)::after {
  content: ",";
}
```
对最后一个列表项使用`:not()`伪类。

## 使用负的nth-child选择项目
在CSS中使用负的nth-child选择项目1到n：(例如1到3)
```
li {
  display: none;
}

/* select items 1 through 3 and display them */
li:nth-child(-n+3) {
  display: block;
}
```

## 对图标使用SVG
```
.logo {
  background: url("logo.svg");
}
```
SVG对所有的分辨率类型都具有良好的扩展性，并支持所有浏览器都回归到IE9。这样可以避开.png、.jpg或.gif文件了。

## 优化显示文本
有时，字体并不能在所有设备上都达到最佳的显示，所以可以让设备浏览器来帮助你：    
```
html {
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}
```
注：请负责任地使用 optimizeLegibility。此外，IE /Edge没有 text-rendering 支持。

## 对纯CSS滑块使用 max-height
使用 max-height 和溢出隐藏来实现只有CSS的滑块：    
```
.slider ul {
  max-height: 0;
  overlow: hidden;
}

.slider:hover ul {
  max-height: 1000px;
  transition: .3s ease;
}
```

## 继承 box-sizing
让 box-sizing 继承 html：    
```
html {
  box-sizing: border-box;
}

*, *:before, *:after {
  box-sizing: inherit;
}
```
这样在插件或杠杆其他行为的其他组件中就能更容易地改变 box-sizing 了。

## 表格单元格等宽
表格工作起来很麻烦，所以务必尽量使用 table-layout: fixed 来保持单元格的等宽：    
```
.calendar {
  table-layout: fixed;
}
```

## 用Flexbox摆脱外边距的各种hack
当需要用到列分隔符时，通过flexbox的 space-between 属性，你就可以摆脱nth-，first-，和 last-child 的hack了：    
```
.list {
  display: flex;
  justify-content: space-between;
}

.list .person {
  flex-basis: 23%;
}
```

## 使用属性选择器用于空链接
当 `<a>` 元素没有文本值，但 href 属性有链接的时候显示链接：    
```
a[href^="http"]:empty::before {
  content: attr(href);
}
```