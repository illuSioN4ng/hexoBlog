title: Ajax从本地文件获取文本
date: 2015-09-07 10:10:29
tags: [JavaScript,Ajax]
categories: JavaScript 
---
今天学习了Ajax从本地文件获取文本。遇到了些问题，在网络上都找到了解决办法。
先把源码都贴上来吧

```
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>First Ajax Script</title>  
    <link rel="stylesheet" href="./ajaxTextFile.css">  
    <script src="./ajaxCreateRequest.js"></script>  
    <script>  
        function getText(url){  
            var ajaxRequest = CreateRequestObject();  
            if(ajaxRequest != false){  
                ajaxRequest.onreadystatechange = function(){  
                    if(ajaxRequest.readyState == 4){  
                        if(ajaxRequest.status == 200 || ajaxRequest.status == 0){  
                            // alert("4");  
  
                            document.getElementById("data").innerHTML = ajaxRequest.responseText;  
                        }else{  
                            alert("There was a problem with the request.");  
                        }  
                    }  
                }//回调函数结束  
            }  
            ajaxRequest.open("GET", url, true);  
            ajaxRequest.setRequestHeader('If-Modified-Since', 'Sat, 03 Jan 2010 00:00:00GMT');//处理缓存？？  
            ajaxRequest.send(null);  
        }  
    </script>  
</head>  
<body>  
    <span style="cursor:pointer; text-decoration:underline" onclick="getText('ajaxText.txt');">Fetch Text from a file</span>  
    <p><div id="data" class="divStyle"></div></p>  
</body>  
</html>  
```
ajaxCreateRequest.js代码如下：
```
function CreateRequestObject(){  
    var ajaxRequest;  
    try{  
        //IE 8.0+, Firefox, Safari  
        ajaxRequest = new XMLHttpRequest();  
    }catch(e){//internet EXplore  
        try{  
            ajaxRequest = new ActiveXObject("Msxml2.XMLHTTP");  
              
        }catch(e){  
            try{//code for ie5 and ie6  
                ajaxRequest = new ActiveXObject("Microsoft.XMLHTTP");  
            }catch(e){  
                return false;  
            }  
        }  
    }  
    return ajaxRequest;  
}  
```
CSS代码：
```
body{  
    background: #ccc;  
}  
  
.divStyle{  
    border: 1px solid blue;  
    font-size: 100%;  
    width: 800px;  
    min-height:50px;  
}  
```
点击前：

![前端截图1](//img.blog.csdn.net/20150907101701663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

点击后：
![前端截图2](//img.blog.csdn.net/20150907101728676?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

下面说下我遇到的问题。
1. 乱码的问题。
![问题截图1](//img.blog.csdn.net/20150907101844569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
起初以为是页面编码的问题，把utf-8改成gbk,gb2312均没有效果。后来想到了应该是txt文件本身的编码，查看了下果然是记事本默认编码是ANSI（搞不懂什么鬼~~~~(>_<)~~~~），改成了utf-8之后重新保存，读取就正常了。
2. Firefox正常，搜狗、chrome、ie均不能读取。
chrome提示如图：
![问题截图2](//img.blog.csdn.net/20150907102343690?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
在网上找到一篇分享，说是chrome不支持本地的异步请求。
如何在chrome上面也能得到相同的执行，所以需要在Chrome的快捷方式后面添加：--allow-file-access-from-files 即可（注意前面需要额外加一个空格，否则会报错。）
如我在本机上的命名是：C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chrome.exe --allow-file-access-from-files
结果就正常了。
![处理后截图](//img.blog.csdn.net/20150907102707843?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

本地file浏览不要用webkit核心浏览器，要用firefox。否则就要搭建服务器后通过http访问。
