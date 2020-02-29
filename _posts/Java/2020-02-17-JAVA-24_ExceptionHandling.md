---
title: "[Java] 예외처리(Exception Handling)"
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
일반적인 오류란 문법적 실수를 뜻하는 경우가 많고, 대부분의 오류는 컴파일 과정에서 드러난다.

하지만 예외는 프로그램이 실행 중에 발생하는 정상적이지 않은 상황을 뜻한다.

두 수 a,b 를 입력받아 a/b를 출력하는 프로그램을 만들었다고 생각해보자.

이 경우, 코드에서 문법적인 오류를 발견할 수는 없지만 b에 0을 입력하는 사용자가 있다면 프로그램이 작동할 수 없다.

이러한 상황을 예외라고 한다.

이러한 경우 출력된 문장을 살펴보자.

"Exception in thread "main" java.lang.ArithmeticException: / by zero at...."

이러한 출력문은 가상머신이 예외를 처리하는 방식. 가상머신은 에외가 발생하면 그 내용을 출력하고 프로그램을 종료한다.

## 예외 처리를 위한 try ~ catch
위에서의 출력문에 java.lang.ArithmeticException 이러한 클래스를 확인 할 수 있었다.

이렇듯 자바는 예외 상황별로 상황을 알리기 위한 클래스를 정의한다.

위와 같은 프로그램에서 수학 연산 관련 오류가 발생하면 가상머신은 예외 클래스 ArithmeticException의 인스턴스를 생성한다.

이 인스턴스를 프로그래머가 처리하기 위한 방법이 try ~ catch 문이다.

try ~ catch문은 try영역에서 발생한 예외 상황을 catch영역에서 처리한다는 것으로, 이렇게 코드를짜면, 프로그래머가 예외를 처리한 것으로 간주하고 가상머신이 프로그램을 종료하지 않는다.

try catch문의 기본 골격을 알아보자.

```java
try{
  //관찰 영역
}catch(ArithmeticException e){
  //처리 영역
}
```

예외가 발생하면, 가상머신이 ArithmeticException 인스턴스를 생성하고, 이 인스턴스는 catch구문의 매개변수 e에 전달이 된다.

위에서도 언급하였지만, catch 구문을 통해 예외 인스턴스가 전달이 되면, 가상 머신은 예외가 처리된 것으로 판단하고, 프로그램을 종료시키지 않고 try catch문 다음 문장을 이어서 실행해나간다.

한가지 알아둬야하는 건, try안에 구문이 실행되다가, 중간에서 예외가 발생하여 catch 영역에서 이 예외가 처리되면, 중간부터 다시 이어나가는 것이 아니라 try catch문 전체를 건너뛰고 실행을 이어나가게 된다.



## 둘 이상의 예외를 처리하기 위한 방법
자바 7 이전에는 catch구문을 여러개 작성하여 예외를 처리하였었다.

```java
try{
  //....
}catch(ArithmeticException e){
  e.getMessage();
}catch(InputMismatchException e){
  e.getMessage();
}
//...
```

이렇듯, catch 구문은 이어서 작성할 수 있다. 하지만 자바7 이후부터는 하나의 catch 구문 안에서 둘 이상의 예외를 처리하는 것이 가능해졌다.

```java
try{
  //....
}catch(ArithmeticException| InputMismatchException e){
  e.getMessage();
}
//...
```

## Throwable 클래스
예외 클래스의 최상위 클래스는 java.lang.Throwable 이다.

이 클래스의 대표적인 메소드는 getMessage()와 printStackTree()가 있다.

아래 코드를 통해 Throwable을 이해해보자.

```java
class Example{
  public static void f1(int n){
    f2(n,0);
  }

  public static void f2(int n1, int n2){
    int r = n1/n2;
  }
  public static void main(String[] args){
    f1(3);
    System.out.println("bye");
  }
}
```

일단 이 코드의 메소드 호출 흐름은 main -> f1 -> f2이다.

예외는 f2에서 발생하였지만, f1에게 책임을 넘기고, f1역시 예외처리를 하지 않았으므로 main에게 책임을 넘긴다.

끝은 main이고 main역시 예외처리를 하지 않았으므로 가상머신이 대신 예외를 처리하고 프로그램을 종료하므로, bye는 출력되지 않는다.

아래는 똑같은 코드를 예외처리한 상황이다.

```java
class Example{
  public static void f1(int n){
    f2(n,0);
  }

  public static void f2(int n1, int n2){
    int r = n1/n2;
  }
  public static void main(String[] args){
    try{
      f1(3);
    }
    catch(Throwable e){
      e.printStackTrace();
    }
    System.out.println("bye");
  }
}
```

실제로 넘어오는 예외는 Throwable은 아니지만, 모든 예외 클래스는 Throwable을 상속하므로 상속관계에 의해 이런식으로 처리할 수 있다(다형성)

이 경우 main에서 예외를 처리했으므로 자연스레 bye가 출력되는 것을 알 수 있다.

## 예외 클래스의 구분
자바에서 오류는 객체이고, 두가지로 분리된다. 프로그램 처리 도중 기대되지 않는 상황을 Exception이라 하고, 치명적인 오류를 Error로 클래스를 구분한다.

__Exception Class__
* 정확한 프로그램
* 예외가 발생할지 모르는 상황을 체크해 준다.
* 예외가 발생하더라도 프로그램을 중단시키지 않고, 복구하여 프로그램을 지속적으로 실행할 수 있도록 한다.

__Error Class__
* Unchecked Exceptions
* 치명적인 오류로 SW적으로 복구 불가능
* Compiler가 체크하지 않는다.
* Error 클래스를 상속하는 예외는 처리의 대상이 아니다. 즉, 처리할 수 있는 예외가 아니다.
* VirtualMachine Error : 가상 머신에 심각한 오류 발생.
* IO Error : 입출력 관련해서 코드 수준 복구가 불가능한 오류 발생.

Exception Class를 상속하는 예외 클래스는 2가지 부류로 나눌 수 있다.
첫번째는 Runtine Exception을 상속하는 예외 클래스와 Runtime Exception 클래스를 제외한 Exception 클래스를 상속하는 예외 클래스이다.

__Checked__ : Runtime Exception을 제외한 모든 Exception
* 예기치 못한 경우 경미한 오류로 SW적으로 복구 가능하기 때문에 오류가 발생하여 프로그램이 중단되는 걸 막기 위해 컴파일 시에 컴파일러가 체크하여 복구 코드를 구현하도록 한다.

__Unchecked Exception__ : Error, Runtime Exception
* Error : SW적으로 복구가 불가능하기 때문에, 컴파일러가 체크하지 않는다.
* Runtime Exception : 코드 작성이 잘못된 오류로 compile 시에 체크되지 않고, 실행 시에 오류가 발생한다.

즉, Error 클래스를 상속하는 예외나 RuntimeException클래스를 상속하는 예외의 경우 예외의 처리는 선택이다. 그러나 (RuntimeException을 상속하지 않는) Exception 클래스를 상속하는 예외는 try-catch문으로 처리하거나 다른 영역으로 넘긴다고 반드시 명시해야 한다.

## RuntimeException을 상속하는 예외 클래스
ArithmeticException : 0으로 나누기를 실행하였을 때.

NullPointerException : 객체를 생성하지 않고 사용했을 때.

NegativeArraySizeException : 배열의 크기를 음수 값을 주었을 때.

ArrayIndexOutOfBoundsException : 배열의 크기를 벗어났을 때.

SequrityException : 보안에 위배되는 기능을 수행할 때.


## Exception Handling
위에서 작성한 것처럼 예외가 발생할지 모르는 코드는 먼저 try~catch 블록으로 감싼다.

여러가지 예외가 있을 경우 catch 블록을 여러개 정의하던, 위에서처럼 이어서 작성한다. 다만, Exception클래스들이 상속 관계일 경우는 서브클래스부터 정의하여야 올바르게 구현된다.

그 후, 예외 발생 여부와 관계없이 마지막으로 꼭 수행해야 하는 문장이 있다면 finally 블록으로 구현한다.

__Declare Exception__
예외를 처리하지 않고, 호출한 메서드에게 예외처리를 넘겨 버릴 수 있다. 호출한 메서드에게 처리 중 예외가 발생했음을 알리기 위해 사용.
* throws : 예외가 발생할 경우 처리를 호출한 쪽으로 인가한다.

또한, VM이 발생시키지 않은 예외를 개발자가 직접 문제가 있는 상황에 대해 예외를 발생시킬 수 있다.

ValueException이란 클래스를 Exception 클래스를 상속받아 설계하고, 객체를 생성하여 

throw new ValueException() 의 문장으로 예외를 발생시킨다.

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__

__[[JAVA] throw와 throws의 차이점](https://vitalholic.tistory.com/246)__

__[[JAVA] 자바 예외처리 Try Catch문 사용법 - 코딩팩토리](https://coding-factory.tistory.com/280)__