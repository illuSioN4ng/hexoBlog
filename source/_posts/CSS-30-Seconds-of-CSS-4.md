title: Donut spinner(环形加载器)
tags:
  - 30 Seconds of CSS
categories:
  - CSS
date: 2019-01-01 18:50:20
---


&emsp;&emsp;用来创建一个用来指示内容加载的环形加载器。`demo` 如下：     

<p data-height="265" data-theme-id="0" data-slug-hash="wRPOdL" data-default-tab="css,result" data-user="illuSioN4ng" data-pen-title="Donut spinner" class="codepen">See the Pen <a href="https://codepen.io/illuSioN4ng/pen/wRPOdL/">Donut spinner</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

&emsp;&emsp;`demo` 中的 `HTML` 代码如下，仅仅是一个 `div` ，类名为 `donut`：    
```html
<div class="donut"></div>
```

&emsp;&emsp;`demo` 中的 `CSS` 样式如下所示：    
```css
.donut {
  display: inline-block;
  border-radius: 50%;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-top-color: #7983ff;
  width: 30px;
  height: 30px;
  animation: donut-spin 1.2s linear infinite;
}

@keyframes donut-spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

&emsp;&emsp;使用半透明的 `border` 的 `div` 元素，仅有一个作为环形加载器的边框设置为其它颜色，同时使用 `CSS3` 中的 `animation` 进行旋转操作。   