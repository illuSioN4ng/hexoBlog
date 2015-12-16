title: 《jQuery基础教程》第四版第五章课后练习
date: 2015-11-26 20:14:26
tags: [JavaScript,jQuery]
categories: jQuery 
---
 1. 修改添加back top链接的代码，以便这些链接只从第四段后面才开始出现。
 2. 在单击back to top链接时，为每个链接后面添加一个新段落，其中包含You were here字样。确保链接仍然有效。 
 3. 在单击作者名字时，把文本改为粗体(通过添加一个标签，而不是操作类或css属性)。
 4. **挑战**:在随后单击粗体作者名字时，删除之前添加的元素(也就是在粗体文本与正常文本之间切换)。
 5. **挑战**：为正文中的每一个段落添加一个inhabitants类，但不能调用.addClass()方法。确保不影响现有的类。
 ```
 //1
  var $pAfterThree = $('div.chapter p').eq(2).nextAll();
  $('<a href="#top">back to top</a>').insertAfter($pAfterThree);
  $('<a id="top"></a>').prependTo('body');
  
  //2
  $('a[href $= "#top"]').on('click', function(){
    $('<p>You were here</p>').insertAfter(this);
  })
  //4第一种方法(第三题包含在4的方法中)
  var flag = 0;
  $('#f-author').on('click', function() {
    if (flag == '0') {
      $(this).wrap("<b></b>");
      flag = 1;
    }else {
      $(this).unwrap();
      flag = 0;
    }
  });
  
  //4第二种方法
  //$('<a href="#top">back to top</a>').insertAfter('div.chapter p');
  //$('a[href$="#top"]').on('click', function(){
  //  $('<p>You were here</p>').insertAfter(this);
  //});

  //5
  $('p').each(function(){
    var a = $(this).attr('class');
    if(a == null){
      $(this).attr('class','inhabitants');
    }else{
      $(this).attr('class',a + ' inhabitants');
    };
  });
 ```
  [效果请点击](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter5/index.html)