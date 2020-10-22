---
title: "[Leet Code] 3. Longest Substring Without Repeating Characters"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

이번 포스팅에서는 3번 문제를 다룬다.

파이썬이 아직 익숙치 않아서 코드 부분에 대해 피드백할 부분이 있으면 언제든 댓글로 남겨주시면 감사드리겠습니다.

## Problem

Given a string s, find the length of __the longest substring__ without repeating characters.

__Example 1:__

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

__Example 2:__

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

__Solution :__

```python
class Solution:
    def length_of_longest_substring(self, s: str) -> int:
        return binary_search(len(s), s)


def binary_search(size: int, s: str) -> int:
    left: int = 0
    right: int = size
    answer: int = 0
    while left <= right:
        middle = (left + right) // 2
        if check(middle, s):
            answer = middle
            left = middle + 1
        else:
            right = middle - 1
    return answer


def check(length: int, s: str) -> bool:
    store: dict = {}
    left: int = 0
    right: int = length - 1
    for idx in range(length):
        if s[idx] in store:
            store[s[idx]] += 1
        else:
            store[s[idx]] = 1
    if len(store) == length:
        return True
    while right + 1 < len(s):
        if store[s[left]] == 1:
            del store[s[left]]
        else:
            store[s[left]] -= 1
        if s[right + 1] in store:
            store[s[right + 1]] += 1
        else:
            store[s[right + 1]] = 1
        left += 1
        right += 1
        if len(store) == length:
            return True
    return False

```

* 문제 example에도 나와있지만, 이 문제는 __subsequence가 아닌 substring__ 즉, 연속된 부분 문자열을 찾는 문제이다.

* 문자열의 길이가 5 * 10^4 이라, 시간복잡도를 고려해봐야 한다.

* 파라메트릭 서치(이 길이에 조건에 맞는 부분 문자열이 존재하는지)와 슬라이딩 윈도우(해당 길이의 부분 문자열이 존재하는지 확인)을 통해 O(NlogN)으로 해결하였다.

* 부분 문자열을 확인하는 방법은 dictionary를 통해서, 부분 문자열 길이와 dictionary의 크기가 같으면 중복되는 단어가 없는 것으로 판단하였다.


![](/assets/img/LeetCode/LeetCode_3_1.jpg)

