---
title: 堆#中等#数组中的第K个最大元素
date: 2024-05-09 00:00:00 +0800
last_modified_at: 2024-05-10 00:00:00 +0800
categories: [cpp, algorithm]
tags: [cpp]
author: author1
---

### 前文

给定整数数组`nums`和整数`k`，请返回数组中第`k`个最大的元素。

请注意，你需要找的是数组排序后的第`k`个最大的元素，而不是第`k`个不同的元素。

你必须设计并实现时间复杂度为O(n)的算法解决此问题。
```
示例 1:

输入: [3,2,1,5,6,4], k = 2
输出: 5

示例 2:

输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

### 正文

```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        
    }
};
```

要点：
- vector的基本用法
- 几种排序算法
- 算法的时间复杂度
- 用空间换时间的解题思路

#### 解法1 双循环

```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {

        for(auto i = 0; i < nums.size(); ++i) {
            for(auto j = i+1; j < nums.size(); ++j) {
                if (nums[i] < nums[j]) {
                    int tmp = nums[i];
                    nums[i] = nums[j];
                    nums[j] = tmp;
                }
                if (j == k)
                    return nums[k-1];
            }
        }

        return nums[k-1];
    }
};
```

双循环时间复杂度较高，最坏情况下，内外循环均遍历一遍，即遍历`n, n-1, ..., 1`次，总次数为`n(n+1)/2`，时间复杂度为O(n^2)，不符合题目要求也超出用时过不了测试用例。

#### 解法2 映射

用空间换时间，将每个数出现的次数存储在映射的值中，key为数本身，例如：
```
map<int, int> myMap;
myMap[11] = 1;
myMap[33] = 2;
myMap[22] = 1;
// 假设此时找第2个出现的最大数，只要返回33即可，因为33出现两次，且map会自动按小到大排序
```

所以可以写出如下代码：
```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {

        map<int, int> myMap;
        for(auto e: nums) {
            if (myMap[e]) {
                ++myMap[e];
            } else {
                myMap[e] = 1;
            }
        }

        for (auto it = myMap.rbegin(); it != myMap.rend(); ++it) {
            k -= it->second;
            if (k <= 0)
                return it->first;
        }

        return 0;
    }
};
```

首先将每个数和次数存储在映射中，其次从尾部到头遍历，计算次数，`k`次数减至`<=0`，即可返回key。

这种方式可以通过全部测试用例，但空间效率较低，时间复杂度上最坏结果`for`循环计算一遍，为O(n)。
