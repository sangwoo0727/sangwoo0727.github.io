---
title: "[JAVA] 제네릭 메소드"
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

> 지금까지는 제네릭 클래스에 대해서 다뤘다. 이번 포스팅에서는 제네릭 메소드에 대해 다룬다.

## 제네릭 메소드
* Collections.sort() 메소드를 먼저 보자.

```java
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}
```

* Collections.sort 메소드는 대표적인 제네릭 메소드이며, 제네릭 타입의 선언위치는 반환타입 바로 앞이다.
* 제네릭 클래스에 정의된 타입 매개변수와 제네릭 메소드에 정의된 타입 매개변수는 전혀 별개의 것이다.
* 같은 타입 문자 T를 사용하더라도 별개다.

```java
public class FruitBox<T>{
    static <T> void sort(List<T> list, Comparator<? super T> comparator) {
        // ...
    }
}
```

```java
FruitBox<Fruit> fruitBox = new FruitBox<>();
FruitBox.<Car>sort(list, (o1, o2) -> 0);
```

* 위의 코드에서 제네릭 클래스 FruitBox에 선언된 타입 매개변수 T와 제네릭 메소드 sort의 타입 매개변수 T는 전혀 별개의 것이 된다.
* 극단적인 예지만, 이렇게 Fruit과 Car는 전혀 별개의 것이지만 제네릭 메소드와 제네릭 클래스의 타입 매개변수는 관련이 없다는걸 보여주고 있다.

## 제네릭 메소드의 사용
* 이전에 와일드 카드로 작성했던 makeJuice() 메소드를 제네릭 메소드로 수정해보자.

```java
// 와일드 카드로 작성한 방식
public class Juicer {
    static Juice makeJuice(FruitBox<? extends Fruit> box) {
        String tmp = box.get(0) + "";
        return new Juice(tmp);
    }
}
```

```java
// 제네릭 메소드를 이용한 방식
static <T extends Fruit> Juice makeJuice(FruitBox<T> box) {
    String tmp = box.get(0) + "";
    return new Juice(tmp);
}
```

* 이 메소드를 호출할때는 아래와 같이 타입 변수에 타입을 대입해야 한다.
* 하지만, 대부분의 컴파일러는 타입을 추정할 수 있어서 생략 가능하다.

```java
FruitBox<Fruit> fruitBox = new FruitBox<>();
FruitBox<Apple> appleBox = new FruitBox<>();
System.out.println(Juicer.<Fruit>makeJuice(fruitBox)); // Juicer.makeJuice(fruitBox) 가능
System.out.println(Juicer.<Apple>makeJuice(appleBox)); // 위와 동
```

---
## 출처
* Java의 정석 - 남궁성