---
title: "[프로그래머스] 문자열 압축"
categories:
  - Algorithm
tags:
- string
comments:
  - true
toc: true
toc_sticky: true
---
## [프로그래머스] 문자열 압축

[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/60057)

* 2019 하반기 카카오 공채 문제였다.
* 그 당시 풀면서, 한가지 놓쳤던 부분이 있었다.

### 생각의 흐름
* 먼저 나는 완전탐색으로 문자열을 1개로 자를 때, 2개로 자를 때... 모든 경우로 탐색해주었다.
* 이때, 입력받은 문자열 길이/2 보다 큰 경우로 자를 경우는 항상 입력받은 문자열 길이로 답이 나오기때문에 그 이상은 보지 않았다.
* 자른 문자열을 stack에 넣고, 다음 그 크기 만큼의 문자열을 보고 스택의 top과 같을 경우는 stack에 추가하고, 아닌 경우는 stack에 있는 값을 답으로 저장할 ans 변수에 추가를 해주고 stack을 비워주었다. 
* 이때, stack의 크기에 따라 ans변수에 공통된 개수의 문자열 크기만큼 넣어주었다.

### 놓쳤던 부분
* 시험을 볼 당시에는 stack의 크기가 1이 아닐 경우, 단순히 문자열 ans 변수에 '1' 문자열을 추가해주었다.
* 이 부분이 다시 풀 때도 계속하여 오답으로 처리되었다.
* 생각을 하다보니, 만약 겹쳐지는 문자열의 개수가 10일 경우는 ans에 추가되어야하는 문자가 두자리수이기 때문에, 2개가 추가되어야 한다.
* 이 부분을 놓쳐 계속해서 오답이 나왔다.
* 그래서 stack의 사이즈 크기의 자리수만큼을 ans 변수에 추가해주었다.

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <stack>
using namespace std;

int solution(string s) {
	int answer = 1000000;
	if (s.length() == 1) return 1;
	for (int cut_num = 1; cut_num <= s.length() / 2; cut_num++) {
		string str, ans;
		stack <string> st;
		int cnt = 0;
		int tmp = 0;
		for (int i = 0; i*cut_num < s.length(); i++) {
			tmp = i;
			str = s.substr(i*cut_num, cut_num);
			if (st.empty()) {
				st.push(str);
			}
			else {
				if (st.top() == str) {
					st.push(str);
				}
				else {
					if (st.size() != 1) {
						int size = st.size();
						while (size) {
							size /= 10;
							ans += '1';
						}
					}
					ans += st.top();
					while (!st.empty()) st.pop();
					st.push(str);
				}
			}
		}
		if (!st.empty()) {
			if (st.size() != 1) {
				int size = st.size();
				while (size) {
					size /= 10;
					ans += '1';
				}
			}
			ans += st.top();
			while (!st.empty()) st.pop();
		}
		int size = ans.length();
		answer = min(answer, size);
	}
	return answer;
}
```

