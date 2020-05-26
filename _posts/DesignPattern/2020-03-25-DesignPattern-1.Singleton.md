---
title: "[디자인 패턴] 싱글톤 패턴(Singleton Pattern)"
categories:
  - Network
read_time: false
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
이번 포스팅에서는 싱글톤 패턴에 대해 알아보도록 하겠다.

## 싱글톤 패턴이란
싱글톤 패턴이란 어떤 클래스의 인스턴스가 하나만 만들어지고, 어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴이다.

다시 설명해보자면, 어떤 클래스가 최초 한번만 메모리를 할당하고, 그 메모리에 객체를 만들어 사용을 하되, 생성자의 호출이 반복적으로 이루어져도 실제로 생성되는 객체는 최초 생성된 객체를 반환해주는 것이다.

## 싱글톤 패턴을 사용하는 이유?
쉽게 생각해보면, 객체들 중에서는 하나만 있으면 되는 것들이 있다.

상품 관리 프로그램을 만들었다고 가정하면, 상품을 관리해주는 객체는 한개만 있으면 된다.
DBCP의 스레드 풀, 로그 기록용 객체, 사용자 설정을 처리하는 객체 등이 그 예이다.

이러한 형식의 객체를 사용할 때 인스턴스가 2개 이상 만들어지게 되면 프로그램에 문제가 생길 수 있다.

즉, 싱글톤 패턴은 고정된 메모리 영역을 얻으면서 한번의 new로 객체를 사용하기 때문에 메모리 낭비를 방지할 수 있으며, 싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기 쉽다!

## 기본적인 싱글톤 패턴 만들기
Manager라는 클래스가 있다고 가정해보자! Manager 클래스의 객체를 생성하기 위해서 우리는 new Manager();라는 문장을 통해 객체를 생성해줬다.

또한 다른 곳에서 다시 Manager 클래스의 객체를 만들기 위해 public으로 선언되어 있다면 언제 어디서든지 new Manager(); 문장으로 객체를 생성할 수 있었다.

그런데 만약 아래와 같은 코드가 있다면 어떨까?

```java
public class Manager{
    private Manager(){

    }
}
```

뭐지.. 생성자가 private로 선언되어 있어서 우리는 객체를 만들 수 없게 된다!! 

우리는 지금껏 private 접근제어자는 해당 클래스 안에서만 사용할 수 있다고 배웠다.

그렇다면, 이 클래스는 특이하게 Manager 클래스 안에서 Manager 클래스의 생성자를 호출할 수 있게 된다.

여기서 약간의 싱글톤 패턴을 만드는 방법이 떠오르게 된다.

```java
public class Manager {
    private static Manager manager = new Manager();
    private Manager() {}
    public static Manager getInstance(){
        return manager;
    }
}
```

여기까지는 자바의 기본적인 접근제어자에 대해 알고있다면 어렵지 않게 할 수 있을 것이다!

확인을 위해 Manager 객체를 담은 리스트를 통해 각 객체가 같은 객체인지를 확인해보자.

```java
public class Main {
    public static void main(String[] args) {
        List<Manager> managerList = new ArrayList<>();
        for(int i=0; i<5; i++){
            managerList.add(Manager.getInstance());
        }
        managerList.forEach(System.out::println);
    }
}
```

![](/assets/img/DesignPattern/20200525.png)

## 기본적인 싱글톤 패턴의 문제점 -  자원 낭비
위에서 만든 디자인 패턴 방법을 __Eager initilization__ 이라고 한다.

즉, 이른 초기화에서는 싱글톤 클래스의 객체가 클래스가 로딩되는 시점에 생성된다.

이 방법은 싱글톤 클래스를 만드는 가장 간단한 방법이지만, 이 객체를 사용하지 않을 경우에도 인스턴스가 생성되는 단점이 존재한다. 즉, 사용하지 않는 것을 만드는 자원낭비가 발생한다.

또한, 대부분의 경우 싱글톤 클래스들은 파일시스템이나 DB연결등과 같은 자원들을 위해 만들어지므로, 많은 자원을 사용하게 된다.

그러므로 프로그램은 클라이언트가 getInstance 메소드를 호출하지 않는다면, 객체 생성하는 것을 피하는 것이 좋을 것이다!

그렇다면 어떤 방법이 있을까? 조금만 생각해보면 충분히 방법을 생각할 수 있는데 이른 초기화의 반대 개념을 떠올리면 될 것이다.

이것을 __Lazy Initialization__, 즉 게으른 초기화라고 한다.

아래의 코드를 봐보자.

```java
public class Manager {
    //Manger 클래스의 유일한 인스턴스를 저장하기 위한 정적 변수
    private static Manager manager;
    //생성자를 private으로 선언하였기 때문에 Manger 클래스에서만 클래스의 인스턴스를 만들 수 있다.
    private Manager() {
    }
    //getInstance 메소드에서는 클래스의 인스턴스를 만들어서 리턴해준다.
    public static Manager getInstance(){
        if(manager==null){
            manager = new Manager();
        }
        return manager;
    }
}
```

클라이언트가 getInstance 를 호출하기 전까지 Manager 클래스의 객체는 생성되지 않는다.

이제는 모든 것이 잘 해결된 싱글톤 패턴같이 보인다! 하지만 또 문제가 생겨버린다.

## Lazy Initialization 싱글톤 패턴의 문제점 - 다중 스레드 환경
위에서 사용한 Lazy Initialization 메소드는 단일 스레드 환경에서는 잘 작동한다. 하지만 멀티 스레드 환경에서 만약 멀티 스레드들이 동시에 if 조건에 접근한다고 생각해보자.

이 경우, Manager 인스턴스가 여러개 생기는 싱글톤 법칙에 위배되는 심각한 문제가 발생한다!!

우리는 thread-safe한 싱글톤을 만들어야 한다.

이때 가장 단순한 방법으로 한번에 오직 한 스레드만 메소드에 접근 가능하도록 동기화 즉, synchronized 키워드를 사용하여 만드는 것이다.

```java
public class Manager {
    private static Manager manager;
    private Manager() {}
    public static synchronized Manager getInstance(){
        if(manager == null){
            manager = new Manager();
        }
        return manager;
    }
}
```

이렇게 synchronized 키워드를 붙히면 한 스레드가 메소드 사용을 끝내기 전까지 다른 스레드는 기다려야 한다. 즉, 두 스레드가 이 메소드를 동시에 실행시키는 일은 발생하지 않게 된다.

그렇다면 역시 싱글톤 패턴은 완벽하게 해결된 것일까?

우리는 synchronized 키워드가 성능저하를 유발하며, 멀티스레드의 장점을 없애기 때문에 남발하면 좋지 않다는 것에 대해 알고 있다.

## Synchronized 를 사용한 thread-safe한 싱글톤의 문제점 - 성능 저하
위의 코드를 다시 한번 조금만 생각해보면, getInstance() 메소드를 동기화를 시키기에 굉장히 아깝다는 생각이 든다.

사실 동기화가 필요한 시점은 처음 Manager 클래스의 객체가 만들어지는 시점이다.

즉, 한번 객체가 생성되고 난 후에는 굳이 이 메소드를 동기화된 상태로 유지시킬 필요가 없다는 것이다!

이제 우리는 __DCL(Double-checking Locking)__ 을 써서 getInstance()에서 동기화 되는 부분을 줄여줄 것이다!

```java
public class Manager {
    private volatile static Manager manager;
    private Manager() {}
    public static Manager getInstance(){
        if(manager == null){
            synchronized (Manager.class) {
                if (manager == null) {
                    manager = new Manager();
                }
            }
        }
        return manager;
    }
}
```

이 코드를 통해 인스턴스가 있는지 확인하고, 없으면 동기화 블럭으로 들어가게 된다. 이렇게 하면 처음 인스턴스를 생성할 때만 동기화를 하게 되어 오버헤드를 줄일 수 있게 된다!!! 드디어 싱글톤 패턴을 완성하였다!

volatile 키워드에 대한 설명은 자바 카테고리의 포스팅을 참조하자!

다소 여러번의 과정에 걸쳐 포스팅을 하여 복잡해보일지 모르지만, 천천히 생각하며 흐름을 이해한다면 어렵지 않게 이해할 수 있을 것이다!

## 참조
Head First Design Patterns - O'REILLY

[WIKIPEDIA-Singleton Pattern](https://en.wikipedia.org/wiki/Singleton_pattern)

[jeong-pro.tistory.com](https://jeong-pro.tistory.com/86)





