---
title: "[프로그래머스] 가운데 글자 가져오기"
categories:
  - Algorithm
tags:
  - String
comments:
  - true
---

## [프로그래머스] 가운데 글자 가져오기

[문제 링크 - 로그인이 필요합니다.](https://programmers.co.kr/learn/courses/30/lessons/12903)

### 문제 설명
* string 문자열 s를 받아와서 가운데 글자를 반환하는 함수를 구현하는 문제.
* 짝수일 땐 가운데 두글자를 반화하면 된다.

### 코드 리뷰
* 쉬운 문제인데 다른 사람 코드를 보다가 대박인 느낌이 들어서 블로그 포스팅을 해놓으려 한다.
* 문자열 길이가 짝수인지 홀수인지 판별하기 위해 나는 문자열 길이를 2로 나눈 나머지가 1인 경우와 0인 경우로 나눠서 풀었다.
* 대부분 위와 같은 방식으로 풀었을텐데, 어떤 분이 s.length() & 1 비트 연산을 통하여 짝수, 홀수를 판별하였다... 짝수는 2진수의 1의 자리가 항상 0이고, 홀수는 2진수의 1의 자리가 항상 1이기 때문에, 1과 비트연산을 하면 홀수는 항상 1, 짝수는 항상 0이 나온다.

```cpp
//내가 푼 코드
#include <iostream>
#include <string>

using namespace std;

string solution(string s) {
	return s.length() % 2 ? s.substr(s.length()*0.5, 1) : s.substr(s.length()*0.5 - 1, 2);
}
```

```cpp
//비트연산자를 이용한 코드
#include <iostream>
#include <string>

using namespace std;

string solution(string s) {
	return s.length() & 1 ? s.substr(s.length()*0.5, 1) : s.substr(s.length()*0.5 - 1, 2);
}
```

