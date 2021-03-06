---
title: "[C++] 참조자(Reference) 이해하기"
categories:
  - C++
tags:
  - C++
  - Reference
comments:
  - true
toc: true
toc_sticky: true
---
## [C++] 참조자(Reference) 이해하기

### 참조자란
* 변수란 할당된 메모리 공간에 붙여진 이름.
* 여기에 이름을 더 부여할 수 있다(별칭같은 느낌)
* 참조자란 자신이 참조하는 변수를 대신할 수 있는 또 하나의 이름.

```cpp
#include <iostream>
using namespace std;

int main() {
	int num = 3;
	int &num2 = num;
	cout << num2;
    num2+=1;
    cout << num;
    return 0;
}
```

* 위의 코드를 실행시켜보면 num이라는 변수에 3을 할당했고, num에 num2라는 참조자를 만들었기 때문에 num2역시 3이 출력된다.
* num2는 num변수의 별칭이라고 생각하면 되기 때문에 num2값을 증가시켜주면 num값 역시 증가하는 것을 확인할 수 있을 것이다.
* 또한 하나의 변수에 여러 개의 레퍼런스를 선언할 수 도 있다.

### 참조자 vs 포인터
* 참조자에 대한 개념을 이해하고 나면 포인터와 유사해보이기 때문에 차이점이 궁금할 것이다.
* 공통점으로는 어떠한 대상을 가리킨다는 점이다.
* 하지만 레퍼런스와 포인터는 꽤 많은 차이점을 가지고 있다.

#### 1. NULL 허용 여부
* 포인터는 NULL을 허용하지만, 레퍼런스는 NULL이 될 수 없다.
* 포인터를 사용할 때 발생하는 Null pointer exception 등의 에러의 대부분 원인은 포인터를 초기화하지 않거나, NULL을 가리키고 있는 포인터에 접근했을 때 발생한다.
* 하지만 레퍼런스를 사용하면 이런 문제 발생하지 않는다. 레퍼런스는 NULL을 할당할 수 없도록 제한되어있기 때문.

#### 2. 참조 대상 할당 및 접근
* 포인터는 할당 할 때 참조 대상에 대해 &연산을 통해 주소값을 할당한다. 
* 반면 레퍼런스는 참조 대상을 그대로 할당한다.

```cpp
#include <iostream>
using namespace std;

int main() {
	int num = 3;
	int *point = &num;
	int &ref = num;
	cout << *point << ref;
	return 0;
}
```

* 넘겨 받은 참조를 사용할 때에도 포인터는 포인터 연산자를 통해 접근해야 하지만 레퍼런스는 지역변수처럼 접근할 수 있다. 레퍼런스의 값을 변경하면 레퍼런스가 참조하고 있는 실제 변수의 값 역시 변경된다.

#### 3. 참조 대상 변경 가능 여부
* 레퍼런스는 참조 대상을 변경할 수 없다. 반면 포인터는 참조 대상을 언제든지 변경할 수 있다.
* 포인터는 참조 대상을 변경할 뿐 아니라 cast를 통해 자기 자신과 전혀 다른 데이터 타입과도 쌍을 이룰 수 있다.
  