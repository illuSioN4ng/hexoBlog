title: leetcode-136-single-number
tags:
  - JavaScript
  - Algorithms
categories:
  - Algorithms
date: 2019-02-12 20:54:44
---

&emsp;&emsp;**Difficulty: easy**
## 问题描述
&emsp;&emsp;Given a non-empty array of integers, every element appears twice except for one. Find that single one.    
**Note**:    
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?    
**Example 1**:    
```
Input: [2,2,1]
Output: 1
```
**Example 2**:    
```
Input: [4,1,2,1,2]
Output: 4
```
## 方法一
### 算法
1. 遍历输入数组 `nums`；
2. 如果该数字不在 `noDuplicateArr` 中，则 `push` 进去；
3. 如果该数字在 `noDuplicateArr` 中已经存在，则删除；
4. 最终在数组 `noDuplicateArr` 剩下的元素就是单一数字。

### 代码
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    let noDuplicateArr = [],
        index = -1
    nums.forEach((num, i) => {
        index = noDuplicateArr.indexOf(num)
        if(index !== -1) {
            noDuplicateArr.splice(index, 1)
        } else {
            noDuplicateArr.push(num)
        }
    })
    return noDuplicateArr.shift()
};
```

### [结果分析](https://leetcode.com/submissions/detail/207352587/)
```
Runtime: 204 ms, faster than 15.66% of JavaScript online submissions for Single Number.
Memory Usage: 37.8 MB, less than 2.29% of JavaScript online submissions for Single Number.
```
![方法一结果分析](/img/leetcode/136.1.png)

### 复杂度分析
- 时间复杂度：O(N<sup>2</sup>)
&emsp;&emsp;迭代 `nums` 数组的时间复杂度为 `O(N)`，在每一次迭代中，我们需要对 `noDuplicateArr` 进行 `indexOf` 操作，判断元素是否重复，这步操作的时间复杂度为 `O(N)`，所以，整体算法的时间复杂度为 `O(N<sup>2</sup>)`     
- 空间复杂度： `O(N)`
&emsp;&emsp;我们需要一个长度为 `n` 的数组和一个记录索引为知的变量，所以，空间复杂度为 `O(N)`

## 方法二
### 算法
&emsp;&emsp;`2(a + b + c) - (a + a + b + b + c) = c`，对于数组 `nums` 求集合，然后用集合的和的两倍减去数组的和，余下的即为单一数字。    

### 代码
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    return 2 * sums(Array.from(new Set(nums))) - sums(nums)
};

var sums = (nums) => {
    return nums.reduce((cur, prev) => {
        return prev + cur
    }, 0)
}
```

### [结果分析](https://leetcode.com/submissions/detail/207167799/)
```
Runtime: 76 ms, faster than 51.47% of JavaScript online submissions for Single Number.
Memory Usage: 37.7 MB, less than 2.29% of JavaScript online submissions for Single Number.
```
![方法二代码分析](/img/leetcode/136.2.png)

### 复杂度分析
- 时间复杂度：`O(n + n) = O(n)`
- 空间复杂度： `O(1)`

## 方法三比特位处理
### 算法
1. 如果我们对一个数字与 `0` 进行异或操作，返回当前数字，如 `a ⊕ 0 = a`；
2. 如果我们对相同的数字进行异或操作，则得到 `0`，如 `a ⊕ a = 0`；
3. 异或操作具有交换律，如 `a ⊕ b ⊕ a = a ⊕ a ⊕ b = 0 ⊕ b = b`；
4. 所以根据以上条件，我们可以对所有数字进行异或操作，从而得到单一数字。

### 代码
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    return nums.reduce((prev, cur) => prev ^ cur, 0)
};
```

### [结果分析](https://leetcode.com/submissions/detail/207167799/)
```
Runtime: 72 ms, faster than 58.97% of JavaScript online submissions for Single Number.
Memory Usage: 35.5 MB, less than 2.59% of JavaScript online submissions for Single Number.
```
![方法三代码分析](/img/leetcode/136.3.png)

### 复杂度分析
- 时间复杂度： `O(N)`
- 空间复杂度： `O(1)`