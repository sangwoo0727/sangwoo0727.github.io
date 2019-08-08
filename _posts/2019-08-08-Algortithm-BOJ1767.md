---
title: "[SWEA1767] 프로세서 연결하기"
categories:
  - Algorithm
tags:
  - DFS
comments:
  - true
---
## [SWEA1767] 프로세서 연결하기

[문제 링크 - 로그인이 필요합니다.](https://www.swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV4suNtaXFEDFAUf)

### 문제 설명
![](/assets/img/Algorithm/08001.png)
* 멕시노스 판에 코어들이 주어진다. 코어가 있는 배열 값은 1, 없는 배열 값은 0.
* 멕시노스 판 가장자리는 전원이 흐르는 곳이다.
* 코어들을 가장자리까지 연결선을 통해 연결할 때, 연결선들끼리는 위의 그림과 같이 교차할 수 없다.
* 이렇게 코어들을 가장자리 전원까지 연결할 때, 연결선 길이의 최소를 구하는 문제이다.
* 단 배열의 가장자리에 있는 코어들은 이미 전원이 연결되어있는 상태이고, 모든 코어들을 전부 전원에 연결할 필요는 없다.
* 가장 많은 코어들을 전원에 연결할 때, 연결선 길이의 최소를 구하면 된다.

### 코드리뷰
* 코어의 최대 개수는 12개이고, 각각의 코어는 네방향으로 연결선을 둘 수 있으므로, 최대 4^12의 경우의 수가 나온다. 1500만 정도 이므로 시간초과에 대해서 걱정할 필요는 없다.
* 처음에 근데 시간초과가 났었는데, 중복되게 새는 경우가 존재했다. 네 방향에 대해 전선을 보내는 경우 말고, 그 코어에서 연결선을 붙히지 않는 경우에 대해 또다시 DFS를 들어갔었는데, 네 방향에 대해 전선을 보내지 못하는 경우와 중복되었다.
* 위의 부분을 해결하고 나니 시간초과 없이 통과는 하였는데 900ms정도가 나왔다.
* 최적화를 위해 고민하던 중 애초에 보드의 가장자리에 있는 코어들은 dfs 탐색 조건에서 배제시키면 된다는 생각이 떠올랐다. 이 부분을 해결하고 나니 58ms정도로 해결하였다.
* 문제 푸는데 걸린시간 1시간 20분.

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;

typedef struct {
	int line_leng;
	int core_cnt;
}Result;

typedef struct {
	int n;
	int m;
}Core;

Core core[13];
Result ans;
int board[13][13];
int check[13][13];
int dc[4] = { 0,0,-1,1 };
int dr[4] = { -1,1,0,0 };
int pos;
int N;

void dfs(int idx, int ccnt, int leng) {
	if (idx == pos) {
		if (ans.core_cnt == 0 && ans.line_leng == 0) {
			ans.core_cnt = ccnt;
			ans.line_leng = leng;
			return;
		}
		else if (ans.core_cnt > ccnt) {
			return;
		}
		else if (ans.core_cnt == ccnt) {
			ans.line_leng = min(ans.line_leng, leng);
			return;
		}
		else if (ans.core_cnt < ccnt) {
			ans.core_cnt = ccnt;
			ans.line_leng = leng;
			return;
		}
	}
	else {
		int n = core[idx].n;
		int m = core[idx].m;
		for (int k = 0; k < 4; k++) {
			int tmp = 0;
			for (int i = 1; i < N; i++) {
				int nnext = n + i * dc[k];
				int mnext = m + i * dr[k];
				if (nnext < 0 || nnext >= N || mnext < 0 || mnext >= N) break;
				else if (i == 1 && (check[nnext][mnext] == 1 || board[nnext][mnext] == 1)) {
					dfs(idx + 1, ccnt, leng);
					break;
				}
				else if (i > 1 && (check[nnext][mnext] == 1 || board[nnext][mnext] == 1)) {
					break;
				}
				else {
					if (nnext == 0 || nnext == N - 1 || mnext == 0 || mnext == N - 1) {
						dfs(idx + 1, ccnt + 1, leng + i);
						tmp++;
						check[nnext][mnext] = 1;
						break;
					}
					tmp++;
					check[nnext][mnext] = 1;
				}
			}
			for (int i = 1; i <= tmp; i++) {
				int nnext = n + i * dc[k];
				int mnext = m + i * dr[k];
				check[nnext][mnext] = 0;
			}
		}
	}
	return;
}
int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int T;
	cin >> T;
	for (int t = 1; t <= T; t++) {
		for (int i = 0; i < 13; i++) {
			core[i].n = 0; core[i].m = 0;
		}
		ans.core_cnt = 0; ans.line_leng = 0;
		memset(check, 0, sizeof(check));
		memset(board, 0, sizeof(board));
		pos = 0;
		cin >> N;
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				cin >> board[i][j];
				if (board[i][j] == 1) {
					if (i == 0 || i == N - 1 || j == 0 || j == N - 1) {
						check[i][j] = 1;
						continue;
					}
					core[pos].n = i;
					core[pos].m = j;
					check[i][j] = 1;
					pos++;
				}
			}
		}
		dfs(0, 0, 0);
		cout << "#" << t << " " << ans.line_leng << '\n';
	}
	return 0;
}
```

![](/assets/img/Algorithm/08082.png)