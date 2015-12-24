title: 《jQuery基础教程》第四版第六章课后练习
date: 2015-12-23 20:56:26
tags: [JavaScript,jQuery]
categories: jQuery 
---
 1. 页面加载后，把exercises-content.html的主体(body)内容提取到页面的内容区域。
 2. 不要一次就显示整个文档，请为左侧的字母列表创建“提示条”，当用户鼠标放到字母上时，从exercises-content.html中加载与该字母有关的内容。
 3. 为页面加载添加错误处理功能，在页面的内容区显示错误消息。修改脚本，请求does-not-exist.html而不是exercises-content.html，以测试错误处理功能。
 4. 挑战：页面加载后，向GitHub发送一个JSONP请求，取得某个用户代码库的列表。把每个代码库的名称和URL插入到页面的内容区。取得jQuery项目代码库的URL是http://api.github.com/users/jquery/repos。(我用的是自己github的地址：http://api.github.com/users/la413972057/repos).

```
$(document).ready(function() {
//题1
  $('#dictionary').load('exercises-content.html .letter');
//题2
  $('h3').mouseover(function(){
    var letter_id = $(this).parent().attr('id');
    //alert(letter_id);
    $('#dictionary').load('exercises-content.html #' + letter_id);
  });
//题3
  $('h3').mouseover(function(){
    var letter_id = $(this).parent().attr('id');
    //alert(letter_id);
    $('#dictionary').load('does-not-exist.html #' + letter_id, function(response, status, xhr){
      if(status == 'error'){
        $('#dictionary').html('Sorry, but an error occurred: ' + xhr.status)
        .append(xhr.responseText);
      }
    });
  });
//题4
  $.getJSON('https://api.github.com/users/la413972057/repos', function(data){
    console.log(data);  
    var html = '';  
    $.each(data, function(jsonIndex, json_val){ 
      console.log(json_val);
      html += '<ul style="list-style-type:none;">' + (jsonIndex + 1);  
      html += '<li>name: ' + json_val.name + '</li>';  
      html += '<li>html_url: ' + json_val.html_url + '</li>';  
      html += '</ul>';  
    });  
    $('#dictionary').html(html);  
  });
});
```

[题1效果见链接](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter6/index1.html)
[题2效果见链接](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter6/index2.html)
[题3效果见链接](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter6/index3.html)
[题4效果见链接](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter6/index4.html)

