title: 细线边框
tags:
  - 30 Seconds of CSS
categories:
  - CSS
date: 2019-01-17 09:04:48
---


&emsp;&emsp;为元素提供宽度与一个本地设备像素等同的边框，这样可以看起来十分锋利和清脆（应该是清晰？）。demo 如下：    

<p data-height="265" data-theme-id="0" data-slug-hash="PXgxve" data-default-tab="css,result" data-user="illuSioN4ng" data-pen-title="hairline border" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" class="codepen"><span>See the Pen <a href="https://codepen.io/illuSioN4ng/pen/PXgxve/">hairline border</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</span></p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## HTML
```html
<div class="hairline-border">text</div>
```

## CSS
```css
.hairline-border {
  box-shadow: 0 0 0 1px;
}
@media (min-resolution: 2dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.5px;
  }
}
@media (min-resolution: 3dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.33333333px;
  }
}
@media (min-resolution: 4dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.25px;
  }
}
```

## 描述
1. `box-shadow` 当仅适用扩展的时候，添加可以作为色子像素的伪边框。
2. 使用 `@media (min-resolution: ...)` 为了检查本地设备像素比(`1dppx` 等于`96 DPI` )，将 `box-shadow` 的分布设置为 `1 / dppx`