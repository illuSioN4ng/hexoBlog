title: JSHOP-【绩效系统】总结
date: 2016-06-24 14:35:28
tags: [JD实习]
categories: Learning Notes
---
本文主要分为两部分，第一部分为相关小技术的总结；第二部分为kpi系统的工程构建相关。
## tips
1. 首页动画+icon相关（icomoon）    
[icomoon](https://icomoon.io/)是一个在线图片转icon生成网站，不过只是支持SVG images, SVG fonts 或者JSONs（之前的svg转icon数据）    
之前一直知道图标字体，也没有机会去实际运用到开发环境中去，这次数据中心个人首页中的基础得分和附加得分的仪表盘的样式中就使用到了一种字体，最后决定用图标字体来显示数字样式。这次使用到的svg文件还是找宇哥帮忙用AI做的，自己没有用过AI，对PS的使用也是很基础，以后可能需要向宇哥和江哥学习相关的软件使用啦，前端还是不能仅仅局限在写代码上面，要和江哥一样，相关软件都要有涉猎，这样在工作中才能运用自如。
2. 弹框垂直水平居中问题
    建议用`tansform:translate`来做，可以自适应大小。（PS：最近做CRM店铺会员页以及JSHOP线上问题支持的过程中，遇到了蛮多相关的定位的问题，因为活动页需要支持低版本IE，所以还是建议用绝对定位+margin负边距来做。**使用场景需要根据项目中的要求来看**）
3. css3弹框动画    
用到了css3的animation和@keyframes
4. [图片modal](http://codepen.io/illuSioN4ng/pen/rLvZjG)    
主要思路来自[菜鸟教程](http://www.runoob.com/css3/css3-images.html)，加上了点击悬浮层关闭modal的js（**codepen中没有考虑浏览器兼容性**）
5. Ajax相关
前后端联调接口的过程中，遇到一些报错，但是因为我用的是`$.get`等封装好的方法，不能够直观的看到错误信息（借助开发者工具也是能够看到的），后来采用`$.ajax`中监控`error`来查看错误信息，这样更加直观。
6. 图片上传相关h5
用到了元哥的H5上传插件，来杜绝`swfupload`插件的火狐不兼容的问题。
7. 下载get数据超限改为post（iframe拼接）
在绩效系统中遇到了这样的需求，导出所有成员的的得分信息，得分信息的查表以及计算比较复杂，耗时比较长，所以避免二次查表，所以需要从前端读取用户得分信息直接发给后端，从而保存为excel。但是当用户数据超过一定范围的时候，`get`请求会报错（window.open），所以采用`post`来替代。基本思想是：用页面一个隐藏的form表单来存储页面的数据，然后提交到页面中的一个隐藏的iframe中去，这样就可以实现无刷新的当前页下载。实现JS代码主要如下：
```
var url =  "/scoreInfo/exportScoresExcel.html"+"?year="+year+"&quarter="+season,
            downWin = $('#J_DownloadWindow'),
            downForm = $('#J_DownloadForm');
        if(!downWin.length){
            downWin = $('<iframe>');
            downWin.attr({'id' : 'J_DownloadWindow' ,'name' : 'J_DownloadWindow'}).css('display', 'none').appendTo('body');
        }
        downWin.attr('src', url);
        $('#J_DownloadInput').val(JSON.stringify(scoreStatistic));
        downForm.attr('action', url)[0].submit();
```
8. height、min-height、max-height
这里遇到的问题，是和宇哥艳玲姐开始对项目还原度的时候发现的。当leader新增的打分项或者是评分详情内容过长可能会造成页面布局溢出的情况。这里的问题提还是因为自己不够成熟，没有经验，后来修改的时候耗时也蛮多的，而且会经常遗漏某些地方，**设计稿永远是最完美的应用场景，作为前端还是需要考虑到最极端的应用场景，**这点是我以后最需要注意的地方。
9. 缩略图（江哥写）
这个需求是我那天学校有事儿，请假回学校了，江哥帮忙处理了下，不过江哥随手写的一个jquery小插件还是有很多我需要学习的地方。以前我的思维总是局限到直接页面的主JS文件或者单独引入一个JS文件，来实现某个功能，而没有想到用jquery插件，以后需要多学习江哥的实现方式，还有jquery插件的一般规范。
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
以后的项目中最好还是前后端分离，即便不能分离，必须要去写vm，也要是前端去写，这样会避免很多坑。    