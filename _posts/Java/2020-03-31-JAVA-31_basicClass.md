---
title: "[Java] 기본 클래스 - Arrays 클래스"
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
자바의 java.util 패키지에는 프로그램을 개발하는 데 사용할 수 있는 유용한 유틸리티 클래스가 여러개 존재한다.

특히, 그 중 java.Arrays 클래스는 배열 조작에 도움을 주는 메소드들로 채워져 있다.

## 배열의 복사
Arrays 클래스에 있는 배열 복사 메소드는 아래와 같다. 단 모든 기본 자료형에 대해 오버로딩 되어 있다.

```java
public static int[] copyOf(int[] original, int newLength)
//original에 전달된 배열을 첫 번째 요소부터 newLength 길이만큼 복사
```

copyOf 메소드는 배열을 복사하여 새로운 배열을 생성하는 메소드이다. 첫 번째 매개변수로 원본 배열을 전달받고, 두 번째 매개 변수로 원본 배열에서 새로운 배열로 복사할 요소의 개수를 전달받는다. 그리고 원본 배열과 같은 타입의 복사된 새로운 배열을 반환하게 된다.

만약, 새로운 배열의 길이가 원본 배열보다 길 경우, 나머지 요소에 대해서는 타입에 맞는 기본 값으로 채워지게 된다.

두 번째로는 배열의 일부만 복사하는 Arrays 클래스의 __copyOfRange__ 메소드가 있다.

copyOfRange 메소드는 첫 번째 매개변수로 복사의 대상이 될 원본 배열을 저장받는다.

두번째 매개변수로는 원본 배열에서 복사할 시작 인덱스를 전달받고, 세번째 매개변수로는 마지막으로 복사될 배열 요소의 바로 다음 인덱스를 전달받는다.

그리고, 원본 배열과 같은 타입의 복사된 새로운 배열을 반환한다.

```java
public static int[] copyOfRange(int[] original, int from, int to)
// original에 전달된 배열을 인덱스 from부터 to 이전 요소까지 복사
```

세번째로는, 배열을 새로 생성하지 않고, 존재하는 배열에 복사를 하려는 경우에는 java.lang.System 클래스의 arraycopy 메소드를 사용한다.

```java
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
// 배열 src의 srcPos에서 배열 dest의 destPos로 length만큼 복사

//ex)
int[] org = {1,2,3,4,5};
int[] cpy = new int[3];

System.arraycopy(org,1,cpy,0,3);
//배열 org의 인덱스 1에서 배열 cpy 인덱스 0으로 세 개의 요소를 복사
//-> 2,3,4
```

## 배열의 비교
배열의 내용 비교를 위해서 사용되는 Arrays 클래스의 메소드는 equals 메소드이다.

바로 아래 코드를 봐보자. 물론 이 메소드 역시 모든 기본 자료형에 대해 오버로딩 되어 있다.

```java
public static boolean equals(int[] a, int[] a2)
// a와 a2로 전달된 배열의 내용을 비교하여 true나 false반환

//ex
int[] a1 = {1,2,3,4,5};
int[] a2 = Arrays.copyOf(a1,a1.length);
System.out.println(Arrays.equals(a1,a2)); //true
```

주의할 점으로는, 이 메소드는 Object형 배열에 대해서도 오버로딩 되어있다.

```java
public static boolean equals(Object[] a, Object[] b)
```

이 메소드는 인스턴스의 참조 값을 저장하고 있는 두 배열에 대해서 비교를 진행하고, 참조 값이 아닌 참조하는 인스턴스의 내용을 비교한다.

그리고 이때 Object클래스에 정의된 equals 메소드가 사용된다.

그렇지만 Object 클래스에 정의된 equals 메소드는 모두 알다시피 참조 값 비교를 하기 때문에, 내용 비교가 목적이라면, equals 메소드를 오버라이딩 해야한다.

## 출처
__[http://tcpschool.com/](http://tcpschool.com/java/java_api_arrays)__

__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__


