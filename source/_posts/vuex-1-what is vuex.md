title: What's Vuex  
date: 2018-08-18 14:58:05  
tags: [Vue,JavaScript]  
categories: Vue

---

&emsp;&emsp;Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。  
&emsp;&emsp;状态管理应该包含以下几部分：

- **state** 驱动应用的数据源
- **view** 已声明式将 state 映射到视图
- **actions** 响应在 view 上的用户输入导致的状态变化 

&emsp;&emsp;下面是一个单向数据流的极简示意图：  
  <img src="https://vuex.vuejs.org/flow.png" style="width: 100%; max-width: 450px;">  

&emsp;&emsp;但是，当应用遇到多个组件共享状态时，单向数据流的简洁性就很容易被破
  坏：
- 多个视图依赖于同一状态
- 来自不同视图的行为需要变更同一状态

&emsp;&emsp;对于问题一，传参的方法对于多层嵌套的组件将会变得非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，我们通常采用父子组件直接饮用  或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致代码无法维护。  
&emsp;&emsp;Vuex 就是为了把组件的状态抽离出来，以一个全局单例模式，通过定义和隔离管理中的各种概念并强烈遵守一定的规则，来管理。  
&emsp;&emsp;Vuex 背后的基本思想，借鉴了 Flux、Redux、和 The Elm Architecture。与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

<img src="https://vuex.vuejs.org/vuex.png" alt="vuex">
