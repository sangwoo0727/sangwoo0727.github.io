---
title: "[Java] 기본 클래스 - Wrapper 클래스"
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
이번 포스팅부터 Wrapper 클래스부터 몇 가지의 자바 기본 클래스에 대해 포스팅 하려한다.

래퍼 클래스는 말 그대로 감싸는 역할을 하는 클래스이다. 이게 무슨 말인지 자세히 알아보자.

## 기본 자료형을 감싸는 클래스
int형 정수나, double형 실수와 같은 기본 자료형들도 인스턴스로 표현해야 하는 경우가 있다.

이런 경우를 위해 래퍼 클래스가 존재한다.

즉, 래퍼 클래스는 기본 자료형에 대해서 객체로서 인식되도록 포장을 했다는 뜻이다. 객체라는 상자에 기본 자료형을 넣은 상태로 생각하면 쉽다.

기본 자료형 대신 래퍼 클래스를 사용하는 이유는 크게 세가지가 있다.

1. 객체 또는 클래스가 제공하는 메소드 사용
2. 클래스가 제공하는 상수 사용
3. 숫자, 문자로의 형변환 또는 진법 변환 시 사용

간단한 예로, 래퍼 클래스는 toString 메소드를 오버라이딩 하고 있다. 따라서 println 메소드의 인자로 아래와 같이 전달될 수 있으며, 이때 인스턴스가 지니는 값이 출력된다.

```java
Integer wc = new Integer(123); //123의 기본 자료형을 래퍼클래스로 감싼 형태
System.out.println(wc); //123 출력
```

모든 기본 자료형을 대상으로 래퍼 클래스가 정의되어 있다.

* Boolean
* Character
* Byte
* Short
* Integer
* Long
* Float
* Double

## 박싱 & 언박싱

![](/assets/img/java/202003311.png)

이미지 출처: TCP School

기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변환하는 과정을 박싱(Boxing)이라 하고, 래퍼 클래스의 인스턴스에 저장된 값을 다시 기본 타입의 데이터로 꺼내는 과정을 언박싱(UnBoxing)이라 한다. 

박싱은 인스턴스의 생성을 통해서 이뤄지며, 언박싱은 래퍼 클래스에 정의된 메소드의 호출을 통해서 이뤄진다.

```java
class Boxing{
  public static void main(String[] args){
    Integer iBox = new Integer(10); // 박싱
    System.out.println(iBox);
    int iNum = iBox.intValue(); //언박싱
    System.out.println(iNum);
    iBox = new Integer(iBox.intValue()+10); //래퍼 인스턴스 값 증가 방법
    System.out.println(iBox);
  }
}
```

위의 예시 코드에서 알 수 있듯 래퍼 인스턴스는 산술 연산을 위해 정의된 클래스가 아니므로, 인스턴스에 저장된 값을 변경할 수 없다.

래퍼 클래스 별 언박싱 메소드의 이름은 아래와 같다.

* Boolean -> public boolean booleanValue()
* Character -> public char charValue()
* Integer -> public int intValue()
* Long -> public long longValue()
* Double -> public double doubleValue()

## 오토 박싱 & 오토 언박싱
자바 5부터는 컴파일러가 박싱과 언박싱이 필요한 상황에서 이를 자동으로 처리해주기 시작했다. 이렇게 자동화된 박싱과 언박싱을 오토 박싱과 오토 언박싱이라고 한다.

```java
Integer iBox = new Integer(17); //박싱
Integer aBox = 17; //오토 박싱
System.out.println(iBox); //17
System.out.println(aBox); //17

int iNum = aBox.intValue();  //언박싱
int aNum = aBox; //오토 언박싱
System.out.println(iNum); //17
System.out.println(aNum); //17
```

위의 예제 코드에서 보듯이, Integer 래퍼 클래스 객체 aBox 대입 연산자의 오른 편에는 Integer 인스턴스가 와야하는데, 정수 17이 있다. 이러한 상황에서는 정수를 기반으로 Integer 인스턴스가 자동으로 생성된다.(오토 박싱이 이뤄진다)

반대로, aNum의 대입 연산자 오른편에 Integer 인스턴스가 있는데, 이러한 경우 자동으로 intValue() 메소드가 호출 된다. 이를 가리켜 오토 언박싱이라 한다.

오토 박싱과 오토 언박싱을 통해 아래와 같은 연산들도 가능해진다.

```java
Integer num = 10;
num++; 
num+=3;
int n = num + 1; 
Integer nBox = num - 1;  
```

## Number 클래스와 래퍼 클래스의 static 메소드
래퍼 클래스는 java.long.Number 클래스를 상속한다.

이 클래스에는 아래와 같은 추상 메소드들이 존재한다.

```java
public abstract int intValue();
public abstract long longValue();
public abstract double doubleValue();
```

즉, 이를 상속하는 래퍼클래스 Integer, Double 등의 클래스들은 위의 추상메소드를 구현하고 있을 것이다.

```java
Integer n = new Integer(29);
System.out.println(n.intValue()); //int 형 값 반환
System.out.println(n.doubleValue()); //double 형 값 반환
```

반대로 Double 형 인스턴스에 저장된 값을 int형으로 반환할 경우 소수점 이하의 값은 삭제될 것이다.

추가로, 래퍼 클래스에는 static으로 선언된 여러 메소드들이 존재한다.

아래 코드를 통해 설명하며 포스팅을 마무리하겠다.

```java
Integer n1 = Integer.valueOf(3);
Integer n2 = 5;

System.out.println(Integer.max(n1,n2)); // 더 큰 수 5 출력
System.out.println(Integer.min(n1,n2)); // 더 작은 수 3 출력
System.out.println(Integer.sum(n1,n2)); 
System.out.println(Integer.toBinaryString(12)); //2진수 출력
System.out.println(Integer.toOctalString(12)); //8진수
System.out.println(Integer.toHexString(12)); //16진수
```

## 출처
__[http://tcpschool.com/](http://tcpschool.com/java/java_api_wrapper)__

__[https://m.blog.naver.com/itinstructor/](https://m.blog.naver.com/itinstructor/100202885690)__

__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__