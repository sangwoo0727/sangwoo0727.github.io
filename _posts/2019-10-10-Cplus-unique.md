---
title: "[C++] unique를 이용한 벡터의 중복원소 제거"
categories:
  - C++
tags:
  - C++
  - unique
comments:
  - true
---
## [C++] unique를 이용한 벡터의 중복원소 제거

### unique 함수란?
* vector 배열에서 중복되지 않는 원소들을 앞에서부터 채워나가는 함수이다.
* algorithm 헤더에 존재한다.
* 중복되지 않는 원소들을 앞에서부터 채워나가는 역할을 하기때문에 남은 뒷부분은 그대로 vector 원소값이 존재한다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	vector <int> v;
	v.push_back(1); v.push_back(1);
	v.push_back(2);
	v.push_back(3); v.push_back(3);
	v.push_back(4);
	v.push_back(5); v.push_back(5); v.push_back(5);
	v.push_back(6);

	cout << "***** 기존 벡터배열 원소 *****" << endl;
	for (const auto& n : v) cout << n << ' ';
	cout << endl;

	cout << "***** unique 함수 적용 *****" << endl;
	unique(v.begin(), v.end());
	for (const auto& n : v) cout << n << ' ';
	cout << endl;
	
	return 0;
}
```

![](/assets/img/Algorithm/201910101.png)

* unique 함수를 적용하면 위와 같이 중복된 원소를 제거하며 앞에서부터 원소들을 채워나간다. 
* 그렇게 되면 원래 배열보다 원소의 개수가 4개 줄어들고 원래 벡터 배열의 나머지 자리엔 기존 벡터배열 원소의 값이 채워지게된다.
* 그러면 줄어든 개수를 알면 원래 개수에서 줄어든 범위를 뺀 개수만큼만 확인하면 중복된 원소를 제거한 배열을 출력할 수 있을 것이다.
* 하지만 문제를 풀거나 보통의 상황에선 몇 개가 중복되고 몇 개의 원소가 줄어든지를 알 수 없기때문에 erase 함수를 사용하여 뒷 부분에 필요없는 값들을 삭제시켜 줄것이다.

### erase 함수를 이용하여 필요한 값만 남기기
* erase 함수는 vector 배열에서 특정 원소를 삭제하는 함수이다.
* v.erase(v.begin()+s, v.begin()+e) 명령어를 입력하면 [s,e) 원소가 삭제된다. 즉 시작 지점은 닫힌구간, 끝나는 지점은 열린 구간으로 삭제된다.
* 이를 적용하여 우리는 unique 함수를 적용한 벡터배열에서 필요한 원소만 뽑아낼 수 있다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	vector <int> v;
	v.push_back(1); v.push_back(1);
	v.push_back(2);
	v.push_back(3); v.push_back(3);
	v.push_back(4);
	v.push_back(5); v.push_back(5); v.push_back(5);
	v.push_back(6);

	cout << "***** 기존 벡터배열 원소 *****" << endl;
	for (const auto& n : v) cout << n << ' ';
	cout << endl;

	cout << "***** erase와 unique 같이 사용하기 *****" << endl;
	v.erase(unique(v.begin(), v.end()), v.end());
	for (const auto& n : v) cout << n << ' ';
	cout << endl;
	return 0;
}
```

![](/assets/img/Algorithm/201910102.png)

* 사진처럼 중복된 원소를 제거하고 원하는 원소들만 출력할 수 있게된다!
* n개의 원소에 대한 unique 함수의 시간복잡도는 O(n)이다.