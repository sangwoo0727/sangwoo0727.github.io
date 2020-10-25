---
title: "[Leet Code] 7. Reverse Integer"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/reverse-integer)

이번 포스팅에서는 6번 문제를 다룬다.

파이썬이 아직 익숙치 않아서 코드 부분에 대해 피드백할 부분이 있으면 언제든 댓글로 남겨주시면 감사드리겠습니다.

## Problem

 Given a 32-bit signed integer, reverse digits of an integer.

Note:
Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

__Example 1:__

````
Input: x = 123
Output: 321
````

__Example 2:__

````
Input: x = -123
Output: -321
````

__Solution :__

```python
class Solution:
    def reverse(self, x: int) -> int:
        if x < 0:
            x *= -1
            s: str = '-' + ''.join(reversed(str(x)))
        else:
            s: str = ''.join(reversed(str(x)))

        if int(s) > 2 ** 31 or int(s) < (-2) ** 31:
            return 0
        return int(s)

```

* 정수 x를 음수와 양수로 나누고, 음수일 경우 -1을 곱하여 잠시 양수로 만든 후, 결과 문자열 s에 -를 붙혀준다.

* 양수일 경우는 그대로 결과 문자열을 저장한다.

* 문자열을 정수형으로 바꾸어, 범위를 넘어가면, 0을 반환해주고, 아닌 경우 그대로 정수형으로 바꾸어 출력해준다.


![](/assets/img/LeetCode/LeetCode_7_1.png)