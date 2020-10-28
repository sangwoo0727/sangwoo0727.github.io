---
title: "[Leet Code] 9. Palindrome Number"
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

[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/palindrome-number/)

* 이번 포스팅에서는 Leet Code 9번 Palindrome 문제를 다룬다.
* 문제에서 제약을 하나 걸어놔서, 두가지 풀이를 올릴 예정이다.

## Problem
* Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
* __Follow up:__ Could you solve it without converting the integer to a string?

__Example 1:__

```
Input: x = 121
Output: true
```

__Example 2:__
```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

__Solution :__

```python

# Solution 1. without converting the integer to a string

import math

class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        elif x == 0:
            return True
        tmp_num, result, idx = x, 0, pow(10,int(math.log10(x)))
        while tmp_num > 0:
            result += ((tmp_num % 10) * idx)
            tmp_num //= 10
            idx //= 10
        return result == x

```

* 문제에서, 정수를 문자열로 변경시키지 않고 풀어보라고 제약을 걸어두었다.
* 자리수를 구해서, 새롭게 팰린드롬 정수를 만들어 원래 정수와 같은지 비교하여 풀었다.
* 자리수는 log함수를 이용하였다.
* 풀고난 후, 파이썬 문법도 적응할 겸 문자열로 변환하여도 풀어보았다.
* 파이썬으로는 문자열로 변환한다면 한줄 코드로 풀 수 있는 문제였다..이렇게 편할줄이야

```python

# Solution 2. with converting the integer to a string

class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]

```

![](/assets/img/LeetCode/LeetCode_9_1.jpg)

