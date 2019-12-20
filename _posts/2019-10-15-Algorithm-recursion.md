---
title: "[알고리즘설계] 재귀를 이용한 순열,조합"
categories:
  - Algorithm
tags:
  - recursion
  - permutation
  - combination
comments:
  - true
---

## [알고리즘설계] 재귀를 이용한 순열,조합
* 복습 겸 재귀를 이용하여 순열과 조합함수를 만들며 공부를 하였다.

### 1. 순열(permutation)
* 순열은 서로 다른 N개의 숫자 중 R개를 순서대로 나열하는 것
* 기호는 nPr

#### 로직
1. 순서를 고려하여야 한다. 1,3 과 3,1은 다른 경우의 순열
2. 중복으로 뽑으면 안된다. -> 해당 숫자를 사용했음을 표시하기 위한 배열 필요

#### cpp를 통한 순열 구현

```cpp
void permutation(int r) {
	if (r == R) { 
		cnt++;
		for (int k = 0; k < R; k++) {
			cout << arr[k] << ' ';
		}
		cout << endl;
		return;
	}
	for (int k = 0; k < N; k++) {
		if (!check[k]) {
			check[k] = true;
			arr[r] = k;
			permutation(r + 1);
			check[k] = false;
		}
	}
}
```

![](/assets/img/Algorithm/201910151.png)

* 출력결과의 하단 출력화면, 마지막 숫자는 순열을 통해 구한 가지수(입력값에 5,3을 넣었고 가지수는 60가지가 나온다)

### 2. 중복 순열(repeated permutation)
* 중복 순열은 서로 다른 N개의 숫자 중 R개를 뽑아 순서대로 나열할 때, 숫자들의 중복을 허용.
* 순열과 똑같은 로직인데 다른 점은 숫자의 중복을 허용한다는 것이다.

#### 로직
1. 중복 순열은 순열과 같이 숫자들의 나열 순서를 고려해야 한다.
2. 중복을 허용하여, 한번 뽑은 숫자를 또 뽑을 수 있다 -> 해당 숫자를 사용했다는 체크 배열 필요 없음.

#### cpp를 통한 중복 순열 구현

```cpp
void rep_permutation(int r) {
	if (r == R) { 
		cnt++;
		for (int k = 0; k < R; k++) {
			cout << arr[k] << ' ';
		}
		cout << endl;
		return;
	}
	for (int k = 0; k < N; k++) {
		arr[r] = k;
		rep_permutation(r + 1);	
	}
}
```

![](/assets/img/Algorithm/201910152.png)

* 출력결과의 하단 출력화면, 마지막 숫자는 중복 순열을 통한 가지수(125가지)가 나온다. 첫번째, 두번째, 세번째 뽑을 때 모두 5가지 숫자 중 아무거나 뽑을 수 있으므로, 가지 수는 125가지.

### 3. 조합(Combination)
* 조합은 서로 다른 N개의 숫자 중 R개를 뽑는 경우의 수이다. 뽑는 순서는 상관없다.(즉, 1,3을 뽑는 경우와 3,1을 뽑는 경우는 같은 경우)

#### 로직
1. 순서가 상관없이 무엇을 뽑을지만 체크해주고 다음 인덱스로 넘어가면 된다.
2. 이 숫자를 사용할지 말지를 결정한 후 다음 함수로 넘어가면 된다.

#### cpp를 통한 조합 구현

```cpp
void combination(int r,int idx) {
	if (r == R) { 
		cnt++;
		for (int k = 0; k < N; k++) {
			if (check[k]) {
				cout << k << ' ';
			}
		}
		cout << endl;
		return;
	}
	if (idx >= N) return;
	check[idx] = 1;
	combination(r + 1, idx + 1);
	check[idx] = 0;
	combination(r, idx + 1);
}
```

![](/assets/img/Algorithm/201910153.png)

### 4. 중복 조합
* 중복 조합은 서로 다른 N개의 숫자 중 중복을 허용하여 r개를 뽑는 경우이다.
* 숫자를 뽑을지 말지를 결정하고 다음 함수로 넘어가면 된다.

#### cpp를 통한 중복 조합 구현

```cpp
void rep_combination(int r,int n) {
	if (r == R) { 
		cnt++;
		for (int k = 0; k < R; k++) {
			cout << arr[k] << ' ';
		}
		cout << endl;
		return;
	}
	if (n >= N) return;
	arr[r] = n;
	rep_combination(r + 1, n);
	rep_combination(r, n + 1);
}
```

![](/assets/img/Algorithm/201910154.png)

* 중복조합의 개수구하는 공식은 H(n,r) = C(n+r-1,r).

