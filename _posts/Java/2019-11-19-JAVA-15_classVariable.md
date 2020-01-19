---
title: "[Java] static 선언을 붙여서 선언하는 클래스 변수"
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
## 들어가기 전
main 메소드에 static 선언이 붙어있는 이유에 대한 궁금증은 이 포스팅과 다음포스팅에서 풀 수 있다.

바로 들어가보자!

## 클래스 변수(static 변수)

인스턴스 변수는 인스턴스가 생성되었을 때, 생성된 인스턴스 안에 존재하는 변수.

반면, __클래스 변수__ 는 인스턴스의 생성과 상관없이 존재하는 변수이다.

클래스 내에 선언된 변수 앞에 static 선언을 붙이면 인스턴스 변수가 아닌, __클래스 변수__ 가 된다.

즉! static으로 선언된 변수는 __변수가 선언된 클래스의 모든 인스턴스가 공유하는 변수이다!!__.

인스턴스 내에 존재하는 변수가 아니라 __어떠한 인스턴스에도 속하지 않은 상태로 메모리 공간에 딱 하나만 존재하는 변수__.

이 변수가 선언된 클래스의 인스턴스들은 이 변수에 바로 접근할 수 있는 권한이 있다.
  
![](/assets/img/java/classVariable_201911191.png)


클래스 변수도 __접근 수준 지시자__ 의 규칙을 적용받기 때문에 public으로 선언되면 어디서든 접근이 가능하다.

## 클래스 변수의 접근 방법

__클래스 내부 접근__ : 변수의 이름을 통해 직접 접근.

__클래스 외부 접근__ : 클래스 또는 인스턴스의 이름을 통해 접근.

아래 예제를 통해 클래스 변수의 접근 방법을 알아보겠다.

```java
class AccessWay{
	static int num = 0;
	AccessWay(){ //생성자
		incrCnt();
	}
	void incrCnt() {
		num++; //클래스 내부에서 이름을 통한 접근
	}
}
class ClassVarAccess {
	public static void main(String[] args) {
		AccessWay way = new AccessWay();
		way.num++; //외부에서 인스턴스의 이름을 통한 접근
		AccessWay.num++; //외부에서 클래스의 이름을 통한 접근
		System.out.println("num = " + AccessWay.num); // num = 3
	}
}
```


인스턴스의 이름을 통한 접근 방법에서, 클래스 변수를 인스턴스 내부에 위치한 것으로 오해하면 안된다.

또한, 클래스 변수 num은 default로 선언되었기 때문에 , __클래스 내부는 물론 클래스 외부여도, 동일 패키지로 묶여 있으면 접근 가능.__

## 클래스 변수의 접근 방법

클래스 변수는 언제 메모리 공간에 할당되고 초기화 될까?

아래 예제를 봐보자

```java
class InstCnt{
	static int instNum = 100;
	InstCnt(){
		instNum++;
		System.out.println("인스턴스 생성: "+ instNum);
	}
}
class OnlyClassNoInstance {
	public static void main(String[] args) {
		InstCnt.instNum -= 15; //인스턴스 생성 없이 instNum에 접근
		System.out.println(InstCnt.instNum);
	}
}
```

인스턴스 생성을 하지 않았음에도 instNum이 메모리 공간에 할당되고 초기화 되었음을 알 수 있다.

즉, __클래스 변수는 인스턴스 생성 이전에 메모리 공간에 존재한다.__

클래스 변수는 해당 클래스 정보가 가상머신에 의해 읽히는 순간 메모리 공간에 할당되고 초기화 된다.(인스턴스의 생성과 무관하게 이루어짐)

따라서, 생성자를 통해 클래스 변수의 초기화를 진행하지 않도록 주의해야 한다.

```java
class InstCnt {
  static int instNum = 100; // 클래스 변수의 정상적인 초기화 방법!
  InstCnt(){
    instNum = 0; //클래스 변수의 초기화라고 착각하면 코드에 큰 혼란이 온당...
    //즉, 이렇게 써두면, 인스턴스를 새로 생성할 때마다 instNum이 0으로 바뀜!!
  }
}
```

## 클래스 로딩

위에서 표현한 __클래스 정보를 가상 머신이 읽는다__ 를 __클래스 로딩__ 이라고 한다.

클래스 로딩은 __가상 머신이 특정 클래스 정보를 읽는 행위를 가리킴__

특정 클래스의 인스턴스 생성을 위해서는 __해당 클래스가 반드시 가상머신에 의해 로딩되어야 한다__

즉, __인스턴스 생성보다 클래스 로딩이 먼저이다.__


## 클래스 변수를 언제 활용될까?

위에서 작성한 글을 토대로 클래스 변수가 유용하게 활용될 상황을 한가지 짐작할 수 있다.

바로, __인스턴스 간에 데이터 공유가 필요한 상황__.

추가적으로, 저자는 한가지 사례를 더 말해주었다.

예제를 봐보자

```java
class Circle{
  static final double PI = 3.1415; //변하지 않는, 참조가 목적인 값
  private double radius;
  ...
  ...
}

class CircleConstPI{
  public static void main(String[] args){
    Circle c = new Circle(1.2);
    ..
    ..
  }
}
```

위의 코드에서 PI를 상수로 선언하였는데, 가만보면 인스턴스 변수가 아닌 __클래스 변수__ 로 선언하였다.

__클래스 변수__ 로 선언하여야 하는 이유에 대해 생각해보자.

__PI는 모든 인스턴스가 참조해야하는 값이지만, 인스턴스 각각이 지녀야하는 값은 아니기 때문!!__

아하! 이런 경우에 딱 들어맞는 변수 선언법이 __클래스 변수__ 겠다!

즉, 참조를 목적으로만 존재하는 값은 __final 선언이 된 클래스 변수에 담는다.__

다음 포스팅에선 static 선언을 붙여서 선언하는 클래스 메소드에 대해 다뤄보겠다.

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__





