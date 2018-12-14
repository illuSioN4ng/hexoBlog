title: Bouncing loader
tags:
  - 30 Seconds of CSS
categories:
  - CSS
date: 2018-12-14 14:58:33
---

&emsp;&emsp;最近可能是做业务做多了，反而对于样式类的东西感兴趣了。开始看`30 Seconds of CSS` 系列的代码。    
&emsp;&emsp;下面是一个创建跳跃的家在动画的代码。

## 效果
<p data-height="265" data-theme-id="0" data-slug-hash="jXWzWM" data-default-tab="result" data-user="illuSioN4ng" data-pen-title="Bouncing loader" class="codepen">See the Pen <a href="https://codepen.io/illuSioN4ng/pen/jXWzWM/">Bouncing loader</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 代码
&emsp;&emsp;HTML结构如下：    
```html
<div class="bouncing-loader">
  <div></div>
  <div></div>
  <div></div>
</div>
```

&emsp;&emsp;CSS核心代码如下：    
```css
@keyframes bouncing-loader {
  to {
    opacity: 0.1;
    transform: translate3d(0, -1rem, 0);
  }
}
.bouncing-loader {
  display: flex;
  justify-content: center;
}
.bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}
.bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}
.bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
```

## 解析
&emsp;&emsp;`1rem` 通常是 `16px` (根据网页的根元素来设置字体大小)。    
1. `@keyframes` 定义了两种状态， 分别是改变`opacity` 和 通过`transform: translate3d()` 来改变2D y方向的位置。很显然使用`transform: translate3d()`来开启硬件加速，提升动画性能。
2. `.bouncing-loader` 是跳动的小球的父级包裹层，使用 `display: flex` 和 `justify-content: center` 让子级居中。
3. `.bouncing-loader > div` 目的是选择的 `html` 中的三个 `div`，`div` 的长宽被设置为 `1rem` ， 设置 `border-radius: 50%;` 让它们变成从方形变成圆形。
4. `margin: 3rem 0.2rem` 是为了让三个 `div` 有一定的空间进行相关的动画，互不影响。
5. `animation` 是以下属性的简写: `animation-name`, `animation-duration`, `animation-iteration-count`, `animation-direction`（如果 `animation-direction` 值是 `alternate`，则动画会在奇数次数（1、3、5 等等）正常播放，而在偶数次数（2、4、6 等等）向后播放）。
6. `nth-child(n)` 是为了选择父级元素的第几个子元素.
7. `animation-delay` 的使用是为了让每个小球跳动动画有一定的时延。

> 参考
> 1. [Bouncing loader](https://30-seconds.github.io/30-seconds-of-css/)