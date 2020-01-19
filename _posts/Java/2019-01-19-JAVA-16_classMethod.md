---
title: "[Java] static 선언을 붙여서 선언하는 클래스 메소드"
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
이전 포스팅에선 static 선언을 붙여서 선언하는 클래스 변수에 대해 알아보았다.

이번 포스팅에서는 이어서 클래스 메소드에 대해 알아보려 한다.

클래스 메소드는 클래스 변수와 거의 유사하다. 클래스 변수에 대해 이해했으면, 클래스 메소드 역시 무리없이 이해할 수 있을 것이다!

## 클래스 메소드의 정의와 호출
이전에 공부한 클래스 변수의 특징을 다시 한번 적어보면 아래와 같다.

-> 인스턴스 생성 이전부터 접근이 가능하다.

-> 어느 인스턴스에도 속하지 않는다.

이 두가지 특성은 클래스 메소드 역시 동일하게 갖는 특성이다.

즉, 메소드에 static 선언을 추가함으로 인해 인스턴스 메소드가 아닌 클래스 메소드를 선언할 수 있게 된다.

## 클래스 메소드에서 인스턴스 변수에 접근이 가능할까?
위의 질문에 대한 대답은 "No"이다.

조금만 생각해보자.

인스턴스 변수는 인스턴스에 속하고, 인스턴스가 생성되어야 메모리 공간에 존재하게 된다.

반면, 클래스 메소드는 인스턴스 생성 이전부터 호출이 가능하다. 

즉, 

__-> 클래스 메소드는 인스턴스에 속하지 않으므로, 인스턴스 변수에 접근이 불가능하다.__

__-> 클래스 메소드는 인스턴스 메소드의 호출도 불가능하다.__

## System.out.println()

static 선언의 의미를 배웠으므로, System.out.println에 대해 이해할 수 있을 것이다.

일단 System은 자바에서 제공하는 __클래스로 java.lang 패키지에__ 묶여 있다.

따라서 원칙적으로 호출할 때는 아래와 같이 호출해야한다.

__java.lang.System.out.println();__

그러나 컴파일러가 __import java.lang.*__ 선언을 삽입해주기 때문에, 패키지의 이름부분을 생략할 수 있다.

out은 System 클래스 내에 선언된 클래스 변수이다.

즉, System.out.println(); 은

__System에 위치한 클래스 변수 out이 참조하는 인스턴스의 println 메소드를 호출하는 문장이라고 할 수 있다.__

## main메소드가 public이고 static인 이유는?

main메소드의 호출이 이루어지는 영역은 클래스 외부이다. 따라서 public으로 선언해야 한다.

또한 main 메소드는 인스턴스가 생성되기 이전에 호출되므로, static으로 선언하는 것이 옳다!

## static 초기화 블록
아래의 코드를 봐보자.

```java
class DateOfExecution{
    static String date;

    public static void main(String[] args){
        System.out.println(date);
    }
}
```

위에서 __클래스 변수__ date를 선언과 동시에 오늘 날짜 정보를 담고 있는 문자열로 초기화하고 싶다.

만약, date가 인스턴스 변수라면, 생성자를 통해 초기화시키면 된다.

하지만, 클래스 변수는 생성자를 사용하기에 적절하지 않다.

이럴때 사용하는 것이 __static 초기화 블록__ 이다.

static 초기화 블록은 클래스 변수와 마찬가지로, 가상머신이 클래스의 정보를 읽어 들일 때(가상머신이 클래스를 로딩할 때) 실행이 된다.

즉 static 초기화 블록을 사용하면, 클래스 변수를 선언과 동시에 초기화 할 수 있다.

아래 코드를 보자.

```java
import java.time.LocalDate;

class DateOfExecution{
    static String date;
    static { //static 초기화 블록
        LocalDate nDate = LocalDate.now();
        date = nDate.toString();
    }
    public static void main(String[] args){
        System.out.println(date);
    }
}
```

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__