---
title: "[Leet Code] 11. Container with most water"
categories:
  - Algorithm
read_time: false
tags:
  - Algorithm
comments:
  - true
toc: true
toc_sticky: true
---
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/container-with-most-water/)

* 이번 포스팅에서는 Leet Code 11번 Container with most water 문제를 다룬다.

## Problem

* Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i is at (i, ai) and (i, 0). 
* Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

```
Notice:
that you may not slant the container.
```

__Example 1:__

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

__Example 2:__

```
Input: height = [1,1]
Output: 1
```

__Solution :__

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l_idx, r_idx = 0, len(height) - 1
        max_area = 0
        while l_idx < r_idx:
            max_area = max(max_area, (r_idx - l_idx) * min(height[l_idx], height[r_idx]))
            if height[l_idx] < height[r_idx]:
                l_idx += 1
            else:
                r_idx -= 1
        return max_area
```

* two pointer를 이용하여, left = 0, right = len(height)-1로 양 사이드 인덱스를 잡고, 범위를 좁혀나가면서, max값을 저장해나간다.
* O(n)으로 해결할 수 있다.

![](/assets/img/LeetCode/LeetCode_11_1.png)

