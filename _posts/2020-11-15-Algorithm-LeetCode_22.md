---
title: "[Leet Code] 22. Generate Parentheses"
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
[![](/assets/img/LeetCode.jpeg){:width = "100%"}](https://leetcode.com/problems/generate-parentheses/)

* 이번 포스팅에서는 Leet Code 22번 Generate Parentheses 문제를 다룬다.

## Problem
* Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

__Example 1:__

````
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
````

__Solution :__

```java
import java.util.*;

public class LeetCode_22_GenerateParentheses {
    static List<String> output;
    static Deque<Character> stack;
    static StringBuilder sb;
    public List<String> generateParenthesis(int n) {
        output = new ArrayList<>();
        stack = new ArrayDeque<>();
        sb = new StringBuilder();
        comb(0,0,n);
        return output;
    }
    static void comb(int left,int right, int n){
        if(left == n && right == n){
            output.add(sb.toString());
            return;
        }
        if(left < n){
            stack.add('(');
            sb.append('(');
            comb(left+1, right, n);
            stack.poll();
            sb.deleteCharAt(sb.length()-1);
        }
        if(right < n){
            if(stack.size()> right){
                sb.append(')');
                comb(left,right+1,n);
                sb.deleteCharAt(sb.length()-1);
            }
        }
    }
}
```

* 괄호 쌍의 개수가 n으로 주어지고, n의 범위는 8까지이다.
* 백트래킹으로 해결.
* 왼쪽괄호를 스택을 통해 담고, 그 기준으로 오른쪽 괄호를 넣을 수 있을때 뎁스를 들어갔다.
* 아닌 경우 가지치기.

![](/assets/img/LeetCode/LeetCode_22_1.png)

