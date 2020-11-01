---
title: "[Leet Code] 14. Longest Common Prefix"
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

[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/longest-common-prefix/)

* 이번 포스팅에서는 Leet Code 14번 Longest Common Prefix 문제를 다룬다.
* 문제는 상단의 그림을 클릭하면 볼 수 있습니다.

## Problem
* Write a function to find the longest common prefix string amongst an array of strings.
* If there is no common prefix, return an empty string "".

__Example 1:__

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

__Example 2:__

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

__Solution :__

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not len(strs):
            return ''
        index = 0
        for i in range(0, len(strs[0])):
            flg = False
            for s in strs:
                if strs[0][0:i + 1] != s[0:i + 1]:
                    flg = True
                    break
            if flg:
                return strs[0][0:i]
            else:
                index = i
        return strs[0][0:index + 1]
```

* 모든 문자열의 공통 prefix를 찾는 것이므로, 기준을 어떤 문자열로 해도 상관없다.
* 리스트의 가장 첫번째 원소문자열을 선택하였다.
* 그 후, prefix의 마지막 index를 순차적으로 선택하여, 다른 문자열과 비교하였다.
* 진행하다가, prefix가 다른 문자열이 하나라도 나오면 반복문을 탈출하였다.

![](/assets/img/LeetCode/LeetCode_14_1.png)
