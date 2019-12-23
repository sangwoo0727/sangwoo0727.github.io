---
title: "[프로그래머스] 괄호 변환"
categories:
  - Algorithm
read_time: false
tags:
- sort
comments:
  - true
toc: true
toc_sticky: true
---

[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/60058)

* 2020 카카오 공채 코딩테스트 기출 문제이다.

### 코드 리뷰
* 재귀를 통한 분할 정복 하는 방법을 문제에서 모두 시뮬레이션 형태로 주어줬다.
* 순서에 맞게 재귀 코드를 작성하면 어렵지 않게 풀 수 있는 문제이다.
* 코드에 대한 부분 설명은 주석에 추가하겠다.

```cpp
#include <string>
#include <vector>
using namespace std;

string solution(string p) {
    //u와 v로 문자열을 분해하되, u는 더 이상 분할할 수 없는 문자열로 만들어야 한다.
	string answer = "";
	int left = 0, right = 0; // 왼쪽 괄호, 오른쪽 괄호 개수를 세기 위한 변수
	bool check = false; // 올바른 괄호 문자열인지 판단하기 위한 변수
	if (p.length() == 0) return ""; //분할한 문자열이 빈 문자열일 경우 빈 문자열 반환
	for (int i = 0; i < p.length(); i++) {
		if (p[i] == '(') left++;
		if (p[i] == ')') right++;
		if (right > left) check = true;
        //오른쪽 괄호 수가 더 커지는 순간 이미 올바른 문자열이 아니기 때문에 check를 해준다
		if (left == right) { // 개수가 같아질 때가 균형잡힌 문자열 u가 만들어지는 순간
			if (check == true) { //u가 올바른 문자열이 아닐 경우
				answer += '(';
				answer += solution(p.substr(i + 1, p.length() - i - 1));
				answer += ')';
				for (int j = 1; j < i; j++) {
					if (p[j] == ')') answer += '(';
					if (p[j] == '(') answer += ')';
				}
				return answer;
			}
			else { //u가 올바른 문자열일 경우
				answer += p.substr(0, i + 1);
				answer += solution(p.substr(i + 1, p.length() - i - 1));
				return answer;
			}
		}
	}
}
```
