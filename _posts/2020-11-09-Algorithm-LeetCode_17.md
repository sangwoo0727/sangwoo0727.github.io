---
title: "[Leet Code] 17. Letter Combinations of a Phone Number"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

* 이번 포스팅에서는 Leet Code 17번 Letter Combinations of a Phone Number 문제를 다룬다.
* 문제는 상단의 그림을 클릭하면 볼 수 있습니다.

## Problem
* Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. 
* Return the answer in any order.
* A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.


__Example 1:__

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

__Constraints:__

```
0 <= digits.length <= 4
digits[i] is a digit in the range ['2', '9'].
```


__Solution :__

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        output = []
        dic = {'2': ['a', 'b', 'c'],
               '3': ['d', 'e', 'f'],
               '4': ['g', 'h', 'i'],
               '5': ['j', 'k', 'l'],
               '6': ['m', 'n', 'o'],
               '7': ['p', 'q', 'r', 's'],
               '8': ['t', 'u', 'v'],
               '9': ['w', 'x', 'y', 'z']}

        def back_track(atom, left_digits):
            if not len(left_digits):
                nonlocal output
                output.append(atom)
                return

            now_digit = left_digits[0]
            for alp in dic[now_digit]:
                back_track(atom + alp, left_digits[1:])

        if digits:
            back_track('', digits)
        return output
```

* 단순 브루트포스로 해결할 수 있는 문제였다.
* digits의 최대 길이가 4이므로, 깊이가 4까지 내려가는 재귀 형태의 완전탐색으로 모든 조건을 확인하였다.

![](/assets/img/LeetCode/LeetCode_17_1.png)