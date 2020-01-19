---
title: "[Java] 정보 은닉(Information Hiding)"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
toc: true
toc_sticky: true
---
자바에서 말하는 __정보__ 는 클래스의 __인스턴스 변수__ 를 의미한다.

정보를 은닉한다는 것은 인스턴스 변수를 숨긴다는 것.

## 정보를 은닉해야 하는 이유

간단한 예제를 보겠다.

```java
/* 정수를 입력하여 0보다 작은 경우 0을 반환하고,
   0 이상인 경우는 숫자 그대로 반환하는 프로그램 */

class Regist{
	int num = 0;
	public Regist(int n) {
		setNum(n);
	}
	public void setNum(int n) {
		if(n<0) {
			num = 0;
			return;
		}
		num = n; 
	}
	public int getNum() {
		return num;
	}
}

class ExampleIF {
	public static void main(String[] args) {
		Regist r = new Regist(3);
		System.out.println(r.getNum());
	}
}
```

즉, 반환되는 num 값은 항상 0 이상이어야 한다.

이러한 프로그램의 의도를 따르기 위해서라도, 입력하는 값을 변경하고 싶더라도 항상 위의 메소드 호출을 통해서 변경을 진행해야 한다.

만약 아래와 같은 문장으로 잘못된 접근을 하였다고 가정해보자.

```java
r.num = -5;
System.out.println(r.getNum());
```

출력한 num의 값은 -5가 되는 것을 알 수 있을 것이다.

이렇듯, __인스턴스 변수의 직접적인 접근을 허용하면, 컴파일 과정에서 드러나지 않는 중대한 실수가 발생할 수 있다__.

위와 같은 접근을 허용하지 않도록 클래스를 설계할 필요가 있고, 이러한 클래스의 설계를 가리켜 __정보 은닉__ 이라고 한다.

## 정보 은닉을 위한 private 선언

인스턴스 변수의 앞에 private를 선언을 추가한다.

그 후, 해당 인스턴스 변수에 접근할 수 있는 메소드를 별도로 제공하면 __정보 은닉__ 이 완료된다.

아래 예제를 보겠다.

```java
class Regist{
	private int num = 0; // 클래스 내부 접근만 허용
	public Regist(int n) {
		setNum(n);
	}
	public void setNum(int n) {
		if(n<0) {
			num = 0;
			return;
		}
		num = n; 
	}
	public int getNum() {
		return num;
	}
}

class ExampleIF {
	public static void main(String[] args) {
		Regist r = new Regist(3);
		System.out.println(r.getNum());
		// r.num = -5; -> 컴파일 에러 발생
	}
}
```

위에서 다뤘던 코드와 똑같은 코드지만, __클래스 Regist의 인스턴스 변수 num을 private로 처리__ 하였다.

이것이 의미하는 바는 아래와 같다.

__'변수 num은 클래스 내부에서만 접근을 허용하겠다.'__

즉, Regist 클래스 내에 정의된 메소드 내에서의 접근만 허용하겠다는 것.

따라서 아까와 같이 클래스 외부에서 r.num = -5 같은 방법으로 private로 선언된 멤버에 접근할 경우 컴파일 에러가 발생.

이렇게 인스턴스 변수를 선언함으로써, 다음 두가지 메소드의 타당성이 인정될 것.

* __setNum() : num에 값을 저장(혹은 수정)__

* __getNum() : num에 저장된 값을 반환__

즉, setNum()은 값의 설정을 위한 메소드이고, getNum()은 값의 참조를 위한 메소드이다.

## 설정과 참조를 위한 메소드

위의 예제에서 private로 선언된 인스턴스 변수 값의 설정과 참조를 위해 setNum()과 getNum() 메소드를 만들었다.

이러한 값의 설정과 참조를 위한 메소드를 가리켜 __게터와 세터__ 라고 부른다.

__게터(Getter)__
* -> 인스턴스 변수의 값을 참조하는 용도로 정의된 메소드
* -> 변수의 이름이 name일 때, 메소드 이름은 getName으로 짓는 것이 관례

__세터(Setter)__
* -> 인스턴스 변수의 값을 설정하는 용도로 정의된 메소드
* -> 변수의 이름이 name일 때, 메소드 이름은 setName으로 짓는 것이 관례

하지만, private으로 선언된 모든 인스턴스 변수를 대상으로 게터와 세터를 반드시 정의해야 하는 것은 아니다.

## 출처 
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__