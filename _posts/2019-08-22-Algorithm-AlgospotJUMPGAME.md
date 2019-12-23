---
title: "[알고스팟] JUMPGAME-외발 뛰기"
categories:
  - Algorithm
tags:
  - DynamicProgramming
comments:
  - true
toc: true
toc_sticky: true
---

## [알고스팟] JUMPGAME-외발 뛰기

[문제 링크](https://algospot.com/judge/problem/read/JUMPGAME)

### 문제 설명
* 0,0 배열에서 시작하여 그 칸에 적힌 숫자만큼 아래나 오른쪽으로 이동한다.
* n-1,n-1까지 이런식으로 도달할수 있는지 없는지에 대한 판단을 내리는 문제이다.

### 코드 리뷰
* DP문제인데, 내가 푼 코드와 답안을 본 코드의 간결함의 차이에 좌절감을 맛봤다..
* 접근은 비슷하다. 체크배열을 통해 그 칸에 다시 접근하였을 때 갈 수 있는 결과인지 없는 결과인지를 저장해두고 그 결과를 재사용 한다.
* 푸는데 걸린시간 25분.
* 변수의 주소값 자체를 카피해서 사용하는 int& ret 변수에 익숙치 않은데 연습을 자주하여 코드를 간결하게 만드는데 익숙해져야겠다.
* 처음 내가 푼 코드.

```cpp
#include <iostream>
using namespace std;

typedef struct {
	int down;
	int right;
}Check;

Check check[101][101];
int board[101][101];
int N;
int ans;

void jump(int n, int m) {
	int k = board[n][m];
	if (n == N - 1 && m == N - 1) {
		ans = 1;
		return;
	}
	if (check[n][m].down == 0) {
		int nnext = n + k;
		if (nnext > N - 1 || (check[nnext][m].down == -1 && check[nnext][m].right == -1))
			check[n][m].down = -1;
		if (nnext < N && (check[nnext][m].down != -1 || check[nnext][m].right != -1)) {
			jump(nnext, m);
			if (ans == 1) return;
		}
	}
	if (check[n][m].right == 0) {
		int mnext = m + k;
		if (mnext > N - 1 || (check[n][mnext].down == -1 && check[n][mnext].right == -1))
			check[n][m].right = -1;
		if (mnext < N && (check[n][mnext].down != -1 || check[n][mnext].right != -1)) {
			jump(n, mnext);
			if (ans == 1) return;
		}
	}
}
int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	while (C--) {
		cin >> N;
		ans = 0;
		for (int n = 0; n < N; n++) {
			for (int m = 0; m < N; m++) {
				cin >> board[n][m];
			}
		}
		jump(0, 0);
		if (ans == 1) cout << "YES" << "\n";
		if (ans == 0) cout << "NO" << "\n";
		for (int n = 0; n < N; n++) {
			for (int m = 0; m < N; m++) {
				check[n][m].down = check[n][m].right = 0;
			}
		}
	}
	return 0;
}
```

* ret을 이용한 코드

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int check[101][101];
int board[101][101];
int N;

int jump(int n, int m) {
	int k = board[n][m];
	if (n == N - 1 && m == N - 1) return 1;
	if (n > N - 1 || m > N - 1) return 0;
	int& ret = check[n][m];
	if (ret != -1) return ret;
	return ret = (jump(n + k, m) || jump(n, m + k));
}
int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	while (C--) {
		cin >> N;
		for (int n = 0; n < N; n++) {
			for (int m = 0; m < N; m++) {
				cin >> board[n][m];
			}
		}
		memset(check, -1, sizeof(check));
		int ans = jump(0, 0);
		if (ans == 1) cout << "YES" << "\n";
		if (ans == 0) cout << "NO" << "\n";
	}
	return 0;
}
```

![](/assets/img/Algorithm/08221.png)