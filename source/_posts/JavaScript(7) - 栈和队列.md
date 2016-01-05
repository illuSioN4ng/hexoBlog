title: JavaScript(7) - 栈和队列
date: 2015-12-24 15:54:15
tags: [JavaScript]
categories: JavaScript 
---
## 栈
stack.js文件如下：
```
function Stack(){
    var items = [];
    //入栈
    this.push = function(element){
        items.push(element);
    }
    //出栈
    this.pop = function(){
        return items.pop();
    }
    //返回栈顶元素
    this.peek = function(){
        return items[items.length - 1];
    }
    //是否为空
    this.isEmpty = function(){
        return items.length === 0;
    }
    //栈元素个数
    this.size = function(){
        return items.length;
    }
    //清空栈
    this.clear = function(){
        items = [];
    }
    //打印元素
    this.print = function(){
        console.log(items.toString());
    }
}

module.exports = Stack;
```

利用栈实现从十进制转换为十六进制以内任意进制，代码如下(在node环境下运行)：
```
var Stack = require('./stack');

var newStack = new Stack();

//newStack.push('Aaa');
//newStack.push('Bbb');
//newStack.print();

function baseConverter(decNumber, base){
    var remStack = new Stack(),
        rem,                                //记录余数
        baseString = '',                    //记录转换后的字符串
        digits = '0123456789ABCDEF';        //用来查找对应的字符
    while(decNumber > 0){
        rem = decNumber % base;
        //console.log(rem);
        remStack.push(rem);
        //remStack.print();
        decNumber = Math.floor(decNumber / base);
    }
    while(!remStack.isEmpty()){
        baseString += digits[remStack.pop()];
    }
    return baseString;
}

console.log(baseConverter(100345, 2));      //11000011111111001
console.log(baseConverter(100345, 8));   	//303771
console.log(baseConverter(100345, 16));     //187F9
```

## 队列
队列遵循FIFO（First In First Out，先进先出，也称为先来先服务）原则的一组有序的项。
队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。
队列的实现：
```
function Queue() {

    var items = [];

    this.enqueue = function(element){
        items.push(element);
    };

    this.dequeue = function(){
        return items.shift();
    };

    this.front = function(){
        return items[0];
    };

    this.isEmpty = function(){
        return items.length == 0;
    };

    this.clear = function(){
        items = [];
    };

    this.size = function(){
        return items.length;
    };

    this.print = function(){
        console.log(items.toString());
    };
}
```
举个例子，如何使用队列：
```
var queue = new Queue();
console.log(queue.isEmpty()); 	//outputs true
queue.enqueue("John");
queue.enqueue("Jack");
queue.print();					//John,Jack
queue.enqueue("Camila");
queue.print();					//John,Jack,Camila
console.log(queue.size()); 		//outputs 3
console.log(queue.isEmpty()); 	//outputs false
queue.dequeue();
console.log(queue.dequeue());	//Jack
queue.print();					//Camila
```

**优先级队列**
```
function PriorityQueue() {

    var items = [];

    function QueueElement (element, priority){
        this.element = element;
        this.priority = priority;
    }

    this.enqueue = function(element, priority){
        var queueElement = new QueueElement(element, priority);

        if (this.isEmpty()){
            items.push(queueElement);
        } else {
            var added = false;
            for (var i=0; i<items.length; i++){
                if (queueElement.priority < items[i].priority){
                    items.splice(i,0,queueElement);
                    added = true;
                    break;
                }
            }
            if (!added){
                items.push(queueElement);
            }
        }
    };

    this.dequeue = function(){
        return items.shift();
    };

    this.front = function(){
        return items[0];
    };

    this.isEmpty = function(){
        return items.length == 0;
    };

    this.size = function(){
        return items.length;
    };

    this. print = function(){
        for (var i=0; i<items.length; i++){
            console.log(items[i].element + ' - ' + items[i].priority);
        }
    };
}

var priorityQueue = new PriorityQueue();
priorityQueue.enqueue("John", 2);
priorityQueue.enqueue("Jack", 1);
priorityQueue.enqueue("Camila", 1);
priorityQueue.enqueue("Maxwell", 2);
priorityQueue.enqueue("Ana", 3);
priorityQueue.print();//Jack - 1,Camila - 1,John - 2,Maxwell - 2,Ana - 3
```

