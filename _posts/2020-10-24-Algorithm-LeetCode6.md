---
title: "[Leet Code] 6. ZigZag Conversion"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/zigzag-conversion/)

이번 포스팅에서는 6번 문제를 다룬다.

파이썬이 아직 익숙치 않아서 코드 부분에 대해 피드백할 부분이 있으면 언제든 댓글로 남겨주시면 감사드리겠습니다.

## Problem

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

__Example 1:__

````
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
````

__Solution :__

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        store: dict = {}
        row: int = 0
        flag: bool = True
        for alp in s:
            if row not in store:
                store[row] = ''
            store[row] += alp
            if flag:
                if row + 1 == numRows:
                    flag = False
                    row -= 1
                else:
                    row += 1
            else:
                if row == 0:
                    flag = True
                    row += 1
                else:
                    row -= 1
        answer: str = ''
        for row in range(numRows):
            if row in store:
                answer += store[row]
        return answer

```

* 주어지는 numRows에 맞게 dictionary의 키를 생성하며, 지그재그에 맞게 데이터를 쌓아나간다.

* 0 numRow에 첫번째 문자를 넣으면, 다음 문자는 1 numRow에, 그 다음 문자는 2 numRow에 쌓아나가는 식이다.

* 그러다가, numRow가 numRows의 마지막 번호에 도달하면 다시 -1씩 줄어들면서, 해당하는 dictionary에 문자를 넣는다.

* value에 해당하는 값의 타입은 문자열로 하여, 문자를 문자열에 추가한다.

* 마지막엔 0번 numRow부터 문자열을 이어붙혀 출력하면 된다.


![](/assets/img/LeetCode/LeetCode_6_1.png)