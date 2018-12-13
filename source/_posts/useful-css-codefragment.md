title: 一些有用的css代码收集
date: 2016-07-30 20:57:35
tags: [Learning Notes,CSS]
categories: Learning Notes 
---
1. [css3水平垂直居中（效果请点击）](//codepen.io/illuSioN4ng/pen/EypbGJ)
```
//利用transform属性来做自适应水平垂直居中，常用在有蒙层的弹窗上（个人喜欢这种方式多余绝对定位方式）
.level-vertical-middle {
  position: relative;
  top:50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
```

2. [单侧阴影（效果请点击）](//codepen.io/illuSioN4ng/pen/akjVxg)
```
//至于为什么那样取值，请自行尝试，真诚脸:)
.box-shadow {
    position: absolute;
    background-color: #AC92EC;
    width: 160px;
    height: 90px;
    margin-top: -45px;
    margin-left: -80px;
    top:50%;
    left: 50%;
}
.box-shadow:after {
    content: "";
    width: 154px;
    height: 1px;
    margin-top: 89px;
    margin-left: -77px;
    display: block;
    position: absolute;
    left: 50%;
    z-index: -1;
    -webkit-box-shadow: 0px 0px 8px 2px #000000;
       -moz-box-shadow: 0px 0px 8px 2px #000000;
            box-shadow: 0px 0px 8px 2px #000000;
}
```

3. [渐变背景动画（效果请点击）](//codepen.io/illuSioN4ng/pen/YWjYKW)
```
.test {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(#FC6E51, #FFF);
  background-size: auto 200%;
  background-position: 0 100%;
  transition: background-position 0.5s;
}    
.test:hover {
    background-position: 0 0;
}
```

4. [首字母大写](//codepen.io/illuSioN4ng/pen/BzPJBw)
```
p {
  width: 100px;
  font-family: 'Simsun';
  text-indent: 2em;
  word-wrap:break-word;
}
p:first-child::first-letter{
  font-family: "Microsoft YaHei";
  font-size: 28px;
  font-weight: bold;
}
```


