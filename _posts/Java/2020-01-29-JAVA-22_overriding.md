---
title: "[Java] 메소드 오버라이딩(Overriding)"
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
메소드 오버라이딩이란 상위 클래스에 정의된 메소드를 하위 클래스에서 다시 재정의 하는 것을 뜻한다.

자바를 공부하기 전에 항상 메소드 오버라이딩이 어려웠는데 이번에 책을 통해 공부하면서 흐름에 맞게 이해를 하니 좀 좋았다.

결론은,, 열혈자바사서 꼭 읽으세요..

메소드 오버라이딩을 시작하기 전에 먼저 상위 클래스의 참조 변수가 참조할 수 있는 대상의 범위에 대해 이해를 해야한다.

상위 클래스가 Person이고 Person 클래스를 상속하는 Student 클래스가 있다고 해보자.

이때,

Person p = new Person(...); 이나

Student s = new Student(...); 문장으로 인스턴스를 생성하는 것은 이미 알고 있는 부분이다.

하지만 상속관계에 있어서 상위 클래스의 참조 변수가 하위 클래스의 인스턴스를 참조할 수 있다.

__Person p = new Student(...);__

다만, 이때 p를 통해 Person에 정의된 메소드 호출은 가능하지만, Student 클래스에 정의된 메소드 호출은 불가능하다.

참조변수 p는 Person 형 참조 변수이다. 이러한 경우, p를 통해 접근이 가능한 멤버는 Person 클래스에 정의되어 있거나, 이 클래스가 상속하는 클래스의 멤버로 제한된다.

다시 말해서, __참조 변수가 참조하는 인스턴스의 종류에 상관 없이, 참조 변수의 형에 해당하는 클래스와 그 클래스가 상속하는 상위 클래스에 정의된 메소드들만 호출이 가능하다는 것.__

## 메소드 오버라이딩(Method Overriding)
상위 클래스에 정의된 메소드를 하위 클래스에서 다시 정의하는 행위를 가리켜 메소드 오버라이딩이라고 한다.

이때, 오버라이딩은 __무효화 시키다__ 의 뜻으로 해석된다.

Student 클래스가 Person클래스를 상속하면서, Person에 정의된 메소드를 오버라이딩시키기 위해서는 Student 클래스의 메소드와 Person의 메소드는 __메소드 이름, 메소드의 반환형, 메소드의 매개변수 선언__ 이 세가지가 같아야 메소드 오버라이딩이 성립한다.

```java
class Person{
  public void show(){
    System.out.println("I am Person");
  }
}

class Student extends Person{
  public void show(){
    System.out.println("I am Student");
  }
}

Person p = new Student();
p.show(); //이 경우, 앞에 배운 바에 의하면 p는 Person의 참조변수이므로, Student 인스턴스를 참조하고 있는 상황이더라도 Person의 show 메소드가 호출되어야 한다. 하지만 Person의 show 메소드는 오버라이딩 되었다. 이 경우 Student의 show 메소드가 대신 호출된다.
```

그렇다면 오버라이딩된 메소드 호출은 불가능한걸까? 아직까지 공부를 많이 안해서 그런지 오버라이딩을 왜 쓰는건지 도통 헷갈리기만 하고 이해가 가질 않긴한다..

아무튼 오버라이딩된 메소드를 호출하는 방법은 간단했다.

이전에 자바를 공부하면서 우리는 super 키워드를 통해 상위 클래스의 생성자를 호출하였다. 이번에도 역시 super다.

```java
class Student extends Person{
  public void show(){
    super.show(); //오버라이딩된 Person의 show 메소드 호출.
    System.out.println("I am Student");
  }
}
```

메소드 오버라이딩이 아닌 인스턴스 변수나 클래스 변수의 오버라이딩도 가능할까?

먼저 책에서는 __상위 클래스에 선언된 변수와 동일한 이름의 변수를 하위 클래스에서 선언하는 일은 피해야 한다__ 고 하고있다.

즉, 이런 코드 자체를 만들지 말라는 것이다.

문법적인 결론으로 말하면, 변수는 오버라이딩되지 않고, 참조 변수의 형에 따라서 접근하는 변수가 결정된다.

```java
Student s = new Student();
s.name = //.. Student의 name에 접근

Person p = new Student();
p.name = //.. Person의 name에 접근
```

## instanceof 연산자
instanceof의 연산자는 다양하게 활용할 수 있지만, 이 포스팅에선 어떤 역할을 하는 연산자인지만 확인하고 넘어가겠다.

자세한 내용은 책이나 실습을 통해 확인하길 바란다.

연산자 instanceof는 참조변수가 참조하는 인스턴스의 __클래스__ 나 참조하는 인스턴스가 __상속하는 클래스__ 를 묻는 연산자이다.

if(s instanceof Student) //s가 참조하는 인스턴스가 Student의 인스턴스이거나 Student가 상속하는 클래스의 인스턴스냐
if(s instanceof Person)

## 클래스와 메소드의 final 선언
어떤 클래스를 다른 클래스가 상속하는 것을 원치 않는다면, class 선언에 있어서 final 선언을 붙히면 된다.

```java
public final class Person{ //다른 클래스는 Person 클래스를 상속할 수 없음.
  //...
}
```



## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__

__[코딩의 시작 TCP school](http://tcpschool.com/java/java_inheritance_overriding)__