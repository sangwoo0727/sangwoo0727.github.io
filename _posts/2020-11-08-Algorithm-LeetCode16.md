---
title: "[Leet Code] 16. 3Sum Closet"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/3sum-closest/)

* 이번 포스팅에서는 Leet Code 16번 3Sum Closet 문제를 다룬다.

## Problem
* Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. 
* Return the sum of the three integers. You may assume that each input would have exactly one solution.

__Example 1:__

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

__Solution :__

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        answer = 0
        dist = 1000000000000000
        nums.sort()
        for idx, num in enumerate(nums):
            if idx == len(nums) - 2:
                break
            left, right = idx + 1, len(nums) - 1
            while left < right:
                sum = num + nums[left] + nums[right]
                abs_num = abs(sum - target)
                if dist > abs_num:
                    answer = sum
                    dist = abs_num
                if sum < target:
                    left += 1
                elif sum > target:
                    right -= 1
                else:
                    return answer
        return answer

```
* 3sum 문제와 비슷하다.
* 세개의 원소의 합이 target과 가장 가까운 값을 찾아내는 문제이다.
* 3sum 문제와 비슷한 발상에서 시작하는데, 완전탐색으로 하기엔 nums 리스트의 범위가 1000이므로, 삼중포문을 하기엔 시간제약이 생긴다.
* 역시 정렬을 하여 논리적으로 더이상 보지 않아도되는 부분은 가지치기를 하였고, a+b+c에서 a를 기준으로 잡고, b와 c를 투포인터를 이용하여 이중포문으로 해결하였다.

![](/assets/img/LeetCode/LeetCode_16_1.png)