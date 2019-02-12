title: leetcode Algorithms task 1 
date: 2016-04-03 16:14:59 
tags: [JavaScript, Algorithms] 
categories: Algorithms
---
**Difficulty: Easy**
## 问题描述
Given an array of integers, return indices of the two numbers such that they add up to a specific target.    
You may assume that each input would have exactly one solution.     
Example:    
```
Given nums = [2, 7, 11, 15], target = 9,    
Because nums[0] + nums[1] = 2 + 7 = 9,    
return [0, 1].    
```

### 方法一：
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var temp,flag;
    for(var i = 0; i < nums.length; i++){
        temp = target - nums[i];
        flag = nums.indexOf(temp);
        if(flag != -1 && flag != i){
            return [i, flag];
        }
    }
};
```
### [方法一结果分析如图](https://leetcode.com/submissions/detail/57759629/)：    
![结果分析图1.1](/img/leetcode/leetcode1.1.png)

## 方法二
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var result = [],
    map = {};
    for(var i = 0; i < nums.length; i++){
        if(typeof(map[target - nums[i]]) !== 'undefined'){
            result.push(map[target - nums[i]]);
            result.push(i);
        }
        map[nums[i]] = i;
    }
    return result;
};
```

###[方法二结果分析如图](https://leetcode.com/submissions/detail/57762380/)：    
![结果分析图1.2](/img/leetcode/leetcode1.2.png)

## 总结
从性能分析中来看显然方法二的性能远远优于方法一，利用建立字典集，在字典集中查找的时间复杂度要远低于利用```indexOf()```函数。