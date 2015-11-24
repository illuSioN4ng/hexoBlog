title: 如何用CSS画三角形
date: 2015-09-02 10:04
tags: [Learning Notes,CSS]
categories: Learning Notes
toc: false 
---

今天在微博上看到一个分享，说是如何画三角形，以前就学过，现在整理一下。
首先，写一个div，border设置成10px。
    ```
    #sty1{  
        height: 100px;  
        width: 100px;  
        background: red;  
        border-width: 10px;  
        border-style: solid;  
        border-color: black;  
    }  
    ```
<!--more-->
效果如图：
![效果图](http://img.blog.csdn.net/20150902100846899?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

border是分为上右下左的四个部分，这四个部分究竟如何分界的？现在将四部分均设置不同的颜色来看下。
    ```
    #sty1{  
        height: 100px;  
        width: 100px;  
        background: red;  
        border-width: 10px;  
        border-style: solid;  
        border-color: black green gray pink;  
    }  
    ```

效果如图：
![效果图](http://img.blog.csdn.net/20150902101103429?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

如果这时候我们将上左右的边界颜色设置成透明，
    ```
    #sty1{  
        height: 100px;  
        width: 100px;  
        background: red;  
        border-width: 10px;  
        border-style: solid;  
        border-color: black green gray pink;  
    }  
    ```
效果如图：
![效果图](http://img.blog.csdn.net/20150902101322739?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

这时候就出现了一个梯形，梯形的上边其实就是盒子模型的宽，下边就是盒子模型的宽+左右border的宽。高即为下边border的宽了。所以，只要我们将盒子的width设置成0，就可以看到一个三角形了。
    ```
    #sty1{  
        height: 0px;  
        width: 0px;  
        background: red;  
        border-width: 10px;  
        border-style: solid;  
        border-color: transparent transparent black transparent;  
    } 
    ```
效果如图：
![效果图](http://img.blog.csdn.net/20150902101322739?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

这个三角形的底边是左右border宽之和，高为下边border的宽。所以改变相应的宽度可以得到不同的三角形。

钝角三角形：
![效果图](http://img.blog.csdn.net/20150902101802180?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

直角三角形：
![效果图](http://img.blog.csdn.net/20150902101842135?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

今天微博上还有分享了一个三角形产生器，[大家可以点击进去看看](http://apps.eky.hk/css-triangle-generator/zh-hant)