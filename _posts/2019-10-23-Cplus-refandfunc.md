---
title: "[C++] 참조자(Reference)와 함수"
categories:
  - C++
tags:
  - C++
  - Reference
comments:
  - true
---
## [C++] 참조자(Reference)와 함수

### Call-by-value & Call-by-reference
* call-by-value : 값을 인자로 전달하는 함수의 호출방식
* call-by-reference : 주소 값을 인자로 전달하는 함수의 호출방식
* 이 두가지에 대해서는 c,c++를 조금이라도 공부한 사람은 잘 알고 있을 것이다.
* 예시를 통해 복습 한번만 하고 진행하겠다!

```cpp
////Call-by-value/////
#include <iostream>
using namespace std;

void swap(int n1, int n2) {
	int temp = n1;
	n1 = n2;
	n2 = temp;
}

int main() {
	int num1 = 3, num2 = 5;
	swap(num1, num2);
	cout << num1 << ' ' << num2 << endl;
	return 0;
}
```

* 위의 경우 num1과 num2의 값을 인자로 전달하였기 때문에 함수가 끝나고 num1과 num2를 출력하더라도 둘의 값이 서로 바뀌지 않음을 확인할 수 있다.

```cpp
/////Call-by-reference/////
#include <iostream>
using namespace std;

void swap(int * n1, int * n2) {
	int temp = *n1;
	*n1 = *n2;
	*n2 = temp;
}

int main() {
	int num1 = 3, num2 = 5;
	swap(&num1, &num2);
	cout << num1 << ' ' << num2 << endl;
	return 0;
}
```

* 위의 경우 num1과 num2의 주소 값을 인자로 전달하여, 그 주소 값이 참조하는 영역에 저장된 값을 직접 변경하고 있다. 그러므로 함수가 끝난 후 num1과 num2에 저장된 값이 바뀌어 있는 것을 확인할 수 있다.

### Call-by-address ?
* 최근 들어, 참조자 기반의 함수 호출과 구분을 위해 주소 값을 전달하는 Call-by-reference 형태의 함수 호출을 Call-by-address라고 불리는 경우가 있다.
* c언어에서 Call-by-reference의 의미는 __주소값을 전달받아서, 함수 외부에 선언된 변수에 접근하는 형태의 함수 호출__ 이다.
* 즉, 주소 값이 외부 변수의 참조도구로 사용되는 함수 호출을 뜻한다.
* 이 말은 주소 값이 전달되었다는 사실이 중요한 것이 아니라, 주소 값이 참조의 도구로 사용되었다는 사실이 중요한 것이고, 그렇기에 Call-by-reference라고 불리는 것.
* 그렇기에 이 포스팅에서는 Call-by-address라는 표현보다는 __주소 값을 이용한 Call-by-refernce__ 와 __참조자를 이용한 Call-by-reference__ 로 구분하여 사용하겠다.

### 참조자를 이용한 Call-by-reference
* C++에서는 참조자를 기반으로도 Call-by-reference의 함수 호출을 진행할 수 있다.
* Call-by-reference의 가장 큰 핵심은 __함수 내에서 함수 외부에 선언된 변수에 접근할 수 있다는 것__
* 아래 코드를 보겠다.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void cbv(vector <int> val) {
	for (int i = 0; i < 5; i++) {
		val[i]++;
	}
}
int main() {
	vector <int> v;
	for (int i = 1; i <= 5; i++) {
		v.push_back(i);
	}
	for (int i = 0; i < 5; i++) {
		cout << v[i] << ' ';
	}
	cout << endl;
	cbv(v);
	for (int i = 0; i < 5; i++) {
		cout << v[i] << ' ';
	}
	cout << endl;
	return 0;
}
```

![](/assets/img/programming_language/1910242.png)

* 이 경우 단순히 call-by-value 형태로 함수를 호출한 방식이고, 함수가 끝난 후 배열을 확인해보아도 v벡터의 값이 변하지 않는 것을 알 수 있다.


```cpp
#include <iostream>
#include <vector>
using namespace std;

void cbr_ref(vector <int>& ref) {
	for (int i = 0; i < 5; i++) {
		ref[i]++;
	}
}
int main() {
	vector <int> v;
	for (int i = 1; i <= 5; i++) {
		v.push_back(i);
	}
	for (int i = 0; i < 5; i++) {
		cout << v[i] << ' ';
	}
	cout << endl;
	cbr_ref(v);
	for (int i = 0; i < 5; i++) {
		cout << v[i] << ' ';
	}
	cout << endl;
	return 0;
}
```

![](/assets/img/programming_language/1910241.png)

* 이 경우는 참조자를 이용한 Call-by-reference 형태로 참조자를 통하여 함수 외부에 선언된 배열에 접근할 수 있다.
* 여기서 의문점을 가질 수 있는 부분이 바로 __참조자는 선언과 동시에 변수로 초기화하여야 한다__ 는 것이다.
* 하지만 매개변수는 함수가 호출되어야 초기화가 진행되는 변수들이고, 매개변수의 선언은 초기화가 이루어지지 않은 것이 아니라, 함수 호출 시 전달되는 인자로 초기화 하겠다는 의미의 선언.


#### 출처 : 윤성우의 열혈 C++ 프로그래밍 , 오렌지 미디어