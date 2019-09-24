---
title: "[C++] auto 이해하기"
categories:
  - C++
tags:
  - C++
  - auto
comments:
  - true
---
## [C++] auto 이해하기

### auto란
* c++ 11에서 타입 추론.
* auto 키워드는 선언된 변수의 초기화 식을 사용하여 해당 형식을 추론하도록 컴파일러에 지시한다.
* auto 키워드를 사용하면 초깃값의 형식에 맞춰 선언하는 인스턴스(변수)의 형식이 자동으로 결정된다. 이것을 타입추론(type inference)라고 한다.

```cpp
#include <iostream>
using namespace std;

int main() {
	auto sum = 1 + 3;
	cout << sum << endl;
	cout << typeid(sum).name() << endl;
	return 0;
}
```

![](/assets/img/programming_language/201909241.png)

* 위와 같이 auto키워드로 선언된 변수 sum의 자료형이 자동으로 int형으로 결정된 것을 알 수 있다.
* 단 auto 사용에는 주의해야할 점이 있는데 생성 시 변수를 초기화 할때만 작동한다. 초기화 값을 사용하지 않고 생성된 변수는 이 기능을 사용할 수 없다.
* 또한 auto 키워드는 함수 매개 변수와 함께 사용할 수 없다.

### auto를 통한 범위 기반 for문(Ranged-based for loops)

```cpp
//사용 방법
for (element_declaration : array) 
    statement;
```

* 배열 array의 요소를 반복하여 element_declaration에 선언된 변수에 현재 배열 요소의 값을 할당한다.
* element_declaration의 자료형과 array의 배열 요소의 자료형이 다른 자료형이게 되면 형변환이 발생하기 때문에 auto 키워드를 사용하는 것이 이상적이다.

```cpp
#include <iostream>
using namespace std;

int main() {
	int arr[5];
	for (int i = 0; i < 5; i++) arr[i] = i;
	for (auto num : arr) cout << num << ' ';
	return 0;
}
```
* num에는 배열의 인덱스가 아닌 배열의 요소 값이 할당 된다.
* 이런 경우 각 배열의 요소가 num에 복사된다. 배열 요소를 복사하는 것이 많은 비용이 들 수 있으므로 참조를 사용하는 방법을 추천한다.

### Ranged-based for loops and references
* 위의 예제는 num은 값으로 선언되어, 반복되는 각 배열 요소가 num에 복사된다.
* 또한 num을 변경한다고 해서, 배열의 값에 영향을 주지 않는다.
* 참조를 사용하면 비용을 절약할 수 있다.

```cpp
#include <iostream>
using namespace std;

int main() {
	int arr[5];
	for (int i = 0; i < 5; i++) arr[i] = i;
	for (auto &num : arr) cout << num << ' '; //참조를 통한 loop반복
	return 0;
}
```

* num은 현재 반복된 배열 요소에 대한 참조이므로 값이 복사되지 않고 num을 수정하면 배열의 요소에 영향을 준다.
* 읽기 전용인 경우 const로 num을 만들어주는 것을 추천!

```cpp
#include <iostream>
using namespace std;

int main() {
	int arr[5];
	for (int i = 0; i < 5; i++) arr[i] = i;
	for (const auto &num : arr) cout << num << ' ';
	return 0;
}
```

* 포인터로 변환된 배열, 동적 배열 등에서는 크기를 모르기 때문에 범위기반 for 루프는 작동하지 않는다.

---
#### 출처: https://boycoding.tistory.com/210