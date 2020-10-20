---
title: "[Leet Code] 1. Two Sum"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/two-sum/)


파이썬 공부를 시작하면서, LeetCode 문제들을 하나씩 풀어볼 예정이다. 첫번째 문제는 1. Two Sum 문제이다.

위의 이미지를 통해 문제를 확인할 수 있다.

## Problem

 Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

 You may assume that each input would have exactly one solution, and you may not use the same element twice.

 You can return the answer in any order.


__Example :__

````
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
````


__Solution :__

```python
def two_sum(self, nums: List[int], target: int) -> List[int]:
    store: Dict[int, int] = {}
    for idx, num in enumerate(nums):
        if num in store:
            return [store[num], idx]
        store[target - num] = idx
```


 2중 포문을 이용해 풀기엔 nums 리스트의 크기가 10^5 였다.

 답은 무조건 1개가 존재함이 보장되므로, store라는 딕셔너리를 만들어서, num값 : 인덱스번호로 저장을 해나갔다.

![](/assets/img/LeetCode/LeetCode_1_1.jpg)



