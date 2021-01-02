---
title: "[JAVA] 제네릭 - 객체 생성과 제한된 제네릭 클래스"
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

> 이번 포스팅에서는 제네릭 클래스의 객체를 생성할 때, 주의해야할 점과 타입 변수의 타입 종류를 제한하는 방법에 대해 다룬다.

---
## 제네릭 클래스의 객체 생성과 사용방법

* 먼저, Box<T>의 객체를 생성할 때는, 참조변수와 생성자에 대입된 타입이 일치해야 한다.

```java
Box<Apple> appleBox = new Box<Apple>(); 
```

* 매개변수 타입이 상속관계에 있을 때는 어떨까?
* 다형성때문에 될 것 같은 생각이 든다.
* 한번 살펴보자.

```java
Box<Fruit> appleBox = new Box<Apple>(); // 에러
```

* Apple이 Fruit을 상속받고 있다고 할 때, 흔히 다형성에 의해 동작할 것처럼 느껴지지만, 에러가 발생한다.
* Fruit과 Apple이 상속관계인 것이지 Box< Fruit >과 Box< Apple >은 아무 관련이 없다.
* 우리가 익숙한 다형성을 통한 객체 생성은 아래와 같을 것이다.

```java
Box<Apple> appleBox = new FruitBox<Apple>();
```

* FruitBox는 Box를 상속받고 있다.
* 추가적으로, 추정이 가능한 경우 타입을 생략할 수 있게 되었다.(jdk1.7부터)
* 참조변수 타입으로 Box가 Apple타입의 객체만 저장한다는 것을 알 수 있기 때문에, 생성자에 반복해서 타입을 지정해주지 않아도 된다.

```java
Box<Apple> appleBox = new Box<>();
```

---

## 제한된 제네릭 클래스
* 타입 문자로 사용할 타입을 명시하게 되면, 한 종류의 타입만 저장할 수 있도록 제한을 하였다.
* 하지만, 객체 생성을 할 때, 타입 변수에 모든 종류의 타입을 지정할 수 있다는 것에는 아직 변함이 없다.
* 이렇듯, 타입 매개변수 T에 지정할 수 있는 타입의 종류를 제한할 수 있을까?
* 일반 상속관계처럼, 이때도 extends 키워드를 사용하면 매개변수 T에 지정할 수 있는 타입의 종류를 제한할 수 있게 된다.

```java
public class Box<T extends Car> {
    private T item;
    ArrayList<T> list = new ArrayList<>();

    public T getItem() {return item;}
    public void setItem(T item) {this.item = item;}

    void add(T item) {
        list.add(item);
    }
}
```

* 제네릭 타입을 < T extends Car >로 작성하면서, 여전히 한 종류의 타입만을 담을 수 있지만, Car 클래스의 자손들만 담을 수 있다는 제한이 더 추가된 것이다.
* 추가적으로 add메소드의 매개변수 타입 T 역시 Car와 그 자손 타입이 될 수 있으므로, 여러 차를 담을 수 있는 상자가 가능하게 된다.

```java
Box<Bus> BusBox = new Box<>();
Box<Fruit> FruitBox = new Box<>(); // 에러 - Fruit은 Car의 자손 타입이 아니다.

Box<Car> box = new Box<>();
box.add(new Bus());
box.add(new Car());
```

* 다형성에서 조상 타입의 참조변수로 자손타입의 객체를 가리킬 수 있는 것처럼, 매개변수화된 타입의 자손 타입도 가능한 것이다.
* 다만 제네릭에서는, 인터페이스를 구현해야 한다는 제약이 필요하다면, 이때도 implements가 아닌, extends를 사용한다.

```java
interface Eatable {}
class FruitBox<T extends Fruit & Eatable> {...}
```

* Fruit의 자손이면서 Eatable 인터페이스도 구현해야할때의 문법이다.


---

## 출처
* Java의 정석 - 남궁성

