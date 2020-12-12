---
title: "[Java] 제네릭(Generics)-2"
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
지난 포스팅에 이어서 제네릭에 대해 더욱 깊은 내용을 들어가보도록 하겠다.

## 타겟 타입(Target Types)
자바 컴파일러는 생략된 자료형 정보에 대해 유추하는 능력이 있다.

아래의 코드들을 봐보자.

```java
class Machine<T> {
    private T o;
    public void set(T o){this.o = o;}
    public T get(){return o;}
}
class MakeClass{
    public static <T> Machine<T> makeMachine(){
        Machine<T> machine = new Machine<>();
        return machine;
    }
}
public class Main {
    public static void main(String[] args) {
        Machine<Integer> machine = MakeClass.makeMachine();
        machine.set(25);
        System.out.println(machine.get());
    }
}
```

이전 포스팅에서 makeMachine의 인자를 통해서 <T>를 유추할 수 있었다. 하지만 위의 makeMachine 메소드는 인자를 전달받지 않으므로, 메소드를 호출할 때 T에 대한 타입 인자를 전달하여야 한다.

```java
Machine<Integer> machine = MakeClass.<Integer>makeMachine();
```

하지만, 자바 7부터는 MakeClass.makeMachine(); 처럼 생략을 할 수 있게 되었다.

즉, 왼편에 선언된 매개변수의 형을 보고 판단을 하게 되는 것이다. 

T의 유추에 사용된 정보 Machine<Integer>를 가리켜 타겟 타입이라고 하며, 대입 연산자의 왼편에 있는 정보를 가지고 컴파일러는 유추를 하게 되는 것이다!

## 와일드 카드(Wildcard)
