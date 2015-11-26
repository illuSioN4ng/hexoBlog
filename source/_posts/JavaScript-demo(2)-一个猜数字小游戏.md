title: Javascript一个猜数字的小游戏
date: 2015-08-27 19:29:29
tags: [JavaScript]
categories: JavaScript
toc: true 
---
今天学习了一个猜数字的小游戏的JS写法，方法很简单，现在写下来：
1. 随机数的产生

        function randomize(){//取1-10随机数，其实是根据时间~~~~(>_<)~~~~  
            var now = new Date();  
            num = (now.getSeconds()) % 10;  
            num ++;  
        }  
    
2. 猜数函数

        function guessIt(form){  
            if(form.tfield.value == num){  
                alert("Correct!");  
                form.tfield.focus();  
                trys = 0;  
                randomize();  
            }else{  
                trys ++;  
                alert(trys + " wrong. Try again!");  
                form.tfield.value = "";  
                form.tfield.focus();  
            }  
        } 
       
3. HTML主体

        <body bgColor="lightgreen" onLoad="randomize()">  
            <center>  
                <b>pick a number between 1 and 10</b>  
                <form action="" name="myform">  
                    <input type="textbox" size="4" name="tfield">  
                    <p>  
                        <input type="button" name="button1" value="Check my guess!" onCLick="guessIt(this.form)">  
                    </p>  
                </form>  
            </center>  
        </body> 
        
[实际效果请点击](http://www.cdyjy.uestc.edu.cn/uestc_la/GuessNum.html)