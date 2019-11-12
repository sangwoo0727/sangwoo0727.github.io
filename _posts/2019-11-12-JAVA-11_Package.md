---
title: "[Java] 패키지(Package)"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
---

* 패키지는 간단하게 __클래스를 묶는 수단__ 이다.

### 패키지 선언의 의미와 목적
* Java SE에서 제공하는 클래스의 수는 수천이다.
* 단순히 클래스가 이름만 갖게되면 __어떠한 용도로 사용되는 클래스__ 인지 구분이 어렵다.
* 예를 들어, Class CookieManager 라는 클래스가 있을때, 이름만 놓고 보면 어떤 용도인지 모를 수 있다.
* 위의 클래스가 속한 패키지 이름은 java.net이다.
* 이 경우, 패키지 이름이 java로 시작하기 때문에 자바에서 제공하는 클래스임을 뜻한다.
* net은 network의 줄임말로 네트워크 관련 기능의 클래스임을 알 수 있게 해준다.
* 결국 패키지는 __클래스를 구분하고 파악하는데 도움을 준다.__
* 또한 패키지는 __클래스의 이름이 겹치는 문제도 해결할 수 있다.__
* 둘 이상의 집단이 제공하는 클래스를 사용하면 클래스의 이름이 충돌하는 문제가 발생할 수 있다.
* 이런 경우, 집단의 고유 정보 등을 이용해서 패키지의 이름을 지으면, 이름 충돌의 문제를 해결할 수 있다.

### 패키지 이름 짓는 관례
1. 클래스의 이름과 구분이 되도록 패키지의 이름은 모두 소문자로 구성.
2. 인터넷 도메인 이름의 역순으로 패키지 이름을 구성.
3. 패키지 이름의 끝에 클래스를 정의한 주체 또는 팀을 구분하는 이름으로 추가.

### 이름 충돌의 해결을 위한 패키지의 효과
* 패키지의 선언은 __클래스의 접근 방법을 구분__ 할 뿐만 아니라, __클래스 파일이 공간적으로도 구분__ 되게 한다.
* 즉, 다음과 같은 두 가지 특성을 만들어낸다.
* __서로 다른 패키지의 두 클래스는 인스턴스 생성 시 사용하는 이름이 다르다__
* __서로 다른 패키지의 두 클래스 파일은 저장되는 위치가 다르다__
* 인스턴스 생성 시 사용되는 이름을 알아보자.
* 도메인이 abcd.com인 회사의 aaaa 팀에서 개발한 클래스를 묶을 패키지 이름은?
 * com.abcd.aaaa
* 패키지 안에 있는 Circle 클래스를 통해 인스턴스 생성하는 문장은?
 * com.abcd.aaaa.Circle c1 = new com.abcd.aaaa.Circle(3);
* 도메인이 cdef.com인 회사의 bbbb 팀에서 개발한 클래스를 묶을 패키지 이름은?
 * com.cdef.bbbb
* 패키지 안에 있는 Circle 클래스를 통해 인스턴스 생성하는 문장은?
 * com.cdef.bbbb.Circle c2 = new com.cdef.bbbb.Circle(4);

* 이렇게 인스턴스 생성 및 참조 변수 선언 시 클래스의 이름 앞에 패키지 이름이 따라붙는 구조가 된다.
* 클래스 파일이 저장되는 위치도 아래와 같이 변하게 된다.
* 패키지 com.abcd.aaaa의 Circle.class 저장 위치 : ...\com\abcd\aaaa
* 패키지 com.cdef.bbbb의 Circle.class 저장 위치 : ...\com\cdef\bbbb

### 패키지의 선언 및 컴파일 방법
* 클래스를 패키지로 묶을 때에는 __해당 클래스를 담고 있는 소스파일의 상단에 패키지 선언을 해야 한다.__
* 예제를 통해 구조를 익혀보자.

```java
/* Circle.class 클래스 파일은 패키지로 묶여있기 때문에,
   .\com\abcd\aaaa 디렉토리에 존재하게 된다. */
package com.abcd.aaaa;

public class Circle {
	double rad;
	final double PI;
	public Circle(double r) {
		rad = r;
		PI = 3.14;
	}
	public double getPerimeter() {
		return (rad * 2) * PI;
	}
}
```

```java
/* 패키지로 묶인 클래스 파일에 접근을 하기 위하여 
   인스턴스 변수 생성 문장을 위에서 배운대로 정의하였다. */
public class CircleUsing {
	public static void main(String[] args) {
			com.abcd.aaaa.Circle c1 = new com.abcd.aaaa.Circle(3);
			System.out.println(c1.getPerimeter());
		}
}
```

### import 선언
* 동일한 이름의 두 클래스를 대상으로 인스턴스를 생성해야 하는 상황이라면 패키지의 이름 생략은 불가능.
* 다만, 필요로 하는 클래스가 하나일 경우 import를 통해 패키지 이름을 생략할 수 있다.

```java
import com.abcd.aaaa.Circle; //import 선언

public class CircleUsing {
	public static void main(String[] args) {
			Circle c1 = new Circle(3);
			System.out.println(c1.getPerimeter());
		}
}
```

* import 문장은 아래와 같이 생각하면 될 것이다.
* 지금부터 Circle이라 하면 com.abcd.aaaa.Circle을 의미하는 것으로 간주해라!
* __클래스가 아닌 패키지를 대상으로도 import 선언이 가능하다__.
* __import com.abcd.aaaa.*__
* 위의 문장은 __지금부터 com.abcd.aaaa 패키지의 묶인 클래스는 패키지 선언을 생략하겠다__ 라고 생각.
* improt 선언은 __이름 충돌이 발생할 수도 있고__, __의도하지 않은 클래스의 인스턴스를 생성하는 상황으로 이어질 수 있다__.
* 주의해서 사용하자!

#### 출처 
* 윤성우의 열혈 Java 프로그래밍, 오렌지 미디어