title: leetcode Algorithms task 2
date: 2016-04-03 16:51:15
tags: [JavaScript, Algorithms] 
categories: Algorithms
---
**Difficulty: Medium**
## 问题描述
You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.    
Example:    
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8    
```

### 方法一：
```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    return addTwoNumbersWithCarry(l1, l2, 0);
};

var addTwoNumbersWithCarry = function(a, b, c) {
    var sum = a.val + b.val + c,
        digit = sum % 10,
        carry = sum >= 10 ? 1 : 0,
        node = new ListNode(digit);
    if(a.next || b.next || carry){
        node.next = addTwoNumbersWithCarry(a.next || new ListNode(0), b.next || new ListNode(0), carry);
    }
    return node;
};
```
### [方法一结果分析如图](https://leetcode.com/submissions/detail/57887158/)：    
![结果分析图1.1](/img/leetcode/leetcode2.1.png)

## 方法二
```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    var List = new ListNode(0),
        head = List,
        sum = 0,
        carry = 0;

    while(l1 !== null || l2 !== null|| sum > 0){
        if(l1 !== null){
            sum += l1.val;
            l1 = l1.next;
        }
        if(l2 !== null){
            sum += l2.val;
            l2 = l2.next;
        }
        if(sum>=10){
            carry = 1;
            sum = sum % 10;
        }

        List.next = new ListNode(sum);
        List = List.next;

        sum = carry;
        carry = 0;

    }

    return head.next;
};
```

###[方法二结果分析如图](https://leetcode.com/submissions/detail/57887682/)：    
![结果分析图1.2](/img/leetcode/leetcode2.2.png)

## 总结
从性能分析中来看两种方法差不多，个人更加倾向方法一递归调用，生成链表。（**也没有想到其他更加好的办法来做Orz**）