---
title: "[C++] new & delete"
categories:
  - C++
tags:
  - C++
  - new&delete
comments:
  - true
---
## [C++] new & delete

* 이 포스팅은 C언어의 malloc과 free를 이해하고, 힙의 메모리 할당 및 소멸에 필요한 함수가 malloc과 free인 것을 알고 있다는 가정하에 진행하는 포스팅입니다.

### malloc & free
* malloc & free를 통한 동적할당 예제를 먼저 보겠다.

```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>
using namespace std;

char * MakeStrAdr(int len) {
	char * str = (char*)malloc(sizeof(char)*len);
	return str;
}

int main() {
	char * str = MakeStrAdr(20);
	strcpy(str, "my name is james");
	cout << str << endl;
	free(str);
	cout << str << endl;
	return 0;
}
```

![](/assets/img/programming_language/1910243.png)

* 이 방법에는 불편한 사항이 있는데, 할당할 대상의 정보를 무조건 바이트 크기 단위로 전달해야 한다.
* C++에서 제공하는 키워드 new와 delete를 사용하면 불편한 사항이 사라진다.

### new & delete
* new는 malloc 함수를 대신하는 키워드이고, delete는 free함수를 대신하는 키워드이다.
* int 형 변수의 할당 : int * ptr1 = new int;
* double 형 변수의 할당 : double * ptr2 = new double;
* 길이가 3인 int 형 배열의 할당 : int * arr1 = new int[3];
* int 형 변수의 소멸 : delete ptr1;
* int 형 배열의 소멸 : delete []arr1;

```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>
using namespace std;

char * MakeStrAdr(int len) {
	char * str = new char[len];
	return str;
}

int main() {
	char * str = MakeStrAdr(20);
	strcpy(str, "my name is james");
	cout << str << endl;
	delete []str;
	cout << str << endl;
	return 0;
}
```

* 이렇듯, malloc & free 역할을 new & delete가 수행할 수 있으며, 코드도 더욱 간결해지는 효과를 가질 수 있다. 
* c++에서는 malloc과 free를 호출하는 일이 없어야 하는데, 그 이유는 이후 포스팅에서 다루겠다. 가볍게 말하면 new와 malloc 함수의 동작 방식에는 차이가 있어 클래스, 객체, 생성자에 대해서 malloc 함수 호출은 문제가 될수도 있기 때문이다.

#### 출처 : 윤성우의 열혈 C++ 프로그래밍 , 오렌지 미디어
