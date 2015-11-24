title: 学习笔记
date: 2015-08-27 15:48:29
tags: [Learning Notes,JavaScript,CSS]
categories: Learning Notes
toc: true 
---

1. 盒子模型的三维立体结构图中，从第一层到第五层依次为：border、content+padding、background-image、background-color、margin。
2. 几个关于Javascript的问题。
    - 创建JavaScript对象的两种方法：
    使用“new”关键字来调用函数，open/close花括号。
    
            var obj ={};  
    
    使用new关键字，什么情况下创建对象?
    
    - 创建数组：
    
            var myArray = new Array();  
                   
            var myArray = [];  
        
    如何高效地删除JavaScript数组中的重复元素？[JavaScript常用数组算法](http://web.jobbole.com/83518/)
    
    - 变量提升（variable hoisting）
    
    变量提升指的是，无论是哪里的变量在一个范围内声明的，那么JavaScript引擎会将这个声明移到范围的顶部。如果在函数中间声明一个变量，例如在某一行中赋值一个变量：
    
            function f(){  
               //此处省略若干代码  
               var a = "abc";  
            } 
        
    实际上运行起来是这样的：
     
            function f(){  
              var a;  
              //此处省略若干代码  
              a = "abc";  
            } 
            
   - 全局变量的风险，以及如何保护代码不受干扰？
   
    全局变量的危险之处在于其他人可以创建相同名称的变量，然后覆盖你正在使用的变量。
   
    最常用的预防方法是创建一个包含其他所有变量的全局变量：
     
            var applicationName = {}; 
        
    每当你需要创建一个全局变量的时候将其附加到对象上即可。
     
            applicationName.myVariable = "abc"; 
        
    另外一种方法是将所有的代码封装到一个自动执行的函数中去，这样的话，所有申明的变量全都申明在该函数的范围内。
     
            (function(){  
                var a = "abc";  
            })(); 
            
   - 如何通过JavaScript对象中的成员变量迭代？
    
            for(var prop in obj){  
                 // bonus points for hasOwnProperty  
                 if(obj.hasOwnProperty(prop)){  
                     // do something here  
                 }  
            } 
         
     