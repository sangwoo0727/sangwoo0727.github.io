---
title: "[Java] String 클래스의 메소드"
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
String 클래스에는 엄청 많은 메소드가 정의되어 있다.

이번 포스팅에서는 책 내용을 바탕으로 몇 가지 메소드를 적을 예정인데, 나머지 메소드나 다른 클래스에 대해서는 JDK 문서를 참조하자.

JDK 문서를 참고하는 습관을 들이자!

## 문자열 연결시키기 : Concatenating
String 클래스의 다음 메소드를 이용하면 두 문자열을 연결시킨 문자열을 결과로 얻을 수 있다.

<i class="far fa-sticky-note"></i> **public String concat(String str)**  
{: .notice--info}
{: .text-justify}

사용 방법은 아래와 같다.

__String1.concat(String2);__ //String1과 String2를 연결한 결과를 반환.

새로운 문자열은 인스턴스 형태로 만들어지고 이렇게 만들어진 인스턴스의 참조 값이 반환된다.

코드를 보자.

```java
String s1 = "Coffee ";
String s2 = "& Bread";

String s3 = s1.concat(s2);
System.out.println(s3); //Coffee & Bread

String s4 = "Fresh".concat(s3);
System.out.println(s4); //FreshCoffee & Bread
```

s4 역시 가능한 이유는 큰따옴표를 이용한 문자열의 표현은 String 인스턴스의 생성으로 이어지고, 이 문자열의 위치에 생성된 인스턴스의 참조 값이 반환된다.

또한 concat 메서드는 연결하여 호출을 할 수 있다.

```java
String str = "AB".concat("CD").concat("EF");
System.out.println(str); // "ABCDEF"
```

## 문자열 일부 추출하기 : Substring
아래의 메소드를 이용하면 문자열의 뒷부분을 별도의 문자열로 추출할 수 있다.

<i class="far fa-sticky-note"></i> **public String substring(int beginIndex)**  
{: .notice--info}
{: .text-justify}

```java
String str = "abcdef";
str.substring(2); //인덱스 2 이후의 내용으로 이루어진 "cdef" 반환
```

인자로 전달되는 숫자는 문자열의 인덱스값이다. 즉 맨 앞글자의 인덱스 값은 0.

substring 메소드는 오버로딩이 되어있는데, 아래와 같이 두 개의 인자를 전달받는 메소드 역시 정의되어 있으며, 이 메소드를 통해 문자열의 중간 부분을 추출할 수 있다.

<i class="far fa-sticky-note"></i> **public String substring(int beginIndex, int endIndex)**  
{: .notice--info}
{: .text-justify}

```java
String str = "abcdef"
str.substring(2,4); //인덱스 2~3에 위치한 내용으로 이루어진 "cd" 반환
```

## 문자열의 내용 비교 : comparing
아래 메소드를 이용하면, 두 개의 String 인스턴스가 지니는 문자열의 내용을 비교할 수 있다.

<i class="far fa-sticky-note"></i> **public boolean equals(Object,anObject)**  
{: .notice--info}
{: .text-justify}

메소드 equals 호출 결과 두 인스턴스가 지니는 문자열이 같으면 true, 다르면 false를 반환한다.

아직 상속에 대해 배우지 않았지만, 책 내용에 따르면 메소드의 매개변수 선언이 Object형으로 되어 있지만 String 인스턴스의 참조 값을 전달할 수 있다고 한다.

상속을 배우고 나서 다시 보러오자!

```java
String str = "my house";
boolean isSame = str.equals("my house");
```

두 문자열의 내용 비교를 __사전 편찬 순서__ 를 기준으로 조금 더 구체적으로 진행하고 싶다면 아래의 메소드도 있다.

<i class="far fa-sticky-note"></i> **public int compareTo(String anotherString)**  
{: .notice--info}
{: .text-justify}

이 메소드는 두 문자열의 사전 편찬 순서를 비교한다.

-> 두 문자열의 내용이 일치하면 0을 반환

-> 호출된 인스턴스의 문자열이 인자로 전달된 문자열보다 앞서면 0보다 작은 값 반환

-> 호출된 인스턴스의 문자열이 인자로 전달된 문자열보다 뒤서면 0보다 큰 값 반환

이때 0보다 큰 값은 1이 될 수도 있고 99가 될 수도 있다. 

CompareTo 메소드와 유사한 다음 메소드도 존재한다.

<i class="far fa-sticky-note"></i> **public int compareToIgnoreCase(String str)**  
{: .notice--info}
{: .text-justify}

이 메소드의 경우 문자열 비교에 있어서 대소문자를 구분하지 않는다. 사전 편찬 순서에 있어 대문자는 소문자보다 앞에 위치하지만, 이 메소드는 대소문자를 고려하지 않는다.

## 기본 자료형의 값을 문자열로 바꾸기
String 클래스에 정의되어 있는 다음 메소드를 호출하면 기본 자료형의 값을 문자열로 바꿀 수 있다.

<i class="far fa-sticky-note"></i> **static String valueOf(...)**  
{: .notice--info}
{: .text-justify}

인자로는 다양한 자료형을 넣을 수 있다.

클래스 메소드이므로 사용 방법도 간단하다.

```java
double e = 2.718;
String se = String.valueOf(e);
```

## 문자열을 대상으로 하는 + 연산
System.out.println("funny"+"camp");

이런 문법을 많이 사용해봤을거라 생각한다.

문자열을 대상으로 +연산이 가능한 이유는 컴파일러에 의해서 +연산이 concat 메소드의 호출로 바뀐다.

그러면 아래의 경우는 어떨까?

String str = "age: "+20;

이 문장 역시 가능한데, 이 문장도 컴파일러가 String str = "age: ".concat(17); 로 처리할까?

이건 컴파일 오류가 발생한다.

따라서 위의 문장은 String str = "age: ".concat(String.valueOf(17));로 처리된다.

## 문자열 결합의 최적화
아래 코드는 자바 컴파일러가 어떻게 처리할까?

```java
String name = "<이름>" + 25 + '.' + "강상우";
```

앞에서 공부한 내용을 바탕으로 생각을 해보면 아래와 같을 것이다.

```java
String name = "<이름>".concat(String.valueOf(25)).concat(String.valueOf('.')).concat("강상우");
```

이렇게 실행하여도 문제는 없지만, __기본 자료형의 값을 문자열로 변환하는 과정을 여러 번 거쳐야 한다__ 는 문제가 생긴다.

기본 자료형의 값을 문자열로 변환하는 일은 __인스턴스의 생성__ 으로 이어지고, 성능에 영향을 미치기 때문.

이러한 문제를 해결하기 위해 StringBuilder라는 클래스가 제공된다. 

StringBuilder 클래스로 문자열을 구성하면 기본 자료형의 값을 문자열로 변환할 필요도 없고, 인스턴스 생성도 일어나지 않는다. 

## StringBuilder 클래스
StringBuilder 클래스는 내부적으로 문자열을 저장하기 위한 메모리 공간을 지니고, String 클래스의 메모리 공간과 다르게 문자를 추가하거나 삭제하는 것이 가능하다.

수정하면서 유지해야 할 문자열이 있다면, StringBuilder에 담아서 관리하는 것이 효율적이다.

<i class="far fa-sticky-note"></i> **public StringBuilder append(int i) :** 기본 자료형 데이터를 문자열 내용에 추가   
{: .notice--info}
{: .text-justify}


<i class="far fa-sticky-note"></i> **public StringBuilder delete(int start, int end) :** 인덱스 start에서부터 end이전까지의 내용을 삭제 
{: .notice--info}
{: .text-justify}


<i class="far fa-sticky-note"></i> **public StringBuilder insert(int offset, String str) :** 인덱스 offset의 위치에 str에 전달된 문자열 추가 
{: .notice--info}
{: .text-justify}

<i class="far fa-sticky-note"></i> **public StringBuilder replace(int start, int end, String str) :** 인덱스 start에서부터 end이전까지의 내용을 str 문자열로 대체 
{: .notice--info}
{: .text-justify}

<i class="far fa-sticky-note"></i> **public StringBuilder reverse() :** 저장된 문자열의 내용을 뒤집는다. 
{: .notice--info}
{: .text-justify}

<i class="far fa-sticky-note"></i> **public String substring(int start, int end) :** 인덱스 start에서부터 end이전까지의 내용만 담은 String 인스턴스의 생성 및 반환
{: .notice--info}
{: .text-justify}

<i class="far fa-sticky-note"></i> **public String toString() :** 저장된 문자열의 내용을 담은 String 인스턴스의 생성 및 반환
{: .notice--info}
{: .text-justify}

추가로, StringBuilder 인스턴스 내부에는 문자열 관리를 위한 메모리 공간이 존재하는데, 인스턴스 생성 과정에서 아래와 같은 방식으로 크기를 지정해줄 수 있다.

StringBuilder sb = new StringBuilder(64);

StringBuilder 인스턴스는 메모리 공간을 스스로 관리해서 부족하면 그 공간을 늘리지만 사용 계획에 따라 적절한 크기를 초기에 만들면 그만큼의 성능 향상을 기대할 수 있다.



## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__
