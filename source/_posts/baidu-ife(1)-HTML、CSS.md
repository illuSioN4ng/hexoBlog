title: 百度web-ife前端学院-task1学习笔记
date: 2016-01-05 19:38:29
tags: [Learning Notes,HTML,CSS]
categories: Learning Notes 
---
1. 用两种方法来实现一个背景色为红色、宽度为960px的<DIV>在浏览器中居中
	实现思路为：一、利用设置左右margin为auto，自动居中；二、利用绝对定位和负边距来居中
	html主要代码如下
	```
	<div class="div1">实现一个背景色为红色、宽度为960px的盒子在浏览器中居中利用margin</div>
    <div class="div">
        <div class="div2 margin">实现一个背景色为红色、宽度为960px的盒子在浏览器中居中利用绝对定位</div>
    </div>
	```
	css主要代码如下：
	```
	.div1{
	    background-color: red;
	    width:960px;
	    height: 100px;
	    margin: 0px auto;
	}

	.div{
	    margin-top: 10px;
	    position: relative;
	}
	.div2{
	    background-color: red;
	    position: absolute;
	    left: 50%;
	}

	.margin{
	     width:960px;
	     height: 100px;
	     margin-left: -480px;
	}
	```
2. 有的圆角矩形是复杂图案，无法直接用border-radius，请在不使用border-radius的情况下实现一个可复用的高度和宽度都自适应的圆角矩形![示例](//img.blog.csdn.net/20160105193800331)
	实现思路为，利用几个高度为1px盒子的左右边线来画一个圆角
	html代码如下：
	```
	<div class="div4">
        <div class="top1"></div>
        <div class="top2"></div>
        <div class="top3"></div>
        <div class="top4"></div>
        <div class="top5"></div>
        <div class="main">
            有的圆角矩形是复杂图案，无法直接用border-radius，请在不使用border-radius的情况下实现一个可复用的高度和宽度都自适应的圆角矩形
        </div>
        <div class="bottom5"></div>
        <div class="bottom4"></div>
        <div class="bottom3"></div>
        <div class="bottom2"></div>
        <div class="bottom1"></div>
    </div>
	```

	CSS代码如下：
	```
	.div4{
	    margin-top: 50px;
	    overflow: hidden;
	}
	.top2, .bottom2, .top3, .bottom3, .top4, .bottom4, .top5, .bottom5{
	    height: 1px;
	    background: lightblue;
	    overflow: hidden;
	}
	.top1, .bottom1{
	    margin: 0px 5px;
	    border-top: solid 1px black;
	}
	.top2, .bottom2{
	    margin: 0px 3px;
	    border-left: solid 1px black;
	    border-right: solid 1px black;
	}
	.top3, .bottom3{
	    margin: 0px 2px;
	    border-left: solid 1px black;
	    border-right: solid 1px black;
	}
	.top4, .bottom4{
	    margin: 0px 1px;
	    border-left: solid 1px black;
	    border-right: solid 1px black;
	}
	.top5, .bottom5{
	    margin: 0px 1px;
	    border-left: solid 1px black;
	    border-right: solid 1px black;
	}
	.main{
	    height: 100%;
	    border-left: solid 1px black;
	    border-right: solid 1px black;
	    background: lightblue;
	}
	```
3. 用两种不同的方法来实现一个两列布局，其中左侧部分宽度固定、右侧部分宽度随浏览器宽度的变化而自适应变化 ![示例2](//img.blog.csdn.net/20160105193829567)
	实现思路为：一、浮动和margin；二、相对布局
	html代码如下：
	```
	<div>
        <p>用两种不同的方法来实现一个两列布局，其中左侧部分宽度固定、右侧部分宽度随浏览器宽度的变化而自适应变化</p>
        <p>第一种：基于float和margin</p>
    </div>
    <div class="div5">
        <div class="divA">divA</div>
        <div class="divB">divB</div>
        <div class="divC">divC</div>
    </div>
    <div>
        <p>用两种不同的方法来实现一个两列布局，其中左侧部分宽度固定、右侧部分宽度随浏览器宽度的变化而自适应变化</p>
        <p>第二种：基于绝对定位</p>
    </div>
    <div class="div6">
        <div class="divA1">divA</div>
        <div class="divB1">divB</div>
        <div class="divC1">divC</div>
    </div>
	```
	CSS代码如下：
	```
	.divA{
	    width: 100px;
	    height: 100px;
	    background-color: red;
	    float: left;
	}

	.divB{
	    height: 100px;
	    background-color: blue;
	    margin-left: 100px;
	}

	.divC{
	    height: 100px;
	    width: 100%;
	    background-color: khaki;
	}

	.div6{
	    position: relative;
	}
	.divA1{
	    width: 100px;
	    height: 100px;
	    background-color: red;
	    position: absolute;
	}

	.divB1{
	    height: 100px;
	    background-color: blue;
	    margin-left: 100px;
	}

	.divC1{
	    height: 100px;
	    width: 100%;
	    background-color: khaki;
	}
	```
4. 用两种不同的方式来实现一个三列布局，其中左侧和右侧的部分宽度固定，中间部分宽度随浏览器宽度的变化而自适应变化
	实现思路为：一、浮动和左右margin(注意盒子顺序)；二、浮动和负边距实现
	html代码如下：
	```
	<div class="div7">
        <div class="diva"></div>
        <div class="divc"></div>
        <div class="divb"></div>
    </div>

    <div class="div8">
        <div class="diva1"></div>
        <div class="divb1"></div>
        <div class="divc1"></div>
    </div>
	```
	CSS代码如下：
	```
	.diva{
	    float: left;
	    width: 100px;
	    height: 100px;
	    background-color: red;
	}

	.divb{
	    margin: 0px 100px;
	    background-color: blue;
	    height: 100px;
	}

	.divc{
	    float: right;
	    width: 100px;
	    height: 100px;
	    background-color: red;
	}

	.div8{
	    margin-top: 50px;
	    padding: 0 100px 0 100px;
	}
	.diva1{
	    float: left;
	    width: 100%;
	    height: 100px;
	    background: red;
	}

	.divb1{
	    float: left;
	    width: 100px;
	    height: 100px;
	    margin-left: -100%;
	    position: relative;
	    left: -100px;
	    background: blue;
	}

	.divc1{
	    float: left;
	    width: 100px;
	    height: 100px;
	    margin-left: -100px;
	    position: relative;
	    right: -100px;
	    background: blue;
	}
	```
5. 实现一个浮动布局，红色容器中每一行的蓝色容器数量随着浏览器宽度的变化而变化 ![示例三](//img.blog.csdn.net/20160105194636987)![示例四](//img.blog.csdn.net/20160105194714517)
	html代码如下：
	```
	<div class="clr"></div>
    <div class="div9">
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="flexbox"></div>
        <div class="clr"></div>
    </div>
	```
	CSS代码如下：
	```
	.div9{
	    background-color: red;
	    overflow: hidden;
	    margin-top: 50px;
	}

	.flexbox{
	    float: left;
	    margin: 10px 10px;
	    width: 300px;
	    height: 300px;
	    background-color: blue;
	}

	.clr{
	    clear: both;
	}
	```
	[以上效果可点击查看](//www.cdyjy.uestc.edu.cn/uestc_la/baidu-ife/task1/task0001.html)
