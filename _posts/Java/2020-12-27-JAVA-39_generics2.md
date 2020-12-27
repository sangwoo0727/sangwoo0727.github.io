---
title: "[JAVA] 제네릭 - 추가공부"
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

> 이전 포스팅에서 제네릭에 대해 한번 다룬적이 있다.

> 내용이 다소 어려워서 이번 포스팅에서 다시 한번 다루려 한다.

## 제네릭이란?
* 제네릭은 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입체크를 해주는 기능이다.
* 객체의 타입을 컴파일 시에 체크해서 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.

> 타입 안정성을 높인다는 것은 의도치 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내올 때 잘못된 형변환으로 발생하는 오류를 막아준다는 것을 의미한다.

* 즉 제네릭을 사용하면 대표적으로 두가지 장점이 생긴다.

> 1. 타입 안정성을 제공

> 2. 타입체크와 형변환을 생략하여 코드가 간결해진다.

* 제네릭 타입은 클래스와 메소드에 모두 선언할 수 있다.
* 클래스에 선언하는 방식에 대해 먼저 살펴보자.

## 제네릭 클래스의 선언

```java
public class Box {
    Object item;
    public Object getItem() {
        return item;
    }
    public void setItem(Object item) {
        this.item = item;
    }
}
```

* 어떠한 자료형의 item이든 담을 수 있는 Box 클래스가 있다고 할 때, 형변환은 필수가 된다.
* Box는 다양한 종류의 타입을 다루는 클래스이기 때문에, item은 Object 형으로 선언했었다.
* 그로인해, 아래와 같이, 넣는 item의 타입에 맞춰 꺼낼 경우에 형변환이 필요했었다.

```java
Box box = new Box();
String item = "My item";
box.setItem(item);

String content = (String) box.getItem(); 
int item1 = 12;
box.setItem(item1);
int content1 = (int) box.getItem();
```

* 다음은 똑같은 Box 클래스를 제네릭 클래스로 변경한 코드이다.

```java
public class Box<T> {
    T item;
    public T getItem() {
        return item;
    }
    public void setItem(T item) {
        this.item = item;
    }
}
```

* Box<T>에서 T를 타입 변수라고 한다.
* 타입 변수는 T가 아닌 다른 것을 사용해도 된다.
* 다양한 종류의 타입을 다루는 Box 클래스같은 클래스나 메소드는 매개변수나 리턴타입으로 Object타입의 참조변수를 많이 사용했고, 그로 인해 형변환이 불가피했지만, 제네릭이 등장하고부터는 그러한 번거로움이 사라졌다.

```java
Box<String> box = new Box<>();
box.setItem(12); // int형 저장 불가능 -> 컴파일 에러
box.setItem("abc"); // 타입 변수를 String으로 지정해서, 문자열 객체 저장 가능
String item = box.getItem(); // 형변환 필요x
```

* 위와 같이 T의 타입변수에 String으로 box 객체를 생성하였으니, Box 클래스는 아래와 같은 형태로 정의된 것과 같다.

```java
class Box{
    String item;
    void setItem(String item){this.item = item;}
    String getItem(){return item;}
}
```

> 제네릭 이전의 코드와의 호환성을 위해, 제네릭 클래스여도 이전 방식으로 객체를 생성해도 허용된다. 경고문만 발생한다.

---

## 제네릭 용어

```java
Class Box<T> {
    // ...
}
```

* Box<T> : 제네릭 클래스
* T : 타입 변수 또는 타입 매개변수(T는 타입 문자)
* Box : 원시 타입(raw type)

```java
Box<String> box = new Box<>();
```

* 위와 같이 객체를 생성할 때, 타입 매개변수에 타입을 지정하는 것을 제네릭 타입 호출이라하고, 지정된 타입 String을 매개변수화된 타입(대입된 타입)이라고 한다.
* 매개변수라고 칭한 이유는 메소드의 매개변수와 유사하기 때문이다.
* Box<String>과 Box<Integer> 는 제네릭 클래스에 서로 다른 타입을 대입한 것일뿐, 별개의 클래스가 아니다.
* 메소드의 매개변수를 달리해서 호출한 것과 같다.
* Box<String>과 Box<Integer>는 컴파일 후에, 원시타입인 Box로 바뀐다.

---

## 제네릭의 제한
* 제네릭 클래스를 작성할 때 제한되는 것들이 있다.

### 1. static 멤버에 제네릭 사용
* 제네릭 클래스는 객체를 생성할 때, 객체별로 다른 타입을 지정한다.
* 하지만 객체 생성시점에 생성되는 인스턴스 변수나 메소드가 아닌 static 멤버는 타입 변수 T를 사용할 수 없다.
* 즉, static item 에 대해, Box<String>.item과 Box<Integer>.item은 같은 것이어야 한다.

### 2. 제네릭 타입의 배열 생성 허용
* 제네릭 타입의 배열을 생성하는 것 역시 허용이 되지 않는다.
* 물론 제네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만, 배열을 new 연산자를 통해 생성하는 것은 안된다.
* 제네릭 배열을 생성할 수 없는 이유는, new 연산자때문인데, new 연산자는 컴파일 시점에 타입 T가 무엇인지 정확히 알아야한다.
* 제네릭 배열을 생성해야 할 필요가 있을때는, Reflection API의 newInstance()나, Object 배열을 생성해 복사한다음에 T[]로 형변환하는 방법등을 사용한다.
* Reflection API 등에 대해서는 공부후에 다시 작성해보겠다.




