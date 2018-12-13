title: 《jQuery基础教程》第四版第四章课后练习
date: 2015-11-19 15:39:29
tags: [JavaScript,jQuery]
categories: jQuery 
---

### 修改样式表，一开始先隐藏页面内容，当页面加载后，慢慢地淡入内容。

```
$(document).ready(function() {
	$('body').css('display', 'none');
	$('body').fadeIn(1500);//自行设置时间
});
```

### 在鼠标悬停到段落上面时，给段落应用黄色背景。
```
$(document).ready(function() {
	$('p').mouseover(function(){
		 $(this).css('backgroundColor', 'yellow');
	});//当鼠标移动到段落上时段落背景应用黄色。
	$('p').mouseout(function(){
		 $(this).css('backgroundColor', 'white');
	});//当鼠标移出段落上时段落背景应用白色。
});
```

### 单击标题使其不透明度变为25%，同时添加20px的左外边距，当这两个效果完成后，把讲话文本变成50%的不透明度。
 
```
$(document).ready(function() {
	$('h2').click(function(){//单击标题(<h2>)
		$(this)
			.fadeTo('slow', 0.25)
			.animate({
			  paddingLeft: '+=20' + 'px'//或者paddingLeft: '+=200px'
			},{
			  duration: 'slow',
			  queue: false
			})
			.queue(function(next){
			  $('div.speech').fadeTo('slow', 0.5)
			});
	});
});
```

### 挑战：按下方向键时，使样式转换器向相应的方向平滑移动20像素；四个方向键的键码分别是37(左)、38(上)、39(右)、40(下)。

```
  var key_left = 37;
  var key_up = 38;
  var key_right = 39;
  var key_down = 40;
  var $switcher = $('#switcher');
  $switcher.css('position', 'relative');
  $(document).keyup(function(event){
    switch(event.which){
      case key_left:
        $switcher
            .animate({
              left: '-=20px'
            },{
              duration: 'fast'
            })
        break;
      case key_up:
        $switcher
            .animate({
              top: '-=20px'
            },{
              duration: 'fast'
            })
        break;
      case key_right:
        $switcher
            .animate({
              left: '+=20px'
            },{
              duration: 'fast'
            })
        break;
      case key_down:
        $switcher
            .animate({
              top: '+=20px'
            },{
              duration: 'fast'
            })
    }
  });
```
[效果请点击](//www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter4/index.html)