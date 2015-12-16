title: 《jQuery基础教程》第四版第三章课后练习
date: 2015-10-08 20:12:29
tags: [JavaScript,jQuery]
categories: jQuery 
---
1. **在Charles Dickens被单击时，给它应用selected样式。**
2. **在双击章标题(`<h3 class=”chaper-title”>`)时，切换章文本的可见性。**
3. **当用户按下向右方向键时，切换到下一个body类；右方向键的键码是39.**
4. **挑战：使用console.log()函数记录在段落中移动的鼠标的坐标位置。（注意：console.log()可以在Firefox的firebug扩展、Safari的Web Inspector或Chrome、IE中的Developer Tools中使用。）**
5. **挑战：使用.mousedown()和.mouseup()跟踪页面中的鼠标事件。如果鼠标按键在按下它的地方被释放，则为所有段落添加hidden类。如果是在按下它的地方之下被释放的，删除所有段落的hidden类。**
_________________________

JavaScript代码如下所示：

```
$(document).ready(function() {
  var temp;
  // Enable hover effect on the style switcher
  $('#switcher').hover(function() {
    $(this).addClass('hover');
  }, function() {
    $(this).removeClass('hover');
  });

  // Allow the style switcher to expand and collapse.
  var toggleSwitcher = function(event) {
    if (!$(event.target).is('button')) {
      $('#switcher button').toggleClass('hidden');
    }
  };
  $('#switcher').bind('click', toggleSwitcher);

  // Simulate a click so we start in a collaped state.
  $('#switcher').click();

  // The setBodyClass() function changes the page style.
  // The style switcher state is also updated.
  var setBodyClass = function(className) {
    $('body').removeClass().addClass(className);

    $('#switcher button').removeClass('selected');
    $('#switcher-' + className).addClass('selected');
    //var temp = '#switcher-' + className;//标识选中的bodyClass
    //alert(temp);

    $('#switcher').unbind('click', toggleSwitcher);

    if (className == 'default') {
      $('#switcher').bind('click', toggleSwitcher);
    }
  };

  // begin with the switcher-default button "selected"
  $('#switcher-default').addClass('selected');
  var temp = '#switcher-default';
  // Map key codes to their corresponding buttons to click
  var triggers = {
    D: 'default',
    N: 'narrow',
    L: 'large'
  };

  // Call setBodyClass() when a button is clicked.
  $('#switcher').click(function(event) {
    if ($(event.target).is('button')) {
      var bodyClass = event.target.id.split('-')[1];
      temp = '#switcher-' + bodyClass;//标识选中的bodyClass
      setBodyClass(bodyClass);
    }
  });
  var i =0;
  // Call setBodyClass() when a key is pressed.
  $(document).keyup(function(event) {
    if (event.keyCode == 39){
      //if(temp=='#switcher-default')
      //  $('#switcher-default').click();
      if(temp == '#switcher-large')
        $('#switcher-default').click();
      else{
        $(temp).removeClass('selected').next().trigger('click');
      }
    }
    var key = String.fromCharCode(event.keyCode);
    if (key in triggers) {
      setBodyClass(triggers[key]);
    }
  });

  $('.author').click(function(){
    $(this).addClass('selected');
  });
  /*
  * 双击效果
  * */
  $('h3.chapter-title').dblclick(function(){
    //$(this).parent().children('p').toggleClass('hidden');//parent一定要加括号
    //$(this).siblings('p').toggleClass('hidden');
    $(this).nextAll('p').toggleClass('hidden');
  });
  /*
  * (4)挑战：使用console.log()函数记录在段落中移动的鼠标的坐标位置。
  * （注意：console.log()可以在Firefox的firebug扩展、
  * Safari的Web Inspector或Chrome、IE中的Developer Tools中使用。）
  * */
  $(document).mousemove(function(event){
    console.log('鼠标的位置为(x,y):（'+event.pageX+','+event.pageY+')');
  });

  /*
  * (5)挑战：使用.mousedown()和.mouseup()跟踪页面中的鼠标事件。
  * 如果鼠标按键在按下它的地方被释放，则为所有段落添加hidden类。
  * 如果是在按下它的地方之下被释放的，删除所有段落的hidden类。
  * */
  var up_X,up_Y,down_X,down_Y;
  $(document).mousedown(function(event){
    down_X = event.pageX;
    down_Y = event.pageY;
  });
  $(document).mouseup(function(event){
    up_X = event.pageX;
    up_Y = event.pageY;
    if(up_X == down_X && up_Y == down_Y){
      $('p').addClass('hidden');
    }else{
      $('p').removeClass('hidden');
    }
  });
});

```
[效果请点击](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter3/index.html)