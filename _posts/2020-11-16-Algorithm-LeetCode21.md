---
title: "[Leet Code] 21. Merged Two Sorted Lists"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/merge-two-sorted-lists/)

* 이번 포스팅에서는 Leet Code 21번 Merged Two Sorted Lists 문제를 다룬다.

## Problems
* Merge two sorted linked lists and return it as a new sorted list. 
* The new list should be made by splicing together the nodes of the first two lists.

__Example 1:__

````
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
````

__Example 1:__

````
Input: l1 = [], l2 = []
Output: []
````

__Solution :__

```java
public class LeetCode_21_MergeTwoSortedLists {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        ListNode head = new ListNode();
        ListNode cur = head;
        while (l1 != null && l2 != null){
            if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
            }else{
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        if (l1 != null){
            cur.next = l1;
        }
        if (l2!= null){
            cur.next = l2;
        }
        return head.next;
    }
}
```

* 간단하게 머지소트에서 이미 정렬된 두 리스트를 병합시키는 과정의 문제이다.
* l1과 l2가 이미 정렬이 되어있으므로, 순차적으로 보면서 작은 값을 리스트에 하나하나 추가해주면 된다.
* 두개의 리스트 중 하나라도 모든 값 탐색이 끝났다면, 탐색이 끝나지 않은 리스트의 나머지를 전부 추가해주면 된다.

![](/assets/img/LeetCode/LeetCode_21_1.jpg)