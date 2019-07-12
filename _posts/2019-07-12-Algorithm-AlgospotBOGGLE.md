---
title: "[알고스팟] BOGGLE-보글게임"
categories:
  - Algorithm
tags:
  - DP
  - BRUTEFORCE
comments:
  - true
---
## [알고스팟] BOGGLE-보글게임

[문제 링크](https://algospot.com/judge/problem/read/BOGGLE)

### 문제설명
* 보드판 한칸 한칸에 대문자 알파벳이 쓰여있다.
* 입력받은 영단어에 대해 보드판을 상하좌우,대각선으로 움직이면서 그 영단어를 나타낼 수 있는지 확인하는 문제이다.
  
### 코드리뷰
* 기본 알고리즘은 완전 탐색으로 해결한다. 허나 단순히 완전 탐색으로 할 경우 시간초과가 발생한다.
* DP를 사용하여야 하는데, 디피테이블은 3차원인 DP[5][5][10]으로 만든다. 입력받는 문자의 단어 수가 10개 이하이기 때문에 10으로 설정.
* DP테이블을 -1로 초기화 한 후, 보드판을 움직이면서 단어를 못만들게 될 경우, 단어의 알파벳이 몇번째나오는지 확인 후, 만약 n번째에 알파벳에서 진행을 못하게 될 경우, DP[][][n]에 0을 대입한다.
* 이렇게 3차원으로 하는 이유는, 다른 곳에서 출발하였을 때 그 알파벳이 n번째가 아닐 경우엔 진행을 해야하기 때문.

```cpp
#include <iostream>
#include <string>
#include <cstring>
using namespace std;

string s[10];
char board[5][5];
int check[5][5][10];
int len[10];
int dr[8] = { 0,1,1,1,0,-1,-1,-1 };
int dc[8] = { -1,-1,0,1,1,1,0,-1 };

int dfs(int i, int j, int pos, int n) {
	if (pos == len[n]-1) {
		check[i][j][pos] = 1;
		return 1;
	}
	for (int k = 0; k < 8; k++) {
		int xnow = i + dr[k];
		int ynow = j + dc[k];
		if (xnow < 0 || xnow >= 5 || ynow < 0 || ynow >= 5) continue;
		else if (board[xnow][ynow] == s[n][pos + 1]) {
			if (check[xnow][ynow][pos + 1] == 1) {
				return 1;
			}
			else if (check[xnow][ynow][pos + 1] == 0) {
				continue;
			}
			else {
				check[xnow][ynow][pos+1] = dfs(xnow, ynow, pos + 1, n);
				if (check[xnow][ynow][pos + 1] == 1) return 1;
			}
		}
	}
	return 0;
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int C;
	cin >> C;
	for (int c = 1; c <= C; c++) {
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				cin >> board[i][j];
			}
		}
		int N;
		cin >> N;
		for (int i = 0; i < N; i++) {
			cin >> s[i];
			len[i] = s[i].length();
		}
		for (int n = 0; n < N; n++) {
			int result = 0;
			memset(check, -1, sizeof(check));
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					if (board[i][j] == s[n][0]) {
						result = dfs(i, j, 0, n);
						if (result == 1) break;
					}
				}
				if (result == 1) break;
			}
			cout << s[n];
			if (result == 1) cout << " YES" << '\n';
			else if (result == 0) cout << " NO" << '\n';
		}
	}
	return 0;
}
```

![](/assets/img/Algorithm/1907121.png)
