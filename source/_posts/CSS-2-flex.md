title: flex基础
date: 2018-12-10 20:22:24
tags: [CSS]
categories: CSS
---
&emsp;&emsp;**CSS3 弹性盒子**(**Flexible Box** 或 **Flexbox**)，是一种用于在页面上布置元素的布局模式，使得当页面布局必须适应不同的屏幕尺寸和不同的显示设备时，元素可预测地运行。对于许多应用程序，弹性盒子模型提供了对块模型的改进，因为它不使用浮动，flex容器的边缘也不会与其内容的边缘折叠。    
&emsp;&emsp;在定义方面来说，弹性布局是指通过调整其内元素的宽高，从而在任何显示设备上实现对可用显示空间最佳填充的能力。弹性容器扩展其内元素来填充可用空间，或将其收缩来避免溢出。    
## 基本概念
&emsp;&emsp;如下图中是一个 `flex-direction` 属性为 `row` 的弹性容器，意味着其内的弹性项目将根据既定书写模式沿主轴水平排列，其方向为元素的文本流方向，在这个例子里，为从左到右。
![flexbox](/img/css/flexbox.png)    
### 弹性容器（Flex Container）
&emsp;&emsp;包含着弹性项目的父元素。通过设置 display 属性的值为 flex 或 inline-flex 来定义弹性容器。    
#### 弹性容器的属性
&emsp;&emsp;弹性容器的属性主要有一下六个：`flex-direction`、`flex-wrap`、`flex-flow`、`justify-content`、`align-items`、`align-content`。    

##### `flex-direction` 属性
&emsp;&emsp;flex-direction属性决定主轴的方向（即项目的排列方向）。
```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
&emsp;&emsp;它可能有四个值：
  - `row（默认值）`：主轴为水平方向，起点在左端。
  - `row-reverse`：主轴为水平方向，起点在右端。
  - `column`：主轴为垂直方向，起点在上沿。
  - `column-reverse`：主轴为垂直方向，起点在下沿。    

##### `flex-wrap` 属性
&emsp;&emsp;默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap` 属性定义，如果一条轴线排不下，如何换行。    
```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
&emsp;&emsp;它可能有三个值：
  - `nowrap（默认）`：不换行
  - `wrap`：换行，第一行在上方
  - `wrap-reverse`：换行，第一行在下方    
  
##### `flex-flow` 属性
`flex-flow` 属性是`flex-direction` 属性和flex-wrap属性的简写形式，默认值为row nowrap。   
```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

##### `justify-content` 属性
&emsp;&emsp;`justify-content` 属性定义了项目在主轴上的对齐方式。   
```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
``` 
&emsp;&emsp;它可能取5个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右: 
  - `flex-start`（默认值）：左对齐
  - `flex-end`：右对齐
  - `center`： 居中
  - `space-between`：两端对齐，项目之间的间隔都相等。
  - `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。    
  

##### `align-items`属性
&emsp;&emsp;`align-items`属性定义项目在交叉轴上如何对齐。    
```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```
&emsp;&emsp;它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下:
  - `flex-start`：交叉轴的起点对齐。
  - `flex-end`：交叉轴的终点对齐。
  - `center`：交叉轴的中点对齐。
  - `baseline`: 项目的第一行文字的基线对齐。
  - `stretch（默认值）`：如果项目未设置高度或设为auto，将占满整个容器的高度。    

##### `align-content`属性
`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
&emsp;&emsp;该属性可能取6个值：    
  - `flex-start`：与交叉轴的起点对齐。
  - `flex-end`：与交叉轴的终点对齐。
  - `center`：与交叉轴的中点对齐。
  - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
  - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
  - `stretch（默认值）`：轴线占满整个交叉轴。

### 弹性项目（Flex item）
&emsp;&emsp;弹性容器的每个子元素都称为弹性项目。弹性容器直接包含的文本将被包覆成匿名弹性单元。    
#### 弹性项目的属性
&emsp;&emsp;一下6个属性设置在项目上: `order`、`flex-grow`、`flex-shrink`、`flex-basis`、`flex`、`align-self`。     
##### `order`属性
&emsp;&emsp;`order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。    
```css
.item {
  order: <integer>;
}
```

##### flex-grow属性
flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
```css
.item {
  flex-grow: <number>; /* default 0 */
}
```
&emsp;&emsp;如果所有项目的`flex-grow` 属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的 `flex-grow` 属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。    
##### flex-shrink属性
&emsp;&emsp;flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```
&emsp;&emsp;如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。    
&emsp;&emsp;**负值对该属性无效**。    
##### `flex-basis`属性
&emsp;&emsp;`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（`main size`）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。
```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```
&emsp;&emsp;它可以设为跟`width`或`height`属性一样的值（比如`350px`），则项目将占据固定空间。    

##### `flex`属性
&emsp;&emsp;`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。
```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
&emsp;&emsp;该属性有两个快捷值：`auto (1 1 auto)` 和 `none (0 0 auto)`。    
&emsp;&emsp;建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。    

##### `align-self`属性
&emsp;&emsp;`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。    
```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
&emsp;&emsp;该属性可能取6个值，除了`auto`，其他都与`align-items`属性完全一致。

  
### 轴（Axis）
&emsp;&emsp;每个弹性框布局包含两个轴。弹性项目沿其依次排列的那根轴称为**主轴(main axis)**。垂直于主轴的那根轴称为**侧轴(cross axis)**。
  - `flex-direction` 确立主轴。
  - `justify-content` 定义了在当前行上，弹性项目沿主轴如何排布。
  - `align-items` 定义了在当前行上，弹性项目沿侧轴默认如何排布。
  - `align-self` 定义了单个弹性项目在侧轴上应当如何对齐，这个定义会覆盖由 `align-items` 所确立的默认值。
### 方向（Direction）
&emsp;&emsp;弹性容器的**主轴起点(`main start`)**/**主轴终点(`main end`)**和**侧轴起点(`cross start`)**/**侧轴终点(`cross end`)**描述了弹性项目排布的起点与终点。它们具体取决于弹性容器的主轴与侧轴中，由 writing-mode 确立的方向（从左到右、从右到左，等等）。    
- `order` 属性将元素与序号关联起来，以此决定哪些元素先出现。
- `flex-flow` 属性是 `flex-direction` 和 `flex-wrap` 属性的简写，决定弹性项目如何排布。    

### 行（Line）
&emsp;&emsp;根据 `flex-wrap` 属性，弹性项目可以排布在单个行或者多个行中。此属性控制侧轴的方向和新行排列的方向。    

## 尺寸(Dimension)    
&emsp;&emsp;根据弹性容器的主轴与侧轴，弹性项目的宽和高中，对应主轴的称为**主轴尺寸(main size)** ，对应侧轴的称为 **侧轴尺寸(cross size)**。
- `min-height` 与 `min-width 属性初始值将为 0。
- `flex` 属性是 `flex-grow`、`flex-shrink` 和 `flex-basis` 属性的简写，描述弹性项目的整体的伸缩性。

## 不影响弹性盒子的属性
&emsp;&emsp;由于弹性盒子使用了不同的布局算法，某些属性用在弹性容器上没有意义：
- 多栏布局模块的 `column-*` 属性对弹性项目无效。
- `float` 与 `clear` 对弹性项目无效。使用 `float` 将使元素的 `display` 属性计为`block`。
- `vertical-align` 对弹性项目的对齐无效。

