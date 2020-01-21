---
title: "[Java] String 클래스"
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
자바에서는 문자열 표현을 위해 String 클래스를 제공하고 있다.

문자열을 표현하기 위해서는 String 클래스의 인스턴스를 생성해야한다.

## String 클래스의 인스턴스 생성
문자열 표현을 위한 인스턴스 생성 방법은 일반적인 인스턴스 생성 방법과 차이가 없다.

```java
String str = new String("hello World");
```

이렇게 인스턴스가 생성되면, str이 참조하는 String 인스턴스 내부에 문자열 "hello World"가 담기게 된다.

출력은 System.out.println(str)로 확인해볼 수 있다.

println 메소드는 String 인스턴스의 참조 값이 인자로 전달 가능하다.

반면 다른 방법으로 인스턴스를 생성할 수도 있는데 아래와 같은 방법이 new를 이용한 방법보다 보편적인 String 인스턴스 생성 방법이다.

```java
String str = "hello World";
```

이어서 아래의 코드를 보자.

```java
class StringInst{
    public static void showString(String str){
        System.out.println(str);
    }
    public static void main(String[] args){
        showString("hello World");
    }
}
```

showString("hello World"); 문장을 보면 마치 문자열을 통째로 전달하는 듯한 모습을 보인다.

그러나 메소드에는 __문자열이 아닌 "hello World"를 대상으로 만들어진 String 인스턴스의 참조 값이 전달 된다.__

위 문장이 실행되면 일단 "hello World"를 대상으로 String 인스턴스가 생성된다. 그리고 이 인스턴스의 참조 값이 문자열 선언을 대신하게 된다.

예를 들어, 문자열 선언을 통해 생성된 인스턴스 참조 값이 Ox1234라고 가정하면, 위의 문장은 메소드 호출 전(인스턴스 생성 후) 아래와 같은 상황이 된다.

showString(Ox1234);

이렇기 때문에 다음과 같이 매개변수 선언이 String 형 참조변수로 선언된 메소드는 문자열을 인자로 전달 받을 수 있게 된다.

## 문자열 생성을 위한 두 가지 방법의 차이점은?

__String str = new String("My String");__

__String str = "My String";__

이 두가지의 인스턴스 생성 방법은 어떤 차이가 있을까?

일단 == 연산에 대해 잠깐 말을 해보겠다.

참조 변수를 대상으로 한 == 연산은 __참조 변수의 참조 값__ 에 대한 비교 연산을 진행한다.

아래의 코드를 봐보자.

```java
String str1 = "hello World";
String str2 = "hello World";

String str3 = new String("hello World");
String str4 = new String("hello World");

if(str1==str2){
    System.out.println("true"); //여기 실행
}
else{
    System.out.println("false");
}

if(str3==str4){
    System.out.println("true");
}
else{
    System.out.println("false"); //여기 실행
}
```

이런 실행 결과를 볼 때, str1과 str2가 참조하는 인스턴스는 동일 인스턴스이고, str3와 str4가 참조하는 인스턴스는 서로 다른 인스턴스임을 알 수 있다.

기본적으로 이런 차이를 보이는 이유는 String 클래스가 Immutable 인스턴스이기 때문이다.

즉, 변경할 수 없는 인스턴스라는 뜻인데, 생성된 인스턴스 내부에 문자열 "hello World"를 지니게 되면, 이 내용은 인스턴스가 소멸될 때 까지 바꿀 수 없게 된다.

## 문자열 내용 비교
String 인스턴스가 지니는 문자열의 내용을 비교하기 위해서는 equals 라는 메소드를 사용해야 한다.

코드를 통해 알아보자.

```java
String str1 = new String("hello");
String str2 = new String("hello");

if(str1 == str2){
    System.out.println("true"); //실행 안됨
}
if(str1.equals(str2)){
    System.out.println("true"); //실행 됨
}
```

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__


