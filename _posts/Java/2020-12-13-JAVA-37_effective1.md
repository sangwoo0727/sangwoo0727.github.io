---
title: "이펙티브 자바 - 1 생성자 대신 정적 팩터리 메소드를 고려하라"
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
## 정적 팩토리 메소드
* 기본적인 인스턴스 생성은 public 생성자를 통해 생성한다.
* 이 방법뿐만 아니라, 정적 팩토리 메소드를 통해 인스턴스를 생성하는 방법이 있다.
* 이 포스팅에서는 무작정 public 생성자를 이용하는 방법이 아닌 정적 팩토리 메소드를 사용하면 얻는 장점과 단점 등에 대해 다룬다.

```java
BigInteger biUseConstructor = new BigInteger(String.valueOf(10L));
BigInteger biUseStatic = BigInteger.valueOf(10L);
```

* 위의 두줄의 코드에서 첫번째 객체 생성 방법이 public 생성자를 이용한 방식이다.
* 두번째 객체 생성 방법이 정적 팩토리 메소드를 통해 생성하는 방법이다.

```java
/**
* Returns a BigInteger whose value is equal to that of the
* specified {@code long}.
*
* @apiNote This static factory method is provided in preference
* to a ({@code long}) constructor because it allows for reuse of
* frequently used BigIntegers.
*
* @param  val value of the BigInteger to return.
* @return a BigInteger with the specified value.
*/
public static BigInteger valueOf(long val) {
    // If -MAX_CONSTANT < val < MAX_CONSTANT, return stashed constant
    if (val == 0)
        return ZERO;
    if (val > 0 && val <= MAX_CONSTANT)
        return posConst[(int) val];
    else if (val < 0 && val >= -MAX_CONSTANT)
        return negConst[(int) -val];

    return new BigInteger(val);
}
```

* 쉽게말해서, 위와 같이, static 메소드를 이용하여 객체를 생성하는 방법이다.
* 위의 코드는 java에서 제공하는 static 팩토리 메소드를 이용하여 BigInteger를 생성하는 코드이다.
* 정적 팩토리 메소드를 사용할 때의 장점을 먼저 알아보자.

---

## 정적 팩토리 메소드 장점

### 1. 이름을 가질 수 있다

* 정적 팩토리는 이름을 잘지으면, 반환될 객체의 특성을 쉽게 묘사할 수 있다.
* 생성자는 매개변수와 생성자만으로는 반환될 객체의 특성을 설명하지 못한다.
* BigInteger(int, int, Random) 과 BigInteger.probablePrime 중 값이 소수를 반환한다는 것을 쉽게 유추할 수 있는 쪽은 후자이다.
* 입력 매개변수로 오버로딩하여 생성자를 추가하는 방식은 어떤 역할을 하는지 정확하게 파악하기 어렵다.

> 한 클래스에 시그니처가 같은 생성자가 여러 개 필요할 것 같으면, 생성자를 정적 팩토리 메소드로 바꾸 고 각각의 차이가 잘 드러나는 이름을 짓자.

### 2. 호출될 때마다 인스턴스를 생성하지 않아도 된다.
* 정적 팩토리 메소드를 사용하면, 인스턴스 통제가 가능하다.

```java
public static BigInteger valueOf(long val) {
    // If -MAX_CONSTANT < val < MAX_CONSTANT, return stashed constant
    if (val == 0)
        return ZERO;
    if (val > 0 && val <= MAX_CONSTANT)
        return posConst[(int) val];
    else if (val < 0 && val >= -MAX_CONSTANT)
        return negConst[(int) -val];

    return new BigInteger(val);
}
```

* 위에서 보였던 BigInteger 객체를 정적 팩토리 메소드를 이용하여 반환하는 방식이다.
* val이 0일 때는 새로운 객체를 생성하지 않고, 만들어둔 객체를 리턴하게 된다.
* 이렇듯, public 생성자를 무분별하게 사용하지 않고, 정적 팩토리 메소드를 사용하면 인스턴스를 통제할 수 있다.

> 익숙해서 생각해보니, 이러한 방식을 싱글톤 패턴을 만들 때 사용했었다.

### 3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

* 반환할 객체의 클래스를 자유롭게 선택할 수 있는 유연성이 있다.
* 구현 클래스를 공개하지 않고, 그 객체를 반환할 수 있다.
* 인터페이스를 정적 팩터리 메소드의 반환타입으로 사용하는 인터페이스 기반 프레임워크를 만드는 핵심 기술이다.
* 직접 만들어본 예시코드를 작성해보겠다. (틀린 부분이 있으면 댓글로 남겨주시면 감사드리겠습니다.)

```java
// 모든 클래스를 설명을 위해 한 곳에 모아 쓴 상황입니다.

// 인터페이스
public interface Motor {
    static Motor choice(int wheelNum) {
        if (wheelNum <= 4) {
            return new Car();
        } else {
            return new Bus();
        }
    }
}

// 구현 클래스 1
public class Car implements Motor {
}

// 구현 클래스 2
public class Bus implements Motor {
}

// 객체 생성 부분
public class item1 {
    public static void main(String[] args) {
        Motor motor = Motor.choice(5);
    }
}
```

* 위와 같은 코드가 있을 때, 바퀴의 개수에 따라, Motor 인터페이스의 하위 클래스인 Car와 Bus 클래스의 객체를 생성할 수 있다.
* 이렇듯 정적 팩토리 메소드를 사용하면 유연하게 하위 타입 객체를 반환할 수 있다.
* 그리고, 인터페이스를 반환 타입으로 사용할 수 있다.

### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

* 이 부분은 위의 예시와 내용과도 일맥상통한다.
* 결국 반환타입의 하위타입이기만 하면 어떤 클래스의 객체를 반환하든 상관 없고, 이는 3번 내용에서부터 이어지는 내용이라고 생각한다.
* 예시로 나온, EnumSet 클래스의 정적 팩토리 메소드 내부 구조를 살펴보자.

```java
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
    Enum<?>[] universe = getUniverse(elementType);
    if (universe == null)
        throw new ClassCastException(elementType + " not an enum");

    if (universe.length <= 64)
        return new RegularEnumSet<>(elementType, universe);
    else
        return new JumboEnumSet<>(elementType, universe);
}
```

* 원소의 길이가 64개 이하일 때는 RegularEnumSet을, 그 외에는 JumboEnumSet을 반환한다.
* 둘은 모두 EnumSet의 하위 클래스이다.

### 5. 정적 팩토리 메소드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

> 추후 작성 예정

---

## 정적 팩토리 메소드의 단점

### 1. 상속을 하려면 public이나 protected 생성자가 필요하니, 정적 팩토리 메소드만 제공하면 하위 클래스를 만들 수 없다.
* 생성자로 인스턴스를 생성하는 방법을 사용자에게 제공하지 않고, 정적 팩토리 메소드만 제공한다는 말은
* public이나 protected 등의 사용자에게 공개되는 생성자를 숨기겠다는 말이다.
* 즉, private 접근제어자만 사용하여 클래스 내부에서 인스턴스를 생성하여, 정적 팩토리 메소드의 반환값으로 인스턴스를 주겠단 의미이다.
* 간단한 코드를 보자.

```java
// 편하게 보기 위해 코드를 한 곳에 적었다.

public class Car {
    public static Car getInstance() {
        return new Car();
    }
    private Car() {}
}

public class SportCar extends Car {
    // 에러 발생
    // There is no default constructor available in 'Car'
}
```

* 즉, 이런 경우에 하위 클래스를 만들 수 없게 된다.

> 하지만, 이 제약은 상속보다 컴포지션을 사용(item 18)하도록 유도하고 불변 타입으로 만들려면 이 제약을 지켜야한다는 점에서 오히려 장점일 수 있다.


### 2. 정적 팩토리 메소드는 프로그래머가 찾기 어렵다.
* 생성자와 다르게, 정적 팩토리 메소드는 명확하게 설명이 드러나지 않는다.
* 사용하기 위해서는 직접 찾아봐야 한다.
* 관례로 사용하는 명명 방식들을 알아보거나, 직접 정적 팩토리 메소드를 찾아봐야 한다.

---

## 출처
* Effective Java 3/E , Joshua Bloch