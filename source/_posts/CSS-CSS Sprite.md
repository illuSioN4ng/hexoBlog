title: CSS Sprite（CSS雪碧图）
date: 2015-08-25 09:09:29
tags: [CSS]
categories: CSS 
---

今天看到慕课网分享的Mozilla的CSS开发者指南，当中谈到了CSS雪碧图，觉得有用，遂整理一下。

之所以用雪碧图，是因为一个网站若有很多的小图标，相对于将每个小图标以png的格式引用到页面上，是用雪碧图只需要引用一张图片，对于内存和带宽更加友好。

实现如下：

假设通过.btn的类，为该类添加一张背景图片：

    .btn {background:url(myfile.png); display:inline-block; height:20px; width:20px }  

背景的位置，可以通过在background的rul()中直接定义X,Y的值，或者通过background的属性来添加。例如：

    #btn1 {background-position: -20px 0px}  
    #btn2 {background:url(myfile.png) -20px 0px no-repeat}  
    
id为btn1和btn2的元素背景均左移20px.

类似的，你可以添加hover来改变背景：

    #btn:hover {background-position: [pixels shifted right]px [pixels shifted down]px;} 
  
详情见[CSS雪碧图](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/CSS_Image_Sprites) [完整Demo](https://css-tricks.com/snippets/css/perfect-css-sprite-sliding-doors-button/s)