---
title: "[알고스팟] PI-원주율 외우기"
categories:
  - Algorithm
tags:
  - DynamicProgramming
comments:
  - true
toc: true
toc_sticky: true
---

## [알고스팟] PI-원주율 외우기

[문제 링크](https://algospot.com/judge/problem/read/PI)

### 문제 설명
* 숫자 문자열이 최대 길이 10000까지 주어진다.
* 한번에 선택할 수 있는 문자의 개수는 3,4,5개 이다.
* 선택한 문자가 반복이 어떻게 되느냐에 따라 난이도가 주어진다.
* 문자열길이까지 문자 선택을 반복해나갈때, 난이도 합의 최소를 구하는 문제이다.

### 코드 리뷰
* 일단 모든 방법의 수를 본다고 할 때 가지수는 너무너무 많다.. 한번 선택할 때 3가지가 있고, 트리의 높이는 대략 2000~3000 정도 나오기 때문에 완전 탐색으로는 해결할 수가 없다.
* 두번째로 생각한 방법은 그리디하게 항상 최소 난이도를 골라가겠다는 방법이다. 이 방법 역시 반례가 존재했다.
* 생각을 바꾸어보자. 앞선 방법은 현재 있는 인덱스에서 3,4,5개 중에 무엇을 선택해 나가겠냐 였지만 이번엔 현재있는 인덱스까지의 난이도의 최소합을 구해나가면 내가 원하는 문자열길이만큼 도착했을 때도 난이도의 최소합이 될 것이다. 
* i번째 인덱스에 위치해있을 때, i,i-1,i-2 의 문자를 선택해서 온 경우, 혹은 4개를 선택해서 온 경우, 마지막으로 5개를 선택해서 온 경우의 합 중 최소를 선택하게 되면 i번째 인덱스까지의 난이도 최소합(dp[i])이 될 것이다.
* 주장을 이렇게 했으므로, dp[i-3] 역시도 i-3번째 위치까지의 최소합이 저장되어 있을 것이고, dp[i-4] 역시도 i-4번째, dp[i-5]역시도 i-5번째 위치까지의 최소합이 저장되어 있을 것이다. 즉 원하는 n번째까지의 문자열 중 최소를 구하기 위하여 i번째 까지의 최소 합을 구하는 작은 문제로 나누었고, 그 저장된 값을 다시 사용하기 때문에 DP문제가 된다.
* DP[i] = min(DP[i-3]+ f3(i) , DP[i-4] + f4(i) , DP[i-5] + f5(i)) 의 점화식을 세울 수 있다.
* 나머지 구현은 노가다..

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;

int dp[10005];
char str[10005];


int findlev3(int pos) {
	int n1 = str[pos] - '0', n2 = str[pos - 1] - '0', n3 = str[pos - 2] - '0';
	if (n1 == n2 && n2 == n3) return 1;
	else if (n1 + 1 == n2 && n2 + 1 == n3) return 2;
	else if (n1 - 1 == n2 && n2 - 1 == n3) return 2;
	else if (n1 == n3 && n1 != n2) return 4;
	else if ((n1 - n2) == (n2 - n3)) return 5;
	else return 10;
}

int findlev4(int pos) {
	int n1 = str[pos] - '0', n2 = str[pos - 1] - '0';
	int n3 = str[pos - 2] - '0', n4 = str[pos - 3] - '0';
	if (n1 == n2 && n2 == n3 && n3 == n4) return 1;
	else if (n1 + 1 == n2 && n2 + 1 == n3 && n3 + 1 == n4) return 2;
	else if (n1 - 1 == n2 && n2 - 1 == n3 && n3 - 1 == n4) return 2;
	else if (n1 == n3 && n2 == n4 && n1!=n2) return 4;
	else if (n1 - n2 == n2 - n3 && n2 - n3 == n3 - n4) return 5;
	else return 10;
}

int findlev5(int pos) {
	int n1 = str[pos] - '0', n2 = str[pos - 1] - '0';
	int n3 = str[pos - 2] - '0', n4 = str[pos - 3] - '0' , n5 = str[pos-4]-'0';
	if (n1 == n2 && n2 == n3 && n3 == n4 && n4 == n5) return 1;
	else if (n1 + 1 == n2 && n2 + 1 == n3 && n3 + 1 == n4 && n4 + 1 == n5) return 2;
	else if (n1 - 1 == n2 && n2 - 1 == n3 && n3 - 1 == n4 && n4 - 1 == n5) return 2;
	else if (n1 == n3 && n3 == n5 && n2 == n4 && n1 != n2) return 4;
	else if (n1 - n2 == n2 - n3 && n2 - n3 == n3 - n4 && n3 - n4 == n4 - n5) return 5;
	else return 10;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	while (C--) {
		cin >> str;
		int cnt = 0;
		dp[0] = 0; dp[1] = 0;
		dp[2] = findlev3(2);
		dp[3] = findlev4(3);
		dp[4] = findlev5(4);
		dp[5] = dp[2] + findlev3(5);
		dp[6] = min(dp[3] + findlev3(6), dp[2] + findlev4(6));
		for (int i = 7; i < strlen(str); i++) {
			dp[i] = min(dp[i - 3] + findlev3(i), min(dp[i - 4] + findlev4(i), dp[i - 5] + findlev5(i)));
		}
		cout << dp[strlen(str)-1] << '\n';
		for (int i = 0; i < strlen(str); i++) {
			dp[i] = 0;
			str[i] = NULL;
		}
	}
	return 0;
}
```

![](/assets/img/Algorithm/1909181.png)

