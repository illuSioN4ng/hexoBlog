title: 《jQuery基础教程》第四版第二章课后练习
date: 2015-10-08 19:48:29
tags: [JavaScript,jQuery]
categories: jQuery
toc: false 
---
最近抽时间系统的学习下jQuery相关知识，看看《jQuery基础教程》。顺便把课后练习的代码整理发上来看看。

1. **给位于嵌套列表第二个层次的所有`<li>`元素添加special类；**
2. **给位于表格第三列的所有单元格添加year类；**
3. **为表格中包含文本Tragedy的第一行添加special类；**
4. ***挑战：选择包含链接(`<a>`)的所有列表项(`<li>`元素)，为每个选中的列表项的同辈列表项元素添加afterlink类;**
5. **挑战：为与.pdf链接最接近的祖先元素`<ul>`添加tragedy类**

-------------------

```
$(document).ready(function() {
  $('#selected-plays > li > ul > li').addClass('special');
  //$('#selected-plays ul > li').addClass('special');
  //$('#selected-plays ul ul > li').removeClass('special');
  $('tr').find('td:eq(2)').addClass('year');
  $('td:contains(Tragedy)').parent().filter('tr:eq(0)').addClass('special');
  //$('tr:contains(Tragedy)').filter('tr:eq(0)').addClass('special');
  //$('a').parent().parent().children().not('li:has(a)').addClass('afterlink');
  $('a').parent().siblings().not('li:has(a)').addClass('afterlink');
  console.log($('#email').parent().siblings());
  $('a[href$=".pdf"]').parents('ul:eq(0)').addClass('tragedy');
});
```
[效果请点击](http://www.cdyjy.uestc.edu.cn/uestc_la/jQuery/chapter2/index.html)