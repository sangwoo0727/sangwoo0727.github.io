---
title: "[SWEA1249] 보급로"
categories:
  - Algorithm
tags:
  - Algorithm
  - BFS
comments:
  - true
---
## [SWEA1249] 보급로

[SWEA 1249 문제링크(로그인이 필요합니다.)](https://www.swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15QRX6APsCFAYD)

### 문제설명
* N*N 보드판에 숫자들이 적혀있다.
* (0,0)에서 출발하여, (N-1,N-1)까지 상하좌우로 이동하여 도착할 때, 밟는 숫자들의 합이 최소가 되게 하면 된다.

### 코드리뷰
* BFS로 해결하였다.
* (0,0)에서 (i,j)까지 BFS로 이동하였을 때 최소값을 result[i][j]에 저장한다.
* result배열은 처음에 -1로 전부 초기화 한다.(방문하지 않은 상태)
* 내가 현재 머무는 곳의 좌표를 xnow,ynow라 하고, 바라보고 있는 좌표를 xnext,ynext라고 할 때, result[xnext][ynext]== -1인 경우는 아직 바라보는 좌표를 한번도 방문하지 않은 것이므로 방문한다. 또한 result[xnext][ynext]에 있는 값보다, result[xnow][ynow]+board[xnext][ynext]값이 작은 경우는 result[xnext][ynext]값을 result[xnow][ynow]+board[xnext][ynext]로 갱신한다.
* 이런식으로 BFS를 탐색하면서 result 배열을 채우고 난후 result[N-1][N-1]을 출력하면 된다.
* result배열을 처음에 -1이 아닌, 0으로 초기화 할 경우에, 방문을 안한 곳과 방문을 하였는데 최소합이 0인 곳을 구분하지 못하여 계속해서 방문을 하게 된다. 그렇지만, 최소합이 0인 경우엔 더이상 최소값으로 갱신될 수가 없기 때문에, 방문을 안한 경우는 -1로 초기화하여 구분해준다.

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <utility>
#include <cstring>
using namespace std;

typedef pair<int, int> p;
queue <p> q;
int N;
int board[105][105];
int result[105][105];
int dr[4] = { 0,0,-1,1 };
int dc[4] = { -1,1,0,0 };

void bfs(int i, int j) {
	result[i][j] = board[i][j];
	q.push(make_pair(i, j));
	while (!q.empty()) {
		int inow = q.front().first;
		int jnow = q.front().second;
		q.pop();
		for (int k = 0; k < 4; k++) {
			int inext = inow + dr[k];
			int jnext = jnow + dc[k];
			if (inext >= 0 && inext < N && jnext >= 0 && jnext < N) {
				if (result[inext][jnext] == -1 || result[inext][jnext] > result[inow][jnow] + board[inext][jnext]) {
					result[inext][jnext] = result[inow][jnow] + board[inext][jnext];
					q.push(make_pair(inext, jnext));
				}
			}
		}
	}
}

int main() {
	int T;
	scanf("%d", &T);
	for (int t = 1; t <= T; t++) {
		scanf("%d", &N);
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				scanf("%1d", &board[i][j]);
			}
		}
		memset(result, -1, sizeof(result));
		bfs(0, 0);
		printf("#%d %d\n", t, result[N - 1][N - 1]);
	}
	return 0;
}
```

---
![](/assets/img/Algorithm/201907032.png)

