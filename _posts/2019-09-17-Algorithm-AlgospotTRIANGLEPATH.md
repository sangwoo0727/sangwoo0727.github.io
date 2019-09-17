---
title: "[알고스팟] TRIANGLEPATH-삼각형 위의 최대경로"
categories:
  - Algorithm
tags:
  - DynamicProgramming
comments:
  - true
---

## [알고스팟] TRIANGLEPATH-삼각형 위의 최대경로


[문제 링크](https://algospot.com/judge/problem/read/TRIANGLEPATH)

### 문제 설명
* 정사각형(N*N) 배열 중 각 N개의 줄에 대해 1~N개의 숫자로 가로줄의 왼쪽부터 수를 채워나간다.
* 즉 첫 줄은 1개, 2번째 줄은 2개, 3번째 줄은 3개 ... 로 삼각형 모양으로 배열을 채운다.
* 1,1에서 출발하여 N번째 줄에 있는 값 까지 도착할 때 움직일 수 있는 경로는 바로 아래숫자 혹은 바로 아래의 오른쪽 숫자로 내려갈 수 있다.
* 이 때 모든 경로 중 숫자의 최대 합 경로를 찾는 문제이다.

### 코드 리뷰
* 모든 가지 수의 경로 중 최대 경로, 즉 최장 경로를 찾는 문제는 기본적으로 NP문제이다.
* 하지만 최장 경로 문제 중 DAG에 대해서는 DP로 풀 수 있다.
* 그렇다면 이 문제에서 주어진 그래프가 DAG일 경우 DP를 이용하여 빠른시간내에 풀 수 있을 것이다.
* 배열을 그래프 모양으로 보기 쉽게 나타내보았다.
  
![](/assets/img/Algorithm/201909171.png)

* 덱이 눈에 보인다면 문제는 다 풀었다고 볼 수 있다. 위 그래프는 DAG, 즉 사이클이 없는 방향 그래프이다.
* DAG에서의 최장경로 문제라는것을 파악했으므로 DP를 사용하여 현재 위치까지의 최장 경로를 앞부분부터 채워나가면서 마지막 N번째 줄에 있는 경로의 끝점들 중 최대 값을 반환하면 해결할 수 있다. 

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;

int board[105][105];
int dp[105][105];
int result;

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	while (C--) {
		int n;
		cin >> n;
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= i; j++) {
				cin >> board[i][j];
			}
		}
		dp[1][1] = board[1][1];
		for (int i = 2; i <= n; i++) {
			for (int j = 1; j <= i; j++) {
				if (j == 1) {
					dp[i][j] = dp[i - 1][j] + board[i][j];
					if (i == n) result = max(result, dp[i][j]);
				}
				else {
					dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1]) + board[i][j];
					if (i == n) result = max(result, dp[i][j]);
				}
			}
		}
		cout << result<< "\n";
		result = 0;
		memset(board, 0, sizeof(board));
		memset(dp, 0, sizeof(dp));
	}
	return 0;
}
```

  ![](/assets/img/Algorithm/201909172.png)