---
title: "[알고스팟] CLOCKSYNC-시계맞추기"
categories:
  - Algorithm
tags:
  - BRUTEFORCE
  - Algorithm
comments:
  - true
toc: true
toc_sticky: true
---

## [알고스팟] CLOCKSYNC-시계맞추기

[문제 링크](https://algospot.com/judge/problem/read/CLOCKSYNC)

### 문제설명
* 이 문제는 배열 한칸한칸에 시계가 있고 그 시계는 12,3,6,9시 중 한 곳을 가리키고 있다.
* 그리고, 스위치 10개가 주어지는데 각 스위치에는 움직일 수 있는 시계번호(배열번호)가 3~5개 들어있다.
* 스위치를 한 번 누를때, 스위치에 속해있는 시계들은 3시간씩 움직인다.
* 스위치를 몇 번 누를때, 배열에 있는 시계들이 모두 12시를 가리키는지에 대한 횟수를 구하는 문제이다. 만약 절대 모든 시계들이 12시를 가리키지 못할 땐 -1을 출력한다.

### 코드리뷰
* 처음 재귀로 접근하였을 때, 스위치를 누르는 순서에 대한 중복을 생각하지 못하고, 풀었다가 시간 초과가 났다.
* 각 스위치는 0~3번까지만 누르게 하면된다. 4번누르는 경우는 한번도 안누른 경우와 같다.
* 0번 스위치부터 시작하여, 각 스위치를 0번,1번,2번,3번 눌렀을 때로 나누어 재귀로 들어가 다음 스위치를 확인한다.
* 이렇게하면, 순서의 중복을 해결할 수 있고, 제한시간내에 풀 수 있다.
* 풀고나서 다른 사람풀이를 보니, 수학적으로 접근하여 0ms에 나오는 풀이들이 있었는데, 다음에 그 방법으로 풀어봐야겠다. 일단 문제 자체에서 시간제한을 10초로 주었고, 완전탐색이 출제의도라고 생각하고 완전탐색으로 풀어서 통과하였다.

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int check[16];
int board[16];
int moving[10][5] = { {0,1,2,-1,-1},{3,7,9,11,-1},{4,10,14,15,-1},{0,4,5,6,7},{6,7,8,10,12},
					{0,2,14,15,-1},{3,14,15,-1,-1},{4,5,7,14,15},{1,2,3,4,5},{3,4,5,9,13} };
int Min= 1000000000;
int movcnt[10];

void dfs(int swit, int cnt) {
	if (cnt >= Min) return;
	if (swit > 9) return;
	for (int i = 0; i < 16; i++) {
		if (check[i] % 4 != 0) break;
		else if (i < 15 && check[i] % 4 == 0) continue;
		else if (i == 15 && check[i] % 4 == 0) {
			Min = min(Min, cnt);
			return;
		}
	}
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 5; j++) {
			if (moving[swit][j] != -1) {
				check[moving[swit][j]] +=i;
			}
		}
		dfs(swit + 1, cnt + i);
		for (int j = 0; j < 5; j++) {
			if (moving[swit][j] != -1) {
				check[moving[swit][j]] -= i;
			}
		}
	}
}
int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	for (int c = 1; c <= C; c++) {
		for (int i = 0; i < 16; i++) {
			cin >> board[i];
			if (board[i] == 12) check[i] = 0;
			else if (board[i] == 3) check[i] = 1;
			else if (board[i] == 6) check[i] = 2;
			else if (board[i] == 9) check[i] = 3;
		}
		if (board[8] != board[12]) cout << "-1" << '\n';
		else if (board[14] != board[15]) cout << "-1" << '\n';
		else {
			dfs(0,0);
			if (Min == 1000000000) Min = -1;
			cout << Min << '\n';
			Min = 1000000000;
			memset(check, 0, sizeof(check));
			memset(movcnt, 0, sizeof(movcnt));
		}
	}
	return 0;
}
```

![](/assets/img/Algorithm/190726.png)