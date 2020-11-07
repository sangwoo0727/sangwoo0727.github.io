---
title: "[Leet Code] 15. 3Sum"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/3sum/)

* 이번 포스팅에서는 Leet Code 15번 3Sum 문제를 다룬다.

## Problem

* Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
* Notice that the solution set must not contain duplicate triplets. 

__Example 1:__

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

__Solution :__

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        answer = []
        nums.sort()
        for idx, num in enumerate(nums):
            if idx == len(nums) - 2:
                break
            if idx != 0 and num == nums[idx-1]:
                continue
            left, right = idx + 1, len(nums) - 1
            while left < right:
                sum = num + nums[left] + nums[right]
                if sum == 0:
                    answer.append([num,nums[left],nums[right]])
                    while left < len(nums)-1 and nums[left] == nums[left+1]:
                        left+=1
                    while right > idx and nums[right] == nums[right-1]:
                        right-=1
                    left += 1
                    right -= 1
                elif sum < 0:
                    left += 1
                else:
                    right -= 1
        return answer

```

* 3개의 원소의 합이 0이 되는 모든 경우의 수를 반환하되, 중복을 제거해야한다.
* 리스트의 길이가 3000까지라, 완전탐색은 불가능하다.
* 중복을 제거하기 위해 가장 먼저 리스트를 정렬을 해주었다.
* 이후, a+b+c = 0 이 되기 위해, a = -(b+c)로 생각을 하였고, a를 기준으로, b와 c를 투포인터를 이용하여 o(n)만에 b와 c를 결정하였다.
* a + b + c > 0 일 경우, b(left)를 한칸 올려주었고, 반대의 경우 c(right)를 한칸 내려주었다.
* 원소는 같은 값이 존재할 수 있으므로, sum == 0인 경우 리스트에 추가한 후, left와 right를 중복된 값이 사라질 때까지 이동시켜 주었다.
* 정렬하는데, O(nlogn) + 탐색하는데 O(n^2)이므로 총 시간복잡도는 o(n^2)

![](/assets/img/LeetCode/LeetCode_15_1.png)