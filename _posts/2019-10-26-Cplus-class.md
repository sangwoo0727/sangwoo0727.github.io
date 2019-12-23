---
title: "[C++] 클래스의 기본"
categories:
  - C++
tags:
  - C++
  - class
comments:
  - true
toc: true
toc_sticky: true
---
## [C++] 클래스의 기본

### 클래스와 구조체의 코드 상의 유일한 차이점
* 키워드 struct를 대신해서 class를 사용하면 구조체가 아닌 클래스가 된다.
* 다만 class로 키워드를 바꾸면 Car run99={"run99",100,0}; 과 같은 방식으로 변수를 선언하지 못한다.
* 이유는 클래스 내에 선언된 함수가 아닌, 다른 영역에서 변수를 초기화하려 했기 때문이다.
* 클래스는 기본적으로(별도의 선언을 하지 않으면) 클래스 내에 선언된 변수는 클래스 내에 선언된 함수에서만 접근 가능하다.
* Car run99; 와 같은 형태로 클래스 변수를 선언해야 한다.
* 클래스는 멤버의 접근과 관련해서 "접근과 관련해서 별도의 선언을 하지 않으면, 클래스 내에 선언된 변수 및 함수에 대한 접근은 허용하지 않을테니 접근과 관련된 지시를 별도로 내려줘"라고 이야기한다.
* 클래스는 정의를 하는 과정에서 각각의 변수 및 함수의 접근 허용범위를 별도로 선언해야 한다. 이것이 구조체와 클래스의 큰 차이점이다.

### 접근제어 지시자(접근제어 레이블)
* C++의 접근제어 지시자는 총 3가지가 존재한다.
* public : 어디서든 접근 허용
* protected : 상속관계에 놓여있을 때, 유도 클래스에서의 접근 허용
* private : 클래스 내에서만 접근 허용
* 아래의 예제를 통해 먼저 public과 private에 대해 알아보겠다.

```cpp
#include <iostream>
#include <cstring>
using namespace std;

namespace CAR_CONST {
	enum {
		ID_LEN = 20, MAX_SPD = 200, FUEL_STEP = 2, ACC_STEP = 10, BRK_STEP = 10
	};
}

class Car {
private: //private가 선언되었으므로, 이어서 등장하는 변수와 함수는 private에 해당하는 범위 내에서(클래스 내에서만)접근가능.
	char gamerID[CAR_CONST::ID_LEN];
	int fuelGauge;
	int curSpeed;
public: //public 선언되었으므로, 이어서 등장하는 변수와 함수는 어디서든 접근이 가능하다.
	void InitMembers(const char * ID, int fuel);
	void ShowCarState();
	void Accel();
	void Break();
};

void Car::InitMembers(const char * ID, int fuel) {
    /* 클래스 안에 선언된 변수의 초기화를 목적으로 정의된 함수이다. 변수가 모두 private로 선언되어서 main함수에서 접근 불가능.
       하지만 이 함수는 동일 클래스 내에 정의된 함수이므로 접근 가능. 뿐만 아니라, 이 함수는 public으로 선언되어서 main함수에서
       호출 가능하다. main함수에서는 이 함수의 호출을 통해서 클래스 안에 선언된 변수를 초기화 할 수 있다.*/
	strcpy(gamerID, ID);
	fuelGauge = fuel;
	curSpeed = 0;
}
void Car::ShowCarState() {
	cout << "쇼유자ID: " << gamerID << endl;
	cout << "연료량: " << fuelGauge << "%" << endl;
	cout << "현재속도: " << curSpeed << "km/s" << endl << endl;
}
void Car::Accel() {
	if (fuelGauge <= 0) return;
	else fuelGauge -= CAR_CONST::FUEL_STEP;
	if ((curSpeed + CAR_CONST::ACC_STEP) >= CAR_CONST::MAX_SPD) {
		curSpeed = CAR_CONST::MAX_SPD;
		return;
	}
	curSpeed += CAR_CONST::ACC_STEP;
}
void Car::Break() {
	if (curSpeed < CAR_CONST::BRK_STEP) {
		curSpeed = 0;
		return;
	}
	curSpeed -= CAR_CONST::BRK_STEP;
}
int main() {
	Car run99;
	run99.InitMembers("run99", 100);
    /* 위의 문장은 초기화를 목적으로 InitMembers 함수를 호출하고 있다. 이 함수는 ID정보와 연료의 게이지 정보를 전달받아서
       초기화되는 형태로 정의되어 있다 */
	run99.Accel();
	run99.Accel();
	run99.Accel();
	run99.ShowCarState();
	run99.Break();
	run99.ShowCarState();
    /* 위의 문장들은 함수 호출이 모두 가능한 이유는 모두 public으로 선언되었기 때문이다.*/
	return 0;
}
```

* 접근 제어 지시자 A가 선언되면, 그 이후에 등장하는 변수나 함수는 A에 해당하는 범위 내에서 접근이 가능하다.
* 새로운 접근 제어 지시자 B가 선언되면, 그 이후로 등장하는 변수나 함수는 B에 해당하는 범위 내에서 접근이 가능하다.
* 함수의 정의를 클래스 밖으로 빼도, 이는 클래스의 일부이기 때문에, 함수 내에서는 private으로 선언된 변수에 접근 가능하다.
* 키워드 struct를 이용해서 정의한 구조체(클래스)에 선언된 변수와 함수에 별도의 접근제어 지시자를 선언하지 않으면, 모든 변수와 함수는 public으로 선언된다.
* 키워드 class를 이용해서 정의한 클래스에 선언된 변수와 함수에 별도의 접근제어 지시자를 선언하지 않으면, 모든 변수와 함수는 private로 선언된다.

#### 출처 : 윤성우의 열혈 C++ 프로그래밍 , 오렌지 미디어