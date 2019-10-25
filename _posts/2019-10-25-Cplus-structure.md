---
title: "[C++] 클래스의 기초 : C++에서의 구조체"
categories:
  - C++
tags:
  - C++
  - class
comments:
  - true
---
## [C++] 클래스의 기초 : C++에서의 구조체
* 구조체를 사용하면, 연관 있는 데이터를 하나로 묶으면 프로그램의 구현 및 관리가 용이하다.
* 구조체는 연관 있는 데이터를 묶을 수 있는 문법적 장치로 데이터의 표현에 큰 도움을 준다.

### C++ 에서의 구조체 변수 선언
* C언어에서의 구조체 변수를 선언하는 방법 ex) struct Car basicCar;
* struct 키워드는 이어서 선언되는 자료형이 구조체를 기반으로 정의된 자료형임을 나타낸다. struct를 생략하려면 구조체를 만들때 typedef를 이용해야 한다.
* C++에서는 기본 자료형 변수의 선언방식이나 구조체 기반으로 정의된 자료형의 변수 선언방식에 차이가 없다. 즉, typedef 없이도 선언 가능 ex) Car basicCar;
* 앞서 정의한 구조체를 기반으로 예제를 작성하면 아래와 같다.
  
```cpp
#include <iostream>
using namespace std;

#define ID_LEN 20
#define MAX_SPD 200
#define FUEL_STEP 2
#define ACC_STEP 10
#define BRK_STEP 10

struct Car {
	char gamerID[ID_LEN]; // 소유자 ID
	int fuelGauge; // 연료량
	int curSpeed; //현재 속도
};

void ShowCarState(const Car &car) { //현재 차의 상태를 출력하는 함수
	cout << "소유자ID: " << car.gamerID << endl;
	cout << "연료량: " << car.fuelGauge << "%" << endl;
	cout << "현재속도:" << car.curSpeed << "km/s" << endl << endl;
}

void Accel(Car &car) {
	if (car.fuelGauge <= 0) return;
	else car.fuelGauge -= FUEL_STEP;

	if (car.curSpeed + ACC_STEP >= MAX_SPD) {
		car.curSpeed = MAX_SPD;
		return;
	}
	car.curSpeed += ACC_STEP;
}

void Break(Car &car) {
	if (car.curSpeed < BRK_STEP) {
		car.curSpeed = 0;
		return;
	}
	car.curSpeed -= BRK_STEP;
}

int main() {
	Car run99 = { "run99",100,0 };
	Accel(run99);
	Accel(run99);
	ShowCarState(run99);
	Break(run99);
	ShowCarState(run99);
	
	Car sped77 = { "sped77",100,0 };
	Accel(sped77);
	Break(sped77);
	ShowCarState(sped77);

	return 0;
}
```
* 위의 예제에서 작성한 함수들은 구조체 Car에 종속적인 함수들이라고 할 수 있다.(왜냐하면 구조체 Car와 함께 부류를 형성하여, Car와 관련된 데이터의 처리를 담당하는 함수들이기 때문)
* 하지만 위의 함수들은 전역함수의 형태를 띠기 때문에, 이 함수들이 구조체 Car에 종속적임을 나타내지 못하고 있는 상황. 다른 영역에서 이 함수를 호출하는 실수를 범할 수도 있는 상황이다.

### 구조체 안에 함수 삽입하기
* 위의 예제의 문제인 함수들이 의미는 구조체 Car에 종속적이지만, 구조체 Car에 종속적임을 나타내지 못하고 있는 상황을 해결하기 위하여 C++에서는 구조체 안에 함수를 삽입하는 것을 허용한다.
* 위의 예제를 조금 수정해보았다.

```cpp
#include <iostream>
using namespace std;

#define ID_LEN 20
#define MAX_SPD 200
#define FUEL_STEP 2
#define ACC_STEP 10
#define BRK_STEP 10

struct Car {
	char gamerID[ID_LEN]; // 소유자 ID
	int fuelGauge; // 연료량
	int curSpeed; //현재 속도


	void ShowCarState() { //현재 차의 상태를 출력하는 함수
		cout << "소유자ID: " << gamerID << endl;
		cout << "연료량: " << fuelGauge << "%" << endl;
		cout << "현재속도:" << curSpeed << "km/s" << endl << endl;
	}

	void Accel() {
		if (fuelGauge <= 0) return;
		else fuelGauge -= FUEL_STEP;

		if (curSpeed + ACC_STEP >= MAX_SPD) {
			curSpeed = MAX_SPD;
			return;
		}
		curSpeed += ACC_STEP;
	}

	void Break() {
		if (curSpeed < BRK_STEP) {
			curSpeed = 0;
			return;
		}
		curSpeed -= BRK_STEP;
	}
};



int main() {
	Car run99 = { "run99",100,0 };
	run99.Accel();
	run99.ShowCarState();
	run99.Break();
	run99.ShowCarState();
	
	Car sped77 = { "sped77",100,0 };
	sped77.Accel();
	sped77.ShowCarState();
	sped77.Break();
	sped77.ShowCarState();

	return 0;
}
```

### 구조체 안에 enum 상수의 선언
* 위의 예제들을 보면 매크로 상수들이 존재한다.
* 하지만 이들 상수 역시 구조체 Car에게만 의미가 있는 상수들이다. 즉 다른 영역에서 사용하도록 정의된 상수가 아니다.
* 어떤 경우에는 이들 상수 역시 구조체 내에 포함시키는 것이 좋을 수 있다.
* 이러한 경우 열거형 enum을 이용해서 구조체 내에서만 유효한 상수를 정의하면 된다.

```cpp
#include <iostream>
using namespace std;

struct Car {
	enum {
		ID_LEN = 20, MAX_SPD = 200, FUEL_STEP = 2, ACC_STEP = 10, BRK_STEP = 10
	};

	char gamerID[ID_LEN]; // 소유자 ID
	int fuelGauge; // 연료량
	int curSpeed; //현재 속도


	void ShowCarState() { //현재 차의 상태를 출력하는 함수
		cout << "소유자ID: " << gamerID << endl;
		cout << "연료량: " << fuelGauge << "%" << endl;
		cout << "현재속도:" << curSpeed << "km/s" << endl << endl;
	}

	void Accel() {
		if (fuelGauge <= 0) return;
		else fuelGauge -= FUEL_STEP;

		if (curSpeed + ACC_STEP >= MAX_SPD) {
			curSpeed = MAX_SPD;
			return;
		}
		curSpeed += ACC_STEP;
	}

	void Break() {
		if (curSpeed < BRK_STEP) {
			curSpeed = 0;
			return;
		}
		curSpeed -= BRK_STEP;
	}
};



int main() {
	Car run99 = { "run99",100,0 };
	run99.Accel();
	run99.ShowCarState();
	run99.Break();
	run99.ShowCarState();
	
	Car sped77 = { "sped77",100,0 };
	sped77.Accel();
	sped77.ShowCarState();
	sped77.Break();
	sped77.ShowCarState();

	return 0;
}
```

### 함수 외부로 빼기
* 보통 프로그램을 분석할 때, 흐름 및 골격 위주로 분석하는 경우가 많다. 이러한 경우에는 함수의 기능만 파악하고 세부 구현까지는 신경쓰지 않는 경우가 많다.
* 구조체를 볼 때, 정의되어 있는 함수의 종류와 기능이 한눈에 들어오게끔, 구조체 내에 정의된 함수의 수가 많거나 그 길이가 길다면 구조체 밖으로 함수를 빼낼 필요가 있다.
* 즉, 함수의 원형선언은 구조체 안에 두고, 함수의 정의를 밖으로 빼내는 것.
* 다만 함수를 밖으로 뺀 경우, 해당 함수가 어디에 정의되어 있는지에 대한 정보를 추가해야 한다.

```cpp
#include <iostream>
using namespace std;

struct Car {
	enum {
		ID_LEN = 20, MAX_SPD = 200, FUEL_STEP = 2, ACC_STEP = 10, BRK_STEP = 10
	};
	char gamerID[ID_LEN]; // 소유자 ID
	int fuelGauge; // 연료량
	int curSpeed; //현재 속도

	void ShowCarState();
	void Accel();
	void Break();
};

void Car::ShowCarState() {
	cout << "소유자ID: " << gamerID << endl;
	cout << "연료량: " << fuelGauge << "%" << endl;
	cout << "현재속도:" << curSpeed << "km/s" << endl << endl;
}

void Car::Accel() {
	if (fuelGauge <= 0) return;
	else fuelGauge -= FUEL_STEP;

	if (curSpeed + ACC_STEP >= MAX_SPD) {
		curSpeed = MAX_SPD;
		return;
	}
	curSpeed += ACC_STEP;
}

void Car::Break() {
	if (curSpeed < BRK_STEP) {
		curSpeed = 0;
		return;
	}
	curSpeed -= BRK_STEP;
}

	
int main() {
	Car run99 = { "run99",100,0 };
	run99.Accel();
	run99.ShowCarState();
	run99.Break();
	run99.ShowCarState();
	
	Car sped77 = { "sped77",100,0 };
	sped77.Accel();
	sped77.ShowCarState();
	sped77.Break();
	sped77.ShowCarState();

	return 0;
}
```

* 사실 구조체 안에 함수가 정의되어 있으면, 함수를 인라인으로 처리해라라는 의미가 내포되어 있다.
* 하지만 위의 경우처럼 함수를 구조체 밖으로 빼면 이러한 의미가 사라진다. 따라서 인라인의 의미를 유지하려면 함수 앞에 inline 키워드를 붙혀 인라인 처리를 명시적으로 지시해야 한다.

### 마무리
* 지금까지 c++에서의 구조체를 포스팅하였다. 구조체는 클래스의 일종으로, 다음 포스팅부터는 클래스에 대한 설명을 해나가겠다.

#### 출처 : 윤성우의 열혈 C++ 프로그래밍 , 오렌지 미디어
