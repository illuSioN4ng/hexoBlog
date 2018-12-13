title: 《jQuery基础教程》第四版第七章课后练习
date: 2015-12-27 14:51:26
tags: [JavaScript,jQuery]
categories: jQuery 
---
 1. 把幻灯片的切换周期延长到1.5秒，把动画效果修改为下一张幻灯片淡入之前，前一张幻灯片淡出。请参考Cycle插件的文档，找到实现上述功能的选项。
 2. 设置名为cyclePaused的cookie，将它的有效期设置为30天。
 3. 限制书名区域，每次缩放只允许以10像素为单位。
 4. 修改滑动条动画，让幻灯片切换时，滑块从当前位置平滑地移动到下一个位置。
 5. 不像以前那样循环播放幻灯片，而是在播放完最后一张幻灯片后停止。当幻灯片停止播放时，也禁用相应的按钮和滑动条。
 6. 创建一个新的jQuery UI主题，让部件背景为浅蓝色，文本为深蓝色，并将这个自定义主题应用到我们的实例文档上。
 7. 修改mobile.html中的HTML代码，让列表视图根据书名的第一个字母分隔开来。可以参考jQuery Mobile官方中关于data-role=”list-divider“的介绍。
 
1~4修改的代码如下：
```
$(document).ready(function() {
	//题1 把幻灯片的切换周期延长到1.5秒，把动画效果修改为下一张幻灯片淡入之前，前一张幻灯片淡出。请参考Cycle插件的文档，找到实现上述功能的选项。
    var $books = $('#books').cycle({
    //timeout: 2000,
    timeout: 1500,//将timeout设置成1500
    fx: 'fade',// name of transition effect (or comma separated names, ex: 'fade,scrollUp,shuffle')
    speed: 200,
    pause: true,
    before: function() {
      $('#slider').slider('value', $('#books li').index(this));
    }
  });
	
	//题2 设置名为cyclePaused的cookie，将它的有效期设置为30
	$.cookie('cyclePaused', 'illuSioN4ng', { expires: 30 });
	alert($.cookie('cyclePaused'));//检查设置的cookie值是否成功(页面需要点击resume按钮，重置cookie，恢复轮播)

	$books.find('.title').resizable({
	//handles: 's'

    //题3 限制书名区域，每次缩放只允许以10像素为单位。
    grid: [10, 10]
  });
	$('<div id="slider"></div>').slider({
    min: 0,
    max: $('#books li').length - 1,

    //题4 修改滑动条动画，让幻灯片切换时，滑块从当前位置平滑地移动到下一个位置。
    animate: true,//Boolean: 当设置为 true时, 滑动手柄将以默认的持续时间执行动画。String: 速度的名称, 比如 "fast" 或 "slow"。Number: 动画持续时间, 以毫秒为单位。
    slide: function(event, ui) {
      $books.cycle(ui.value);
    }
  }).appendTo($controls);
});
```

题5修改的代码如下：
```
$(document).ready(function() {
    var stop_flag = 0;//停止标志符，0为正常轮播，1为停止
	var $books = $('#books').cycle({
		timeout: 1500,//将timeout设置成1500
	    fx: 'fade',// name of transition effect (or comma separated names, ex: 'fade,scrollUp,shuffle')
	    speed: 200,
	    nowrap: true,// true to prevent slideshow from wrapping
	    pause: true,
	    before: function() {
	      $('#slider').slider('value', $('#books li').index(this));
	    },

	    end: function(){
	      $('button').click(function(event){
	        $(this).effect('shake', {
	          distance: 5
	        });
	      });
	      stop_flag = 1;
	    }//end回掉函数可以和autostop或者nowrap配合使用
	  });
  if ( $.cookie('cyclePaused') ) {
    $books.cycle('pause');
  }
  var $controls = $('<div id="books-controls"></div>').insertAfter($books);
  $('<button>Pause</button>').click(function(event) {
    if(stop_flag === 0){//判断停止标识符
      event.preventDefault();
      $books.cycle('pause');
      $.cookie('cyclePaused', 'y');
    }else {//禁用按钮
      $(this).effect('shake', {
        distance: 10
      });
    }
  }).button({
    icons: {primary: 'ui-icon-pause'}
  }).appendTo($controls);
  $('<button>Resume</button>').click(function(event) {
    if(stop_flag === 0){//判断停止标识符
      event.preventDefault();
      var $paused = $('ul:paused');
      if ($paused.length) {
        $paused.cycle('resume');
        $.cookie('cyclePaused', null);
      }else {//禁用按钮
        $(this).effect('shake', {
          distance: 10
        });
      }
    }
  }).button({
    icons: {primary: 'ui-icon-play'}
  }).appendTo($controls);

  $('<div id="slider"></div>').slider({
    min: 0,
    max: $('#books li').length - 1,

    animate: true,//Boolean: 当设置为 true时, 滑动手柄将以默认的持续时间执行动画。String: 速度的名称, 比如 "fast" 或 "slow"。Number: 动画持续时间, 以毫秒为单位。
    slide: function(event, ui) {
      if(stop_flag === 0){//判断停止标识符
        $books.cycle(ui.value);
      }else {//禁用滑动条
        $(this).effect('shake', {
          distance: 10
        });
      }
    }
  }).appendTo($controls);
});
```
[效果请点击](//www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter7/index.html)

题6 需要先到//jqueryui.com/themeroller/上制作样式并下载，然后应用相应的文件

题7 的代码如下：
 方法一：
```
 <div data-role="content">
    <ul data-role="listview" data-inset="true" data-filter="true" data-autodividers="true">
      <li><a href="#drupal-7">Drupal 7 Development by Example</a></li>
      <li><a href="#jq-game">jQuery Game Development Essentials</a></li>
      <li><a href="#jqmobile-cookbook">jQuery Mobile Cookbook</a></li>
      <li><a href="#jquery-designers">jQuery for Designers</a></li>
      <li><a href="#jquery-hotshot">jQuery Hotshot</a></li>
      <li><a href="#jqui-cookbook">jQuery UI Cookbook</a></li>
      <li><a href="#mobile-apps">Creating Mobile Apps with jQuery Mobile</a></li>
      <li><a href="wp-mobile-apps.html">WordPress Mobile Applications with PhoneGap</a></li>
    </ul>
  </div>
```
 [效果请点击](//www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter7/mobile.html)
 方法二： 
```
 <div data-role="content">
    <ul data-role="listview" data-inset="true" data-filter="true">
      <li data-role="list-divider">D</li>
      <li><a href="#drupal-7">Drupal 7 Development by Example</a></li>
      <li data-role="list-divider">J</li>
      <li><a href="#jq-game">jQuery Game Development Essentials</a></li>
      <li><a href="#jqmobile-cookbook">jQuery Mobile Cookbook</a></li>
      <li><a href="#jquery-designers">jQuery for Designers</a></li>
      <li><a href="#jquery-hotshot">jQuery Hotshot</a></li>
      <li><a href="#jqui-cookbook">jQuery UI Cookbook</a></li>
      <li data-role="list-divider">C</li>
      <li><a href="#mobile-apps">Creating Mobile Apps with jQuery Mobile</a></li>
      <li data-role="list-divider">W</li>
      <li><a href="wp-mobile-apps.html">WordPress Mobile Applications with PhoneGap</a></li>
    </ul>
  </div>
```
[效果请点击](//www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter7/mobile2.html)