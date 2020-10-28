---
title: "[Leet Code] 8. String to Integer(atoi)"
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

[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/string-to-integer-atoi/)

* 이번 포스팅에서는 8번 String to Integer(atoi) 문제를 다룬다.
* 파이썬이 아직 익숙치 않아서 코드 부분에 대해 피드백할 부분이 있으면 언제든 댓글로 남겨주시면 감사드리겠습니다.

## Problem
* Implement atoi which converts a string to an integer.
* The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
* The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
* If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
* If no valid conversion could be performed, a zero value is returned.

```
Note:
 * Only the space character ' ' is considered a whitespace character.
 * Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. 
   If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.
```

__Example 1:__

```
Input: str = "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign. 
             Then take as many numerical digits as possible, which gets 42.
```

__Example 2:__
```
Input: str = "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign. 
             Therefore no valid conversion could be performed.
```

__Solution :__

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        INT_MAX = pow(2, 31) - 1
        INT_MIN = -pow(2, 31)

        num = ''
        for idx, c in enumerate(s):
            if c == ' ':
                continue
            elif c.isalpha() or c == '.':
                break
            elif c.isdigit():
                num += c
                if idx+1 <len(s) and not s[idx+1].isdigit():
                    break
            elif c == '-' or c =='+':
                if idx+1 <len(s) and s[idx+1].isdigit():
                    num += c
                else:
                    break
        num = int(num) if num else 0
        num = INT_MAX if num > INT_MAX else num
        num = INT_MIN if num < INT_MIN else num
        return num

```

* 예외를 조심하자.
* 문자열 관련 구현문제.

![](/assets/img/LeetCode/LeetCode_8_1.jpg)