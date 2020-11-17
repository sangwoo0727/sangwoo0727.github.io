---
title: "[Leet Code] 23. Merge k Sorted Lists"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/merge-k-sorted-lists/)

* 이번 포스팅에서는 Leet Code 23번 Merge k Sorted Lists 문제를 다룬다.

## Problem
* You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.
* Merge all the linked-lists into one sorted linked-list and return it.

__Example 1:__

````
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
````

__Solution :__

```java
public class LeetCode_23_MergeKSortedLists {
    public ListNode mergeKLists(ListNode[] lists) {
        int size = lists.length;
        if(size == 0) return null;
        for(int dist = 1; dist < size; dist *= 2){
            for (int i = 0; i < size - dist; i+=(dist * 2)) {
                lists[i] = merge(lists[i],lists[i+dist]);
            }
        }
        return lists[0];
    }
    static ListNode merge(ListNode l1 , ListNode l2){
        ListNode head = new ListNode();
        ListNode cur = head;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
                cur = cur.next;
            }else{
                cur.next = l2;
                l2 = l2.next;
                cur = cur.next;
            }
        }
        cur.next = l1 != null? l1: l2;
        return head.next;
    }
}
```

* 머지소트에서 병합 과정 방식으로 해결하였다.
* 단 링크드 리스트로 구현되었다는 점이 차이점이다.
* 구현력이 매우 떨어졌다. 열심히해야지
* 한번 병합할 때, 전체 원소의 개수 즉 k * list[i].length 만큼 (N)의 시간이 걸리고 (10^4 * 500)
* 그러한 과정을 logN 만큼 반복하므로, 시간복잡도는 O(NlogN)

![](/assets/img/LeetCode/LeetCode_23_1.jpg)