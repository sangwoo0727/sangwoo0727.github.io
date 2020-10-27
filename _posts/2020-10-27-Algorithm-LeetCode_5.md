---
title: "[Leet Code] 5. Longest Palindromic Substring"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/longest-palindromic-substring/)

이번 포스팅에서는 5번 문제를 다룬다.

파이썬이 아직 익숙치 않아서 코드 부분에 대해 피드백할 부분이 있으면 언제든 댓글로 남겨주시면 감사드리겠습니다.

## Problem

__5. Longest Palindromic Substring__

Given a string s, return the longest palindromic substring in s.

1 <= s.length <= 1000

s consist of only digits and English letters (lower-case and/or upper-case),

__Example 1:__

```
Input: s = "cbbd"
Output: "bb"
```

__Example 2:__

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```


__Solution :__

```python
from typing import *


class Solution:
    def longestPalindrome(self, s: str) -> str:
        dp: List[List[bool]] = [[False] * len(s) for _ in range(len(s))]
        answer: str = s[0]

        for idx in reversed(range(len(s))):
            for jdx in range(idx, len(s)):
                dp[idx][jdx] = s[idx] == s[jdx] and (jdx - idx <= 2 or dp[idx + 1][jdx - 1])
                if dp[idx][jdx] and jdx - idx >= len(answer):
                    answer = s[idx:jdx + 1]

        return answer
```


* DP를 이용하여 풀었다.

* s[i:j]가 팰린드롬이려면, s[i+1:j-1] 역시 팰린드롬이면 된다.

* 시간복잡도는 O(n^2)인데, 시간이 왜이렇게 많이 나오는지 모르겠다. 파이썬이라 그런가


![](/assets/img/LeetCode/LeetCode_5_1.jpg)




