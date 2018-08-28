title: 基于SVG path的弧形文字
date: 2018-08-28 17:27:46
tags: [CSS]
categories: CSS
---
&emsp;&emsp;最近在做课堂互动的演示版本的时候，遇到了一个简单的需求，即教师端点击表扬按钮的时候，学生端需要收到如下的一个效果：    
![表扬](http://ww1.sinaimg.cn/large/8c55dc23gy1fupotdkqsrj207606tdgw.jpg)    
&emsp;&emsp;虽然某些Javascript插件中可以实现类似的效果，不过这些脚本需要把所有的文字包裹在独立的 `<span></span>` 中，在分别将各个 `span` 旋转定位，从而形成，这时候就想到《CSS揭秘》中提到的SVG原生支持以任何路径排队的文字，所以环形文字和弧形文字都非常简单来实现了。    
## 基本实现思路
&emsp;&emsp;在SVG中，让文本按照路径排列的基本方法就是使用 `<textpath>` 来包裹住这段文本，再把它们装进一个 `<text>` 元素中，这个 `<textpath>` 标签还需要在它的 `id` 属性中引用一个 `<path>` 元素，然后就可以用这个 `<path>` 定义我们想要的路径。    

<iframe height='265' scrolling='no' title='LJZreG' src='//codepen.io/illuSioN4ng/embed/LJZreG/?height=265&theme-id=0&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/illuSioN4ng/pen/LJZreG/'>LJZreG</a> by illuSioN4ng (<a href='https://codepen.io/illuSioN4ng'>@illuSioN4ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

&emsp;&emsp;如上图，我们初始化路径为一个黑色的圆，其中我们使用viewBox来定义它的大小，它可以自适应外层容器的尺寸。    
&emsp;&emsp;弧形路径用到的参数比较多， `a rx ry x-axis-rotation large-arc-flag sweep-flag dx dy` ，弧形命令A的前两个参数分别是x轴半径和y轴半径，  `x-axis-rotation` 为x轴旋转角度
`large-arc-flag`（角度大小） 和`sweep-flag`（弧线方向）， `large-arc-flag` 决定弧线是大于还是小于180度，0表示小角度弧，1表示大角度弧。 `sweep-flag` 表示弧线的方向，0表示从起点到终点沿逆时针画弧，1表示从起点到终点沿顺时针画弧。所以上述 `path` 命令是先移动到点（0, 50）,顺时针画一个黑色的圆（z是闭合路径）。     

&emsp;&emsp;效果如下：    
<iframe height='265' scrolling='no' title='MqeXvZ' src='//codepen.io/illuSioN4ng/embed/MqeXvZ/?height=265&theme-id=0&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/illuSioN4ng/pen/MqeXvZ/'>MqeXvZ</a> by illuSioN4ng (<a href='https://codepen.io/illuSioN4ng'>@illuSioN4ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

&emsp;&emsp;样式加上
```css
.circular path {
  fill: none;
}
.circular svg {
  overflow: visible;
  margin: 3em;
}
```

<iframe height='265' scrolling='no' title='YOWveN' src='//codepen.io/illuSioN4ng/embed/YOWveN/?height=265&theme-id=0&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/illuSioN4ng/pen/YOWveN/'>YOWveN</a> by illuSioN4ng (<a href='https://codepen.io/illuSioN4ng'>@illuSioN4ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

&emsp;&emsp;知道怎么画环形文字之后，弧形文字就简单了，直接上代码吧：    

<iframe height='265' scrolling='no' title='环形文字' src='//codepen.io/illuSioN4ng/embed/xJoEYm/?height=265&theme-id=0&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/illuSioN4ng/pen/xJoEYm/'>环形文字</a> by illuSioN4ng (<a href='https://codepen.io/illuSioN4ng'>@illuSioN4ng</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

&emsp;&emsp;大概思路就是这样啦，贝塞尔曲线路径具体的属性含义如下：     

```
C x1 y1, x2 y2, x y (or c dx1 dy1, dx2 dy2, dx dy)
```    
&emsp;&emsp;最后一个坐标(x,y)表示的是曲线的终点，另外两个坐标是控制点，(x1,y1)是起点的控制点，(x2,y2)是终点的控制点。

&emsp;&emsp;以上。