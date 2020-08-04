---
title: "[Java] 제네릭(Generics)"
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
이번 포스팅에서는 자바 5에서 처음 소개된 제네릭에 대해 알아보겠다.

제네릭(Generic)이 갖는 의미는 자료형에 대한 일반화이다. 즉, 클래스에서 사용할 타입을 외부에서 설정하는 것을 말한다.

자세히 알아보자.

## 제네릭 사용 이전의 코드

```java
class Box{
    private Object ob;
    public void set(Object ob){
        this.ob = ob;
    }
    public Object get(){
        return ob;
    }
}

class FruitAndBox{
    public static void main(String[] args){
        Box aBox = new Box();
        Box oBox = new Box();
        aBox.set(new Apple());
        oBox.set(new Orange());
        Apple ap = (Apple)aBox.get();
        Orange og = (Orange)oBox.get();
    }
}
```
무엇이든 담을 수 있는 Box 클래스를 오브젝트 타입을 사용하여 만들었기 때문에 여기까진 좋다.

다만, Box내에서 인스턴스를 저장하는 참조변수가 Object형이기 때문에, 저장된 인스턴스를 꺼낼 때에는 인스턴스에 맞는 형 변환을 해야한다.

또한 실수로 다른 형을 담게 될 경우, 꺼내면서 형변환 과정에서 예외가 발생하게 된다.(런타임 시점)

이런 부분들이 제네릭 등장 이전의 불편함이었다. 상자에서 물건을 꺼낼 때 형변환을 해야하는 것이고, 프로그래머의 실수로 인한 예외가 컴파일 시점이 아닌 런타임 시점에 보인다는 문제다.

## 제네릭의 등장
제네릭이 등장하면서 자료형에 의존적이지 않은 클래스를 정의할 수 있게 되었다.

사과를 저장할 목적이면 인스턴스 생성 시 T를 Apple로, 오렌지를 저장할 목적이면 T를 Orange로 정하면 된다.

```java
class Box<T>{ //클래스 이름 뒤에 붙히는 <T>는 인스턴스 생성 시 자료형을 결정하기 위한 표식 ->타입 매개변수
    private T ob;
    public void set(T o){
        ob = o;
    }
    public T get(){
        return ob;
    }
}

class FruitAndBox{
    public static void main(String[] args){
        Box<Apple> aBox = new Box<Apple>(); //T를 Apple로 결정 , Apple을 타입 인자라고 하고, Box<Apple>을 매개변수화 타입이라고 한다.
        Box<Orange> oBox = new Box<Orange>(); 

        aBox.set(new Apple()); 
        oBox.set(new Orange());

        Apple ap = aBox.get(); //형변환이 필요없다.
        Orange og = oBox.get();
    }
}
```

여기서 Box<Apple>을 가리켜 매개변수화 타입이라고 한다. 자료형 Apple이 타입 매개변수 T에 전달이 되어 Box<Apple>이라는 새로운 자료형이 완성된 것이다.

## 다중 매개변수 기반 재네릭 클래스
위에서는 매개변수 T 하나에 대한 제네릭 클래스를 정의하였으나, 둘 이상의 타입 매개변수에 대한 제네릭 클래스를 정의할 수도 있다.

```java
class Score<L,R>{
    private L lScore;
    private R rScore;

    public void set(L l, R r){
        lScore = l;
        rScore = r;
    }
}

class Main{
    public static void main(String[] args){
        Score<Integer,Integer> score = new Score<>();
        score.set(10,15);
    }
}
```

이렇게 타입 매개변수는 이름 짓기 나름이지만, 일종의 관례가 있다. 첫 번째로는 한 문자로 이름을 짓고, 두번 째로는 대문자로 이름을 짓는 것이다.


## 기본 자료형에 대한 제한
제네릭 클래스는 매개변수화 타입(Box<Apple>) 을 구성할 때, 기본 자료형의 이름은 타입 인자로 쓸 수 없다.

즉, Box<int> box가 아닌, Box<Integer> box와 같은 형태로 작성해야 한다.

다만, 이러한 래퍼클래스에 대해서는 자동으로 오토박싱과 오토언박싱이 진행되기 때문에, 편하게 사용하면 된다.

## 제네릭 클래스의 타입 인자 제한하기
위에서 제네릭을 통해 제네릭 클래스를 정의한 Box<T>에는 어떠한 것이든 담을 수 있었다.

하지만, 담고 싶은 것을 제한할 수 있어야 한다.

예를 들어, Number 클래스를 상속하는 클래스의 인스턴스만 담고 싶다면 아래와 같이 정의하면 된다.

```java
class Box<T extends Number> {
    private T num;
    public void set(T num){
        this.num = num;
    }
    public T get(){
        return num;
    }
    //이러한 코드를 넣을 수 있게 된다!!
    public int toIntValue(){
        return num.intValue();
    }
}

class Main{
    public static void main(String[] args){
        Box<Integer> iBox = new Box<>();
        iBox.set(13);

        Box<Double> dBox = new Box<>();
        dBox.set(13.33);
    }
}
```

위와 같이 타입 인자를 Number를 상속하는 클래스로 제한을 하였고, 이를 통해 toIntValue()같은 메소드를 만들 수 있게 되었다.

이전의 extends가 없던 제네릭 클래스는 num이 참조할 인스턴스가 어떠한 클래스의 인스턴스인지 알지 못하기 때문에 저러한 메소드를 사용할 수 없었다.

하지만 이렇게 extends를 통해 Number 클래스의 intValue 메소드를 호출할 수 있게 되었다. Number클래스를 상속하는 모든 클래스의 인스턴스는 intValue()메소드를 가지고 있기 때문이다.

## 제네릭 클래스의 타입 인자를 인터페이스로 제한하기

인터페이스를 통해서도 타입 인자를 제한할 수 있다.

```java
interface Moveable{
    public String move();
}

class Car implements Moveable{
    @Overide
    public String move(){
        return "I can move";
    }
}

class Machine<T extends Moveable>{
    T ob;
    public void set(T o){
        ob = o;
    }
    public T get(){
        System.out.println(ob.move());
        return ob;
    }
}

class Main{
    public static void main(String[] args){
        Machine<Car> machine = new Machine<>();
        machine.set(new Car());
        Car car = machine.get();
    }
}
```

제네릭 클래스의 타입 인자를 인터페이스의 이름으로 제한할 때 역시 extends를 사용한다.

그리고, Moveable 인터페이스를 구현하는 클래스로 타입인자를 제한했기 때문에, 인터페이스에 선언되어있는 move 메소드를 호출할 수 있게 된 것이다.

타입 인자를 제한할 때에는 __하나의 클래스와 하나 이상의 인터페이스__ 에 대해 동시에 제한을 할 수 있다!

```java
class Machine<T extends Number & Moveable>{...}
```

## 제네릭 메소드의 정의
위의 글들은 모두 클래스를 대상으로 한 제네릭이었는데, 메소드에 대해서도 제네릭으로 정의하는 것이 가능하다.

```java
public static Machine<T> makeMachine(T o){...}
```

이렇게 메소드를 정의한다면 우리는 충분히 메소드의 이름이 makeMachine이며, 반환형이 Machine<T>라는 것을 알 수 있다.

하지만, 컴파일러는 T에 대해 알 수 없으므로 T가 타입 매개변수의 선언임을 표시해주어야 한다.

즉, 아래와 같은 코드가 된다.

```java
public static <T> Machine<T> makeMachine(T o){
    Machine<T> machine = new Machine<T>();
    machine.set(o);
    return machine;
}
```

제네릭 클래스는 인스턴스 생성 시 자료형이 결정되지만, 제네릭 메소드는 __메소드 호출시에 자료형이 결정__ 된다.

즉, makeMachine 메소드는 아래와 같이 호출한다.

```java
Machine<String> sMachine = makeMachine("Car");
```

꽤 많은 내용을 포스팅하였다.

다음 포스팅에서는 제네릭의 심화 문법 들에 대해 설명을 해보겠다!

## 출처
__[YABOONG](https://yaboong.github.io/java/2019/01/19/java-generics-1/)__

__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__


