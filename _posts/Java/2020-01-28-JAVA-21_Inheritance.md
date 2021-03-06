---
title: "[Java] 상속(Inheritance)"
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
상속이란 부모 클래스가 가지고 있는 모든 것을(생성자 제외) 자식 클래스가 물려받아 같이 공유하며 나아가 확장(extends)하는 개념이다. 

부모 클래스를 상위 클래스라고 부르며, 자식 클래스를 하위 클래스라고 부른다.

확장(extends)에 대해서는 오버라이딩에 대해 작성하며 다시 설명을 해보려한다.

열혈자바에서는 __상속은 코드의 재활용을 목적으로 사용하는 문법이 아니다__ 라는 것을 강조하고 있다.

이유는 재활용을 목적으로 상속을 사용할 경우 무의미하게 코드가 복잡해지고, 기대와 달리 코드를 재활용하지 못하는 상황을 쉽게 경험할 것이라고 한다.

아직 자바를 통해서 제대로 된 프로젝트를 진행해보지 못했지만, 프로젝트를 하면서 이런 부분에 대한 나의 의견도 생기지 않을까 싶다.

## 상속의 기본
상속의 가장 기초는, 기존의 정의된 클래스에 메소드와 변수를 추가하여 새로운 클래스를 정의하는 것이라고 할 수 있다.

아래와 같은 클래스가 있다고 해보자.

```java
class Car{
    String number;
    public void CarName(){
        System.out.println("이것은 자동차이다");
    }
}
```

자동차 클래스를 상속하여, 아래와 같이 버스 클래스를 정의할 수 있다. 이때 extends 키워드를 사용하여 Car 클래스를 상속한다는 의미로 사용할 수 있다.

```java
class Bus extends Car{
    String busKind;
    public void BusKind(){
        CarName(); //Car클래스를 상속해서 호출할 수 있다.
        System.out.println("이 버스의 종류는"+busKind);
    }
}
```

Bus 인스턴스를 생성하면, Car 클래스를 상속했기 때문에, Car 클래스의 변수와 메소드 역시 존재하게 된다.

![](/assets/img/java/202001281.png)

## 상속에서의 생성자
만약, Bus 인스턴스를 생성하면, Car 클래스를 상속하게 되어, Car클래스의 변수 역시 Bus 클래스의 멤버변수가 된다.

그렇다면 이때는 Car 클래스의 변수는 어떻게 초기화 할 것인가?

결론부터 말하면,

__하위 클래스의 인스턴스 생성 시 상위 클래스, 하위 클래스의 생성자 모두 호출된다.__

__하위 클래스의 인스턴스 생성 시 상위 클래스의 생성자가 먼저 호출된다.__

하지만 아직까지 의문이 남는다. 상위 클래스의 생성자를 자동으로 호출한다고 하면, 인자는 어떻게 되는거지?

하위 클래스의 생성자에서 상위 클래스의 생성자를 명시적으로 호출하지 않으면, __인자를 받지 않는 생성자가 자동으로 호출된다.__

명시적으로 상위 클래스의 생성자를 호출하기 위한 키워드는 __super__ 이다.

```java
public Bus(String name){
    super(name); //상위 클래스의 생성자 호출을 의미함.
    System.out.println("이 버스의 종류는");
}
```

결국, 하위 클래스의 생성자에서 super를 사용하여 인자값을 넘겨 상위 클래스의 생성자를 호출할 수 있고, 호출을 생략하면, 인자값이 없는 상위 클래스의 생성자 호출문이 자동으로 들어간다는 말이 된다.

주의해야할 부분은 상위 클래스의 생성자는 하위 클래스 생성자의 가장 앞쪽에서 실행되어야 한다.

자바는 상속 관계에서, 상위 클래스의 멤버는 상위 클래스의 생성자를 통해서 초기화하도록 유도하고 있다.

또한 자바는 하나의 클래스가 상속할 수 있는 클래스의 수가 하나인 단일 상속만을 지원한다.

다만, 상속의 깊이를 더하는 것은 가능.

```java
//가능
class A{};
class B extends A{};
class C extends B{};
```

## 클래스 변수, 클래스 메소드 상속
인스턴스 생성과 상관이 없는 static선언이 붙은 클래스 변수와 클래스 메소드는 상속의 대상이 아니다.

그러면 상위 클래스에 있는 클래스 변수나 메소드에 하위 클래스에선 어떻게 접근을 할까?

예전에 포스팅했던 접근 수준 지시자의 protected가 여기서 등장한다!

즉, 접근 수준 지시자가 접근을 허용해야 접근이 가능하다.

예전 포스팅을 읽어보면, protected는 상속받은 다른 클래스에서 접근이 가능하다고 했었다.

즉, private으로 선언이 된 클래스 변수나 메소드는 접근이 불가능하다.

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__

[[Java] 상속(Inheritance)이란 ?](https://slowlywalk1993.tistory.com/entry/Java-%EC%83%81%EC%86%8DInheritance%EC%9D%B4%EB%9E%80)



