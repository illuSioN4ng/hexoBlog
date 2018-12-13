title: Javascript实现幻灯片效果
date: 2015-08-24 14:44:29
tags: [JavaScript]
categories: JavaScript 
---
Web上很多页面都有提供javascript幻灯效果的代码库，我们可以免费直接使用很多优秀的代码库。今天我开始学习用JavaScript实现幻灯效果，新手一步一步开始学起。

在创建一个幻灯效果的时候，页面的访客是我们需要首要考虑的因素。其中包括访问者的带宽、浏览器的类型、在下载大图片的时候是否需要漫长的下载时间。导航是第二个重要因素，访客是手动进行控制幻灯还是页面加载完成后自动播放，退出时自动停止播放还是自动循环固定次数后停止？我们可能还会想创建控制按钮，像开始、结束、暂停、前进、后退、加速播放、减速播放等。还会想在图片上创建超链接和可单击的图片，每帧图片都连接到不同的URL。甚至会想随机地显示图片或者按照某种顺序来显示。（摘自JavaScript详解）
下面就介绍一种我今天刚刚学习的最简单的带有按钮的幻灯展示。

    <div style="text-align:center;">  
        <br>  
        <img src="./images/1.jpg" alt="" name="pic" height="192" width="108">  
        <br>  
        <input type="button" value="start Show" onClick="return startSlideShow();">  
        <input type="button" value="stop Show" onClick="return stopSlideShow();">  
    </div>  

上面代码为html主体，包括一个images和两个按钮，点击分别实现幻灯片开始和停止的功能。

    function preLoadImages(){  
        if(document.images){  
            pictures = new Array();  
            pictures[0] = new Image();  
            pictures[0] = "./images/1.jpg";  
            pictures[1] = new Image();  
            pictures[1] = "./images/2.jpg";  
            pictures[2] = new Image();  
            pictures[2] = "./images/3.jpg";  
            pictures[3] = new Image();  
            pictures[3] = "./images/6.jpg";  
        }else{  
            alert("There are no images to preload!");  
        }  
    }  

上面是preLoadImages函数，在HTML文档head部分定义该函数来预加载图像列表。脚本运行之前，使用该函数在浏览器的缓存中存储一些必须的照片，这样做可以减少访客的等待时间。

    var index = 0;  
     function startSlideShow(){  
         if(index < pictures.length){  
             document.images["pic"].src = pictures[index];  
             index++;  
         }else{  
             index = 0;  
             document.images["pic"].src = pictures[index];  
             index++;  
         }  
         timeout = setTimeout(startSlideShow, 1500);  
     }  
       
     function stopSlideShow(){  
         clearTimeout(timeout);  
     } 

上面代码为startSlideShow和stopSlideShow函数，通过index索引值来遍历图片的地址，并用定时器设置1.5S显示一张图片。

该功能的实际效果见连接。[点击打开链接](//www.cdyjy.uestc.edu.cn/uestc_la/SlideShow/SlideShow.html "Demo")
