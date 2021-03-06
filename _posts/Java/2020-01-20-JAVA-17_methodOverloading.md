---
title: "[Java] 메소드 오버로딩"
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
한 클래스 내에 동일한 이름의 메소드를 둘 이상 정의하는 것은 당연하게도 불가능할 것이다.

하지만 메소드의 매개변수의 선언이 다를 경우 동일한 이름의 메소드 정의가 가능한데, 이를 __메소드 오버로딩__ 이라고 한다.

## 메소드 오버로딩의 조건
호출할 메소드를 찾을 때는 아래의 두 정보를 참조하여 메소드를 찾게 된다.

-> 메소드의 이름

-> 메소드의 매개변수 정보

아래의 코드를 한번 봐보자.

```java
class MyHome{
    void mySimple(int n){
        //...
    }
    void mySimple(int n1, int n2){
        //...
    }
    void mySimple(double n){
        //...
    }
}
```

이렇게 메소드의 이름이 같더라도, 매개변수 선언이 다르면 메소드 호출문의 전달인자를 통해서 호출된 메소드를 구분할 수 있다.

따라서, 매개변수의 선언이 다르면 동일한 이름의 메소드를 정의하는 것을 허용하고 이를 __메소드 오버로딩__ 이라고 한다.

위에서 __매개변수의 선언이__ 달라야 오버로딩이 성립한다고 하였는데, 구체적으로는 매개변수의 수나 형이 달라야 한다.

```java
//매개 변수의 수가 다른 경우
void mySimple(int n){
    //...
}
void mySimple(int n1, int n2){
    //...
}
```

```java
//매개 변수의 형이 다른 경우
void mySimple(int n){
    //...
}
void mySimple(double n){
    //...
}
```

반면, 메소드의 반환형이 다른 경우에는 메소드 오버로딩이 성립하지 않는다.

반환형은 호출할 메소드를 선택하는데 있어서는 구분할 수 있는 기준이 아니기 때문이다!

## 조금 어려운 상황

```java
void simple(int p1, int p2){
    //...
}
void simple(int p1, double p2){
    //...
}
```

위와 같은 상황에선 오버로딩이 가능할까?

당연히 가능하다! 하지만 아래와 같이 메소드를 호출하게 될 경우 어떤 메소드가 호출될까?

```java
simple(7,'K');
```

이런 경우는 메소드의 인자 전달 과정에서 발생하는 형 변환때문에 자동 형 변환 규칙을 적용하여 호출할 메소드를 찾게 된다.

문제는 클래스에 정의된 두 simple메소드 모두 형 변환 규칙을 적용했을 때 호출이 가능하다는 데 있다.

이런 상황은 자동 형변환 규칙을 적용하되, 가장 가까운 위치에 놓여있는 자료형으로의 형 변환을 우선 시도한다.

따라서, int,int 메소드가 호출된다.

이런 상황은 공부를 위해 만든것이지, 실제로 이런 애매한 상황은 만들지 않는 것을 추천한다!

## 생성자 오버로딩
생성자 역시도 오버로딩을 할 수 있다.

아래의 코드를 봐보자.

```java
class Person{
    private int regiNum;
    private int passNum;
    Person(int r,int p){
        regiNum = r;
        passNum = p;
    }
    Person(int r){
        regiNum = r;
        passNum = 0;
    }
}
```

위의 두 생성자는 매개변수 선언이 다르므로 오버로딩 관계에 있다.

## this를 이용한 다른 생성자의 호출
위의 생성자 오버로딩 코드를 다시 한번 봐보자.

두번째로 위치한 생성자를 대신해서 다음과 같이 생성자를 정의할 수 있다.

```java
Person(int r){
    this(r,0);
}
```

위에서 사용된 키워드 __this__ 는 __오버로딩 된 다른 생성자__ 를 의미한다. 즉 매개 변수의 개수가 2개인 다른 생성자를 호출 하는 것!

이와 같이 this를 이용한 생성자의 정의를 통해 중복된 코드의 수를 줄이는 효과를 얻을 수 있게 된다.

## this를 이용한 인스턴스 변수의 접근
위에서 설명한 키워드 this는 다른 의미로도 사용될 수 있다.

아래의 코드를 봐보자.

```java
class Person{
    private int rnum;
    private int pnum;
    Person(int rnum, int pnum){
        this.rnum = rnum; //this.rnum은 인스턴스 변수 rnum을 의미함.
        this.pnum = pnum;
    }
}
```

클래스 Person의 인스턴스 변수로 rnum과 pnum이 선언되었다. 따라서 인스턴스 메소드 내에서, rnum, pnum이라는 이름의 변수로 접근하면 이는 인스턴스 변수 rnum, pnum의 접근으로 이어진다.

그러나! 위의 코드와 같이 매개변수로 rnum과 pnum이 선언되면 상황이 애매해진다.

위와 같이 매개변수의 이름이 인스턴스 변수의 이름과 동일하게 선언된 경우, 선언된 지역 내에서의 해당 이름은 매개변수를 의미하게 된다.

하지만 키워드 this를 이용하면, 이 영역 안에서도 인스턴스 변수에 접근할 수 있게 된다.

즉 이런 상황에서 this.rnum에서 this가 의미하는 것은 이 문장이 속한 인스턴스이다. 

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__



