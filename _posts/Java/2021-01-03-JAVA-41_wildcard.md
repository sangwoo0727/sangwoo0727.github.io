---
title: "[JAVA] 와일드 카드"
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

> 제네릭 타입에 extends를 적용하여, 제한을 시키는 것만으로는 한계가 있다.

> 이를 해결하기 위한 와일드 카드라는 개념에 대해 다룬다.

---
## 제네릭 클래스로 해결이 안되는 부분

* FruitBox를 대입하면 주스를 만들어서 반환하는 행위를 가지고 있는 Juicer라는 클래스가 있고,
* makeJuice()라는 static 메소드로 행위를 구현한다고 해보자.

```java
public class Juicer {
    static Juice makeJuice(FruitBox<Fruit> box) {
        String tmp = box.get(0) + "";
        return new Juice(tmp);
    }
}
```

* Juicer클래스는 제네릭 클래스가 아니고, 제네릭 클래스라고 하더라도 static 메소드는 타입 매개변수 T를 사용할 수 없다.
* 즉, 타입 매개변수대신 특정 타입을 지정해야 한다.

```java
FruitBox<Fruit> fruitBox = new FruitBox<>();
FruitBox<Apple> appleBox = new FruitBox<>();
System.out.println(Juicer.makeJuice(fruitBox));
System.out.println(Juicer.makeJuice(appleBox)); // Error
```

* makeJuice에서 인자로 FruitBox< Fruit >으로 고정해두었기때문에, appleBox는 매개변수가 될 수 없다.
* 그렇다면, makeJuice 메소드를 오버로딩하여 FruitBox< Apple >을 매개변수로 갖는 메소드를 만들면 어떨까?
* 이 부분은 컴파일 에러가 발생하는데, 제네릭 타입이 다른 것만으로는 오버로딩이 성립하지 않는다.
* 왜냐면, 제네릭 타입은 컴파일할 시점에만 사용하고 제거하므로, 두 메소드는 오버로딩이 아니라 메소드 중복정의가 된다.
* 이를 위해 필요한 것이 와일드 카드이다.

## 와일드 카드
* 와일드 카드는 물음표 기호(?)를 이용하여 표현하는데, 
* ? 이 의미는 어떠한 타입도 올 수 있다는 뜻이다.
* 그러면 결국 Object를 선언한 것과 다를 바가 없어서 와일드 카드는 상한과 하한을 정할 수 있다.

> <? extends T> : 와일드 카드의 상한 제한. T와 그 자손들만 가능

> <? super T> : 와일드 카드의 하한 제한. T와 그 조상들만 가능

```java
public class Juicer {
    static Juice makeJuice(FruitBox<? extends Fruit> box) {
        String tmp = box.get(0) + "";
        return new Juice(tmp);
    }
}
```

```java
FruitBox<Fruit> fruitBox = new FruitBox<>();
FruitBox<Apple> appleBox = new FruitBox<>();
System.out.println(Juicer.makeJuice(fruitBox));
System.out.println(Juicer.makeJuice(appleBox)); //OK
// ? extends Fruit이므로 Fruit과 그의 자손들이 다 가능하다. 
```

---
## 출처
* Java의 정석 - 남궁성


