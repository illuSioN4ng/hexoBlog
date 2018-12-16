title: Constant width to height ratio
tags:
  - 30 Seconds of CSS
categories:
  - CSS
date: 2018-12-16 16:53:19
---

&emsp;&emsp;给定可变宽度的元素，它将确保其高度以响应的方式保持成比例(即，其宽度与高度的比率保持恒定)。    
## 效果
&emsp;&emsp;如下例子，产生一个宽高比为 `2:1` 的矩形容器（调整窗口大小比列不变）：    

<p data-height="265" data-theme-id="0" data-slug-hash="EGKBpE" data-default-tab="css,result" data-user="illuSioN4ng" data-pen-title="Constant width to height ratio" class="codepen">See the Pen <a href="https://codepen.io/illuSioN4ng/pen/EGKBpE/">Constant width to height ratio</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 代码
&emsp;&emsp;HTML代码很简单，一个 `div` 即可：    
```html
<div class="constant-width-to-height-ratio"></div>
```

&emsp;&emsp;CSS代码也不复杂，如下所示：    
```css
.constant-width-to-height-ratio {
  background: #333;
  width: 100%;
}
.constant-width-to-height-ratio::before {
  content: '';
  padding-top: 50%;
  float: left;
}
.constant-width-to-height-ratio::after {
  content: '';
  display: block;
  clear: both;
}
```

## 解析
&emsp;&emsp;实现的主要思路就是利用 `padding-top` 属性的百分比是基于元素 `width` 属性来计算的，因此 `padding-top: 50%;` 则表示元素的高始终为宽的一半，即能够产生宽高比为 `2:1` 的元素。

