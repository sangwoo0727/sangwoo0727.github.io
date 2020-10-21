---
title: "[Leet Code] 2. Add Two Numbers"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/add-two-numbers/)

두번째 포스팅할 Leet Code 문제는 2. Add Two Numbers 문제이다.

위의 이미지를 통해 문제를 확인할 수 있다.


## Problem
 
 You are given two non-empty linked lists representing two non-negative integers. 
 
 The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

 You may assume the two numbers do not contain any leading zero, except the number 0 itself.


__Example :__

````
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
````

__Solutions :__

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def add_two_numbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        l1_str: str = ''
        l2_str: str = ''
        while l1 is not None or l2 is not None:
            if l1 is not None:
                l1_str += str(l1.val)
                l1 = l1.next
            if l2 is not None:
                l2_str += str(l2.val)
                l2 = l2.next

        l1_int: int = int(''.join(reversed(l1_str)))
        l2_int: int = int(''.join(reversed(l2_str)))

        answer_str: str = ''.join(reversed(str(l1_int + l2_int)))

        answer: ListNode = ListNode()
        cur_node: ListNode = ListNode()
        for idx, num in enumerate(answer_str):
            if idx == 0:
                answer = ListNode(num)
                cur_node = answer
            else:
                new_node: ListNode = ListNode(num)
                if cur_node.next is not None:
                    cur_node = cur_node.next
                cur_node.next = new_node
        return answer
```

링크드 리스트로 구현되어 있고, 답 역시 링크드 리스트로 구하는 문제여서, 파이썬을 처음 공부하며 도움이 많이 되었다.

들어온 두 링크드 리스트를 순회하며, 문자열로 바꿔준 후, 그 문자열을 다시 정수로 바꿔주었다.

answer_str이라는 문자열 변수에는 위에서 구한 두 정수를 더하여 reverse 된 문자열을 넣어주었다.

이후, answer_str의 문자 하나하나를 순회하며 새로운 링크드 리스트를 구현해주었다.

![](/assets/img/LeetCode/LeetCode_2_1.jpg)