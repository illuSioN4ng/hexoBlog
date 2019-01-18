title: 高度过渡
tags:
  - CSS-30-Seconds-of-CSS
categories:
  - CSS
date: 2019-01-18 09:06:21
---

&emsp;&emsp;今天来说一下鼠标悬浮的时候的高度过渡效果。效果图如下：    

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="illuSioN4ng" data-slug-hash="oJRwdb" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="Height transition">
  <span>See the Pen <a href="https://codepen.io/illuSioN4ng/pen/oJRwdb/">
  Height transition</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 代码
&emsp;&emsp;`HTML` 代码比较简单：    
```html
<div class="trigger">
  Hover me to see a height transition.
  <div class="el">content</div>
</div>
```

&emsp;&emsp;`CSS` 代码如下：    
```css
.el {
  transition: height 0.5s;
  overflow: hidden;
  height: 0;
}
.trigger:hover > .el {
  height: var(--max-height);
}
```

&emsp;&emsp;还涉及到部分 `JavaScript` 代码：    
```js
var el = document.querySelector('.el')
var height = el.scrollHeight
console.log(height)
el.style.setProperty('--max-height', height + 'px')
```

## 解析
&emsp;&emsp; `CSS` 中主要使用到的样式如下：    
1. `transition: height 0.5s;` 对元素高度属性使用过渡动画，动画时长为 0.5s，动画函数默认为 `ease`;
2. `overflow: hidden;` 防止隐藏元素的高度超出其父级容器；
3. `height: 0;` 将元素高度设置为0；
4. `height: var(--max-height);` 当元素父级被鼠标悬浮的时候，将其高度设置成为 `CSS` 变量 `--max-height` （在后续 `js` 中设置）

&emsp;&emsp; `JavaScript` 中主要使用到的代码如下：    
1. `el.scrollHeight` 获取到元素的实际高度，包括被隐藏的部分，这部分高度是根据元素内容来动态改变的；
2. `el.style.setProperty(...)` 设置 `--max-height` 用于指定 `height` 目标悬停在其上的元素的，允许它从 `0` 平滑过渡到设置的高度。