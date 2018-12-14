title: 策略模式
date: 2017-12-17 14:02:56
tags: [OOP,JavaScript]
categories: [OOP]
---
&emsp;&emsp;在我们的实际业务中经常会遇到很多分支判断的情况，包括商城根据用户会员等级的促销折扣、根据绩效等级的年终奖金计算，这些情况下的每种分支的业务都是类似的，只是对于具体业务的具体处理过程或者算法的不同，导致最终的效果不同。各分支之间都是平级关系，这种情况下我们可以采用策略模式，来解决算法和使用者之间的耦合。    
&emsp;&emsp;策略模式的定义是：定义一系列的算法，把它们一个一个封装起来，并且使它们可以相互替换。    
## 使用策略模式计算奖金
### 最原始的代码实现
&emsp;&emsp;我们编写一个名为 `calculateBonus` 的函数来计算每个人的年终奖金。显然，改函数需要两个参数：员工的工资数额和他的绩效考核等级。
```js
var calculateBonus = function( performanceLevel, salary ){
    if ( performanceLevel === 'S' ){
        return salary * 4;
    }
    if ( performanceLevel === 'A' ){
        return salary * 3;
    }
    if ( performanceLevel === 'B' ){
        return salary * 2;
    }
};

calculateBonus( 'B', 20000 ); // 返回：40000
calculateBonus( 'S', 6000 ); // 返回：24000
```

&emsp;&emsp;可以看出元时代吗中存在着显而易见的缺点：
- `calculateBonus` 函数会随着绩效考核等级的增加而不断变得臃肿起来， `if-else` 语句会越变越多；
- `calculateBonus` 函数缺乏弹性，每增加一种绩效等级或者是改变一个绩效等级的奖金系数，我们都必须深入到函数内部去进行查找并修改；
- 函数的复用性差。

&emsp;&emsp;因此我们就需要重构这一部分代码。

### 使用组合函数的方式重构代码
&emsp;&emsp;重构最简单的方式就是使用组合函数来进行，我们把各种算法封装到一个一个的小函数中，使用一些良好的命名规范，可以一目了然地知道它对应的算法，他们也可以复用到程序的其他地方。

```js
var performanceS = function( salary ){
    return salary * 4;
};
var performanceA = function( salary ){
    return salary * 3;
};
var performanceB = function( salary ){
    return salary * 2;
};
var calculateBonus = function( performanceLevel, salary ){
    if ( performanceLevel === 'S' ){
        return performanceS( salary );
    }
    if ( performanceLevel === 'A' ){
        return performanceA( salary );
    }
    if ( performanceLevel === 'B' ){
        return performanceB( salary );
    }
};
calculateBonus( 'A' , 10000 ); // 返回：30000
```

&emsp;&emsp;采用组合函数的方式我们解决了功能复用的问题，但是还是没有解决函数变得越来越臃肿的可能以及缺乏弹性的事实。

### 使用策略模式
&emsp;&emsp;正如上文中提到的策略模式的定义：定义一系列的算法，把它们一个一个封装起来。将不变与变的部分隔离开是每个设计模式的主题，策略模式也不例外，策略模式的目的就是将函数的使用和函数的实现隔离开。   
&emsp;&emsp;一个基于策略模式的程序至少由两部分组成，第一部分是预测策略类，用来封装具体的算法，并负责计算过程；第二部分是环境`context`类，接受到用户的请求后，随后将该请求委托给具体的某一个策略类。所以在 `context` 中要维持对某个策略对象的引用。

```js
var performanceS = function(){};
performanceS.prototype.calculate = function( salary ){
    return salary * 4;
};
var performanceA = function(){};
performanceA.prototype.calculate = function( salary ){
    return salary * 3;
};
var performanceB = function(){};
performanceB.prototype.calculate = function( salary ){
    return salary * 2;
};

//接下来定义奖金类Bonus：

var Bonus = function(){
    this.salary = null; // 原始工资
    this.strategy = null; // 绩效等级对应的策略对象
};
Bonus.prototype.setSalary = function( salary ){
    this.salary = salary; // 设置员工的原始工资
};
Bonus.prototype.setStrategy = function( strategy ){
    this.strategy = strategy; // 设置员工绩效等级对应的策略对象
};
Bonus.prototype.getBonus = function(){ // 取得奖金数额
    return this.strategy.calculate( this.salary ); // 把计算奖金的操作委托给对应的策略对象
};

var bonus = new Bonus();
bonus.setSalary( 10000 );

bonus.setStrategy( new performanceS() ); // 设置策略对象
console.log( bonus.getBonus() ); // 输出：40000
bonus.setStrategy( new performanceA() ); // 设置策略对象
console.log( bonus.getBonus() ); // 输出：30000
```    

&emsp;&emsp;上文中的 `context` 类就是 `bonus` ， `performanceX` 类就是一个策略类，代表绩效等级 `X` 的策略方法。    
&emsp;&emsp;虽然已经对于代码进行了相应的重构，不过该段代码是基于传统的面向对象语言来实现的，在 `JavaScript` 中有更加简单的方式去实现。    

## JavaScript中的策略模式
&emsp;&emsp;Talk is cheap,show you code.    
```js
var strategies = {
		"S": function( salary ){
			return salary * 4;
		},
		"A": function( salary ){
			return salary * 3;
		},
		"B": function( salary ){
			return salary * 2;

		}
	};
	var calculateBonus = function( level, salary ){
		return strategies[ level ]( salary );
	};

	console.log( calculateBonus( 'S', 20000 ) ); // 输出：80000
	console.log( calculateBonus( 'A', 10000 ) ); // 输出：30000
```

## 策略模式的优缺点
&emsp;&emsp;测试模式是一种有效的设计模式，在上述的描述中，我们不难看出策略模式的一些优缺点。    
### 优点
- 策略模式利用组合、委托和多态等技术和思想，可以有效地避免多重条件选择语句；
- 策略模式封装了一些策略类，将算法封装在独立的 `strategy` 中，使得它们之间易切换、理解、扩展；
- 策略模式中的策略类易于复用，从而避免了许多复制粘贴修改的工作；
- 使用组合和委托来让 `context` 类拥有执行算法的能力，这也是继承的一种更有效的替代方案。

### 缺点
- 选择使用的 `strategy` 的决定权在用户，所以用户必须了解所有的 `strategy` 的作用，这样才能找到一个合适的 `strategy` ，这增加了使用成本；
- 因为策略模式中的 `strategy` 都是独立封装的，所以一些复杂算法中处理相同逻辑部分也不能公用，所以我们需要采用一些其他的方案来辅助。

> 参考: 
> 1. [JavaScript设计模式与开发实践](https://book.douban.com/subject/26382780/) 
> 1. [JavaScript设计模式](https://book.douban.com/subject/26589719/) 