---
title: "[알고스팟] BOARDCOVER-게임판 덮기"
categories:
  - Algorithm
tags:
  - DFS
  - Algorithm
comments:
  - true
---

## [알고스팟] BOARDCOVER-게임판 덮기

[문제 링크](https://algospot.com/judge/problem/read/BOARDCOVER)

### 문제설명

* 보드판을 #과 .을 이용하여 표기한다.
* .인 부분을 ㄱ 자 모양의 블록을 이용하여 덮을 때, 덮을 수 있는 가지수를 구하는 문제이다.
* ㄱ자 모양의 블록은 자유롭게 회전할 수 있다. 
* 하지만, 블록을 겹치거나, # 부분에 걸쳐지거나 보드판 밖으로 블록이 나가서는 안된다.

### 코드리뷰
* ㄱ자 블록은 총 3칸의 보드판을 차지할 수 있다. ㄱ자 블록의 3칸 중 중심이 되는 점은 ㄱ자블록의 사이드에 있는 경우와 ㄱ자블록의 가운데가 되는 경우가 있다.
* 그 점을 기준으로 만들 수 있는 블록모양의 총 가지수는 12가지가 된다.
* 하지만 이런 12 방향의 모든 블록의 가지수를 고려하여 문제를 풀게되면, 중복이 되는 경우가 무수하게 발생하게 되고, 시간초과가 발생한다. 문제 풀다가 이부분에서 막혔다.
* 결국 블록을 이용하여 보드판의 가장 왼쪽, 그리고 가장 위쪽부터 채워나가는 식으로 문제를 풀게되면, 블록을 회전시켜 만들 수 있는 모양은 총 4가지로 줄게된다. 
* 이렇게 풀게되면 시간초과없이 문제를 풀 수 있다.

```cpp
#include <iostream>
using namespace std;

int shape[4][3][2] = { {{0,0},{0,1},{1,1}},
					   {{0,0},{1,0},{1,1}},
					   {{0,0},{1,0},{1,-1}},
					   {{0,0},{1,0},{0,1}} };
int check[21][21];
char board[21][21];
int H, W;

bool checkshape(int k, int x, int y) {
	for (int i = 0; i < 3; i++) {
		int nx = x + shape[k][i][0];
		int ny = y + shape[k][i][1];
		if (nx < 0 || nx >= H || ny < 0 || ny >= W || check[nx][ny]==1 || board[nx][ny]!='.') return false;
	}
	return true;
}

int dfs() {
	int x = -1; int y = -1;
	for (int i=0; i < H; i++) {
		for (int j = 0; j < W; j++) {
			if (board[i][j] == '.' && check[i][j]==0) {
				x = i; y = j;
				break;
			}
		}
		if (x != -1 && y != -1) break;
	}
	if (x == -1 && y == -1) return 1;
	int result = 0;
	for (int k = 0; k < 4; k++) {
		if (checkshape(k, x, y)) {
			for (int i = 0; i < 3; i++) {
				int nx = x + shape[k][i][0];
				int ny = y + shape[k][i][1];
				check[nx][ny] = 1;
			}
			result += dfs();
			for (int i = 0; i < 3; i++) {
				int nx = x + shape[k][i][0];
				int ny = y + shape[k][i][1];
				check[nx][ny] = 0;
			}
		}
	}
	return result;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	for (int c = 1; c <= C; c++) {
		cin >> H >> W;
		for (int i = 0; i < H; i++) {
			for (int j = 0; j < W; j++) {
				cin >> board[i][j];
			}
		}
		cout << dfs() << '\n';
	}
	return 0;
}
```

![](/assets/img/Algorithm/190723.png)
