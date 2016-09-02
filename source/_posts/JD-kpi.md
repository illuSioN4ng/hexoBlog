title: JSHOP-【绩效系统】总结
date: 2016-06-24 14:35:28
tags: [JD实习]
categories: Learning Notes
---
本文主要分为两部分，第一部分为相关小技术的总结；第二部分为kpi系统的工程构建相关。
## tips
1. 首页动画+icon相关（icomoon）    
[icomoon](https://icomoon.io/)是一个在线图片转icon生成网站，不过只是支持SVG images, SVG fonts 或者JSONs（之前的svg转icon数据）
2. 弹框垂直水平居中问题
    建议用`tansform:translate`来做，可以自适应大小
3. css3弹框动画    
4. [图片modal](http://codepen.io/illuSioN4ng/pen/rLvZjG)    
主要思路来自[菜鸟教程](http://www.runoob.com/css3/css3-images.html)，加上了点击悬浮层关闭modal的js（**codepen中没有考虑浏览器兼容性**）
5. Ajax相关 + 多次上传问题
6. 图片上传相关h5
7. 下载get数据超限改为post（iframe拼接）
8. height、min-height、max-height
9. 缩略图（江哥写）
10. [tip三角样式](http://codepen.io/illuSioN4ng/pen/xOjVPQ)

## jd_kpi_2016 project
### 工程目录
#### vm文档相关
1. /firstPage(成员得分公示)
2. /layout(页面公用css、js、头尾等vm)
3. /man(man端超级管理员管理部门经理相关vm) - 后端：周国鑫
4. /scoreInfo(打分相关vm) - 后端：周国鑫
5. /sys(部门、成员管理) - 后端：柏雪峰
6. index.vm(普通成员得分首页)

### kpi系统相关感受
该工程没有完全的前后端分离，导致了后端同学修改部分前端代码到vm中导致页面一些冲突（比如id不唯一，容器定位不准确的一些坑）。  
以后的项目中最好还是前后端分离，即便是必须要去写vm，也要是前端去写，这样会避免很多坑。    