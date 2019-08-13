---
title: "[알고스팟] FENCE-울타리 잘라내기"
categories:
  - Algorithm
tags:
  - Divide_Conquer
  - Algorithm
comments:
  - true
---

## [알고스팟] FENCE-울타리 잘라내기

[문제 링크](https://algospot.com/judge/problem/read/FENCE)

### 문제 설명

* 많이 등장하는 문제이다. 백준에선 히스토그램에서 가장 큰 직사각형이라는 문제로 존재한다.
* 막대기들을 쭉 세울 때, 막대기 안에 직사각형 모양의 넓이 중 가장 큰 직사각형의 넓이를 구하는 문제이다.

### 코드 리뷰-1
* 예전에 풀어봤던 문제인데, 모든 막대의 범위 중, 가장 높이가 작은 막대기를 선택한 후, 그 막대기를 제외한 후 막대기 기준으로 왼쪽 영역과 오른쪽 영역으로 나눈다. 그리고 선택했던 막대기는 범위 중 가장 높이가 작은 막대기이므로, 전체범위만큼 직사각형을 그릴 수 있다.
* 위에서 구한 직사각형의 넓이와 왼쪽 영역, 오른쪽 영역으로 나누어서 분할 정복으로 푼 넓이 세개를 비교하여 가장 큰 답을 출력한다.
* 원래 기존에 이런 알고리즘으로 풀 때, 가지고 온 범위 중 가장 높이가 작은 막대기를 선택할 경우 세그먼트 트리를 전처리로 만들어 놓고 최소 막대기의 인덱스를 쿼리작업을 통해 가져왔었다. 
* 이번에 풀 때는 전처리 작업을 하지 않고 재귀호출을 할때마다 그 범위만큼 탐색을 하며 가장 높이가 작은 막대기를 선택하였는데, 세그먼트 트리를 사용하지 않을 경우, 이 알고리즘은 큰 문제가 하나 존재한다. 
* 최악의 경우 매번 범위의 가장 끝에서 가장 작은 막대기가 나올 경우, 분할이 1:N-1로 이루어지므로 재귀의 깊이가 N만큼 깊어지게 된다.
* 계속해서 가장 작은 막대기가 중간에 존재할 경우엔 nlgn 시간이 걸리지만, 위의 상황과 같을 땐 최악의 경우에 O(N^2)의 상황을 피하지 못한다.
* 이렇게 짠 알고리즘을 먼저 첨부한다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int minheight(vector <int> &fence, int left, int right) {
	int Min = 100000;
	int idx = 0;
	for (int i = left; i <= right; i++) {
		if (Min > fence[i]) {
			Min = fence[i];
			idx = i;
		}
	}
	return idx;
}
int dq(vector <int> &fence, int idx, int left, int right) {
	if (left >= right) return fence[right];
	if (right <= left) return fence[left];
	int area = fence[idx] * (right - left+1);
	int leftidx = minheight(fence, left, idx-1);
	int rightidx = minheight(fence, idx + 1, right);
	int leftarea = dq(fence, leftidx, left, idx-1);
	int rightarea = dq(fence, rightidx, idx+1, right);
	return max(leftarea, max(rightarea, area));
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	for (int c = 1; c <= C; c++) {
		int N;
		cin >> N;
		vector <int> fence;
		int Min = 100000;
		int idx = 0;
		for (int n = 0; n < N; n++) {
			int h;
			cin >> h;
			fence.push_back(h);
			if (Min > h) {
				Min = h;
				idx = n;
			}
		}
		int ans = dq(fence, idx, 0, N - 1);
		cout << ans << '\n';
		fence.clear();
	}
	return 0;
}
```

### 코드 리뷰-2
* 위의 코드의 경우 왼쪽 재귀와 오른쪽 재귀가 정확히 이쁘게 반반씩 나뉠 땐, nlgn 시간이 걸리지만, 최악의 경우 계속해서 1:N-1로 나누어질 땐, 최소 막대기를 찾는 작업이 재귀깊이 N만큼 이루어지므로 O(nlgn)이 된다.
* 그러므로 위와같은 코드를 짤 때는, 탐색에대한 쿼리시간을 줄이기 위해 세그먼트 트리를 이용하여 전처리 작업을 해줘야 시간이 줄어든다.
* 세그먼트 트리를 사용한 코드는 이전에 히스토그램에서 가장 큰 직사각형이라는 포스팅을 참조하면 된다.
  * [히스토그램에서 가장 큰 직사각형 포스팅](https://sangwoo0727.github.io/algorithm/Algorithm-BOJ6549/)
* 이번에는 재귀의 깊이를 줄이기 위해 조금 다른 방향의 알고리즘을 제시한다.
* 범위안에 최소막대기 인덱스번호를 기준으로 왼쪽 , 오른쪽으로 나누지 않고 항상 N/2,N/2로 나눌 수 있다면 아무리 탐색시간이 N이라 해도 재귀 깊이가 lgN이기 때문에, 항상 O(nlgn)에 풀 수 있게된다.
* 방법은 범위를 반을 나눈 후, 왼쪽 범위에서 나올 수 있는 가장 큰 직사각형의 넓이, 오른쪽 범위에서 나올 수 있는 가장 큰 직사각형의 넓이, 그리고 반을 항상 지나는 직사각형의 넓이 중 가장 큰 값을 반환하면 된다.
* 왼쪽 범위에서 나올 수 있는 가장 큰 직사각형의 넓이와 오른쪽에서 나올 수 있는 가장 큰 직사각형의 넓이는 재귀호출을 통해 작은 문제로 분할하여 풀면 된다.
* 문제는 가운데를 항상 지나는 직사각형의 넓이를 구하는 것인데, 이 직사각형은 왼쪽영역에서 가장 오른쪽에 있는 막대기, 그리고 오른쪽 영역에서 가장 왼쪽에 있는 막대기는 항상 포함하게 된다.
* 그 후, 한 칸씩 보는 막대기를 좌 우로 늘려가며 최대 넓이를 찾아내는 것이다. 이런 경우 트리 높이마다 N만큼 탐색시간이 걸리지만 높이가 logN밖에 되지 않으므로 nlogn 시간에 해결할 수 있다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int dq(vector <int> &fence, int left, int right) {
	if (left == right) return fence[left];
	int mid = (left + right) / 2;
	int ret = max(dq(fence, left, mid), dq(fence, mid + 1, right));
	int ml = mid, mr = mid + 1;
	int height = min(fence[ml], fence[mr]);
	ret = max(ret, height * 2);
	while (left < ml || mr < right) {
		if (mr < right && (ml == left || fence[ml - 1] < fence[mr + 1])) {
			height = min(height, fence[++mr]);
		}
		else {
			height = min(height, fence[--ml]);
		}
		ret = max(ret, height*(mr - ml + 1));
	}
	return ret;
}

int main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int C;
	cin >> C;
	for (int c = 1; c <= C; c++) {
		int N;
		cin >> N;
		vector <int> fence;
		int Min = 100000;
		int idx = 0;
		for (int n = 0; n < N; n++) {
			int h;
			cin >> h;
			fence.push_back(h);
		}
		int ans = dq(fence,0, N - 1);
		cout << ans << '\n';
	}
	return 0;
}
```

![](/assets/img/Algorithm/1908131.png)