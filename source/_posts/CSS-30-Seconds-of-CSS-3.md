title: 自定义滚动条样式
tags:
  - 30 Seconds of CSS
categories:
  - CSS
date: 2019-01-01 09:38:08
---

&emsp;&emsp;滚动条样式，在 `chrome` 中我们经常会使用到。
1. `::-webkit-scrollbar` 指向整个滚动元素
2. `::-webkit-scrollbar-track` 指向整个滚动条轨道
3. `::-webkit-scrollbar-thumb` 指向滑动块    


<p data-height="265" data-theme-id="0" data-slug-hash="pqdxXz" data-default-tab="html,result" data-user="illuSioN4ng" data-pen-title="Custom scrollbar" class="codepen">See the Pen <a href="https://codepen.io/illuSioN4ng/pen/pqdxXz/">Custom scrollbar</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

&emsp;&emsp;HTML demo如下：
```html
<div class="custom-scrollbar">
  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit.<br />
    Iure id exercitationem nulla qui repellat laborum vitae, <br />
    molestias tempora velit natus. Quas, assumenda nisi. <br />
    Quisquam enim qui iure, consequatur velit sit?
  </p>
</div>
```

&emsp;&emsp;CSS 样式如下：
```css
.custom-scrollbar {
  height: 70px;
  overflow-y: scroll;
}
/* To style the document scrollbar, remove `.custom-scrollbar` */
.custom-scrollbar::-webkit-scrollbar {
  width: 8px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
}
```

&emsp;&emsp;以上就是滚动条样式在 `chrome` 中的使用。