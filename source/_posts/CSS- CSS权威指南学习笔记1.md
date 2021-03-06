title: CSS权威指南学习笔记1
date: 2016-01-10 17:23
tags: [Learning Notes,CSS]
categories: CSS 
---
- CSS关键字往往由空格分隔，只有一种情况例外。在CSS的font属性中，只有一种情况可以使用斜线（/）来分隔两个特定的关键字。栗子如下：
	```
		h2 { font: large/150% sans-serif; }//斜线用来分隔了用来设置元素字体大小和行高的两个关键字。
	```
- 是选择器的特殊性
	由选择器本身的组件决定。特殊性值表述为四个部分，一个选择器的具体特殊性如下确定：
	1. 对于选择其中给定的各个ID值：加0,1,0,0；
	2. 对于选择器中给定的各个类属性值、属性选择或者伪类，加0,0,1,0；
	3. 对于选择器中给定的元素和伪元素，加0,0,0,1；
	4. 结合符和通配符选择器对特殊性没有任何贡献。
	```
		h1{color: red}//0,0,0,1
		p em{color: purple}//0,0,0,2
		.grape{color: purple}//0,0,1,0
		*.broght{color: yellow}//0,0,1,0
		p.bright em.dark{color: maroon}//0,0,2,2
		#id306{color: blue}//0,1,0,0
		div#siderBar*[href]{color: silver}//0,1,1,1
	```
- 任何情况下，用户代理都会确定哪些规则与一个元素匹配，计算出所有相关的声明及其特殊性，确定哪些规则胜出，然后将胜出的规则应用到元素，从而得到应用样式后的效果。
- 通用选择器的特殊性
通配选择器对一个选择器的特殊性没有贡献，换句话就是，他的特殊性为0,0,0,0。
- 内联样式的特殊性
特殊性第一位的0就是为了内联样式保留的，他比其他声明的优先级都高。
- 重要性（!iportant）
有时候，某个声明可能比较重要，超过了所有其他声明。CSS2.1称之为重要声明，并允许这些声明的结束分号之前插入`!important` 来标志。
**因为有了这种顺序的排列，才有了通常的推荐链接样式的顺序，一般建议为：link-visited-hover-active(LVHA)的顺序声明连接样式**