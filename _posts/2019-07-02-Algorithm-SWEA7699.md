---
title: "[SWEA7699] 수지의 수지 맞는 여행"
categories:
  - Algorithm
tags:
  - Algorithm
  - DFS
comments:
  - true
---
## [SWEA7699] 수지의 수지 맞는 여행

[SWEA 7699 문제링크(로그인이 필요합니다.)](https://www.swexpertacademy.com/main/code/problem/problemSubmitHistory.do?contestProbId=AWqUzj0arpkDFARG)

### 문제설명
1. R*C 보드판에 알파벳이 쓰여있다.
2. (0,0)에서 출발하여 보드판을 상하좌우로 움직일 때, 밟았던 알파벳 종류는 다시 안밟는다고 할 때, 지나갈 수 있는 알파벳의 최대 개수를 구하는 문제이다.

### 코드리뷰
1. DFS로 해결하였다.
2. 처음엔 방문한 곳을 확인하는 check 배열과, 밟은 알파벳 종류를 확인하는 checkalp 배열 두개를 이용해봤는데 어렵게 생각해서 시간초과가 났다. 
3. checkalp[board[n][m]-'A'] 공식으로 밟은 알파벳 종류만 체크해도 방문한 곳을 확인하는 역할을 할 수 있고, 밟은 알파벳 종류또한 확인할 수 있다. 
4. 위 식으로 알파벳 모든 종류를 배열 인덱스로 저장한다고 해도 인덱스 수가 30이면 충분하다.
5. dfs를 통해, 방문한 알파벳방 개수를 저장하며, MAX변수에 개수의 최대값을 저장하면서 해결하였다.

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int R, C;
char board[25][25];
int checkalp[30];
int dr[4] = { 0,0,-1,1 };
int dc[4] = { -1,1,0,0 };
int MAX;

void dfs(int n, int m, int cnt) {
	checkalp[board[n][m]-'A'] = 1;
	for (int k = 0; k < 4; k++) {
		int xnext = n + dr[k];
		int ynext = m + dc[k];
		if (checkalp[board[xnext][ynext]-'A'] == 0 && xnext >= 0 && xnext < R && ynext >= 0 && ynext < C) {
			dfs(xnext, ynext, cnt + 1);
		}
	}
	checkalp[board[n][m] - 'A'] = 0; 
	MAX = max(MAX, cnt);
}


int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	int T;
	cin >> T;
	for (int t = 1; t <= T; t++) {
		cin >> R >> C;
		for (int i = 0; i < R; i++) {
			for (int j = 0; j < C; j++) {
				cin >> board[i][j];
			}
		}
		dfs(0, 0, 1);
		cout << "#" << t << " " << MAX << "\n";
		memset(checkalp, 0, sizeof(checkalp));
		MAX = 0;
	}
	return 0;
}
```

---
![](/assets/img/Algorithm/20190702.png)