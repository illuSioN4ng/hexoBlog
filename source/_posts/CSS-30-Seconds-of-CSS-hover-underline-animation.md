title: 悬停下划线动画
tags:
  - CSS-30-Seconds-of-CSS
categories:
  - CSS
date: 2019-01-18 20:10:40
---

&emsp;&emsp;今天来写一下很久以前就知道的悬停下划线的动画，效果如下：    

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="css,result" data-user="illuSioN4ng" data-slug-hash="gZNRqW" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="hover underline animation">
  <span>See the Pen <a href="https://codepen.io/illuSioN4ng/pen/gZNRqW/">
  hover underline animation</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 代码
&emsp;&emsp;`HTML` 代码很简单，仅仅只是一个 `div` 包裹层：    
```html
<p class="hover-underline-animation">
  Hover this text to see the effect!
</p>
```

&emsp;&emsp;`CSS` 代码如下：    
```css
.hover-underline-animation {
  position: relative;
  display: inline-block;
  color: #0087ca;
}

.hover-underline-animation:after {
  content: '';
  height: 2px;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  transform: scale(0);
  transform-origin: bottom right;
  background-color: #0087ca;
  transition: transform 0.25s ease-out;
}

.hover-underline-animation:hover:after {
  transform: scale(1);
  transform-origin: bottom left;
}
```

## 解析
1. `display: inline-block` 让块级元素 `p` 展示成为行内块，以防止下划线的宽度展示满满一行；
2. `position: relative` 是为了在元素上给伪元素建立笛卡尔坐标系；
3. `::after` 定义一个伪元素；
4. `position: absolute` 让伪元素脱离文档流，以其父级元素为标准相对定位；
5. `width: 100%` 确保伪元素跨越整个文本块；
6. `transform: scaleX(0)`初始时将伪元素缩放为0，不显示；
7. `bottom: 0 and left: 0` 定位到父级的左下角；
8. `transition: transform 0.25s ease-out` 代表在0.25s内通过 `ease-out` 时间函数过渡 `transform` 属性；
9. `transform-origin: bottom right` 表示变换锚点位于块的右下方;
10. `:hover::after` 然后使用 `scaleX(1)` t将宽度转变为 `100%,`， 再将`transform-origin` 改变为 `bottom left` 从而描点翻转, 从而允许悬停离开的时候可以反方向过渡。