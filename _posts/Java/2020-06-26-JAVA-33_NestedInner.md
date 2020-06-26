---
title: "[Java] 네스티드 클래스와 이너클래스"
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
이번 포스팅에서는 네스티드(Nested) 클래스와 이너(Inner)클래스에 대해 소개해보려고 한다.

## 네스티드 클래스 
클래스내에 정의된 클래스를 네스티드 클래스라하고, 이를 감싸는 클래스를 외부(outer) 클래스라고 한다.

네스티드 클래스는 static의 선언 여부를 기준으로 __Static 네스티드 클래스__ 와 __Non-static 네스티드 클래스__ 로 나뉜다.

이 중 Non-static 네스티드 클래스를 __이너(Inner) 클래스__ 라고 한다.

```java
class OuterClass{
    static class staticNestedClass{}
}

class OuterClass{
    class nonStaticNestedClass{} // == 이너 클래스
}
```

그리고, 이너 클래스는 정의되는 위치나 특성에 따라 다시 __멤버 (이너) 클래스__ , __로컬 (이너) 클래스__, __익명 (이너) 클래스__ 로 나뉜다. (이너는 생략하고 부르는 것이 일반적)

## Static 네스티드 클래스
static 선언이 갖는 특성이 반영된 클래스로, 자신을 감싸는 외부 클래스의 인스턴스와 상관없이 static 네스티드 클래스의 인스턴스 생성이 가능하다.

```java
public class Main{
    public static void main(String[] args) {
        Outer.Nested1 nested1 = new Outer.Nested1();
        nested1.add(5);
        Outer.Nested2 nested2 = new Outer.Nested2();
        System.out.println(nested2.get());
    }
}

class Outer{
    private static int num = 0;
    static class Nested1{
        void add(int n) {
            num += n;
        }
    }
    static class Nested2{
        int get(){
            return num;
        }
    }
}
```

이 예제를 보면 금방 이해할 수 있다.

Outer클래스 안에는 두개의 Static 네스티드 클래스가 정의되어 있다.

Outer클래스의 static 변수 num은 Nested1 클래스와 Nested2 클래스의 모든 인스턴스가 공유하게 된다.

그리고, 위의 에제에서 보듯이, Static 네스티드 클래스의 인스턴스 생성은 외부 클래스의 인스턴스의 생성과 무관하다.

즉, Static 네스티드 클래스내에서는 외부 클래스에 인스턴스 변수와 메소드에는 접근이 불가능하지만, static으로 선언된 변수와 메소드에만 접근이 가능하다.

## 이너 클래스 - 1. 멤버 클래스
위의 글에서 이너클래스는 네스티드 클래스 중에서 static 선언이 붙지 않은 클래스를 가리킨다고 하였다.

이너클래스 중 익명클래스는 클래스의 이름이 존재하지 않는 익명의 클래스이다.

반면, 멤버 클래스와 로컬 클래스는 클래스가 정의된 위치에 따라 구분되는데, 먼저 멤버클래스는 인스턴스 변수, 인스턴스 메소드와 동일한 위치에 정의되는 클래스를 말한다.

우리가, 클래스 내에 선언된 변수와 메소드를 멤버 변수, 멤버 메소드라고 부르는 것과 매칭시켜 생각하면 쉽게 생각할 수 있을 듯 하다.

```java
public class Main{
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Member member = outer.new Member();
        member.add(5);
        System.out.println(member.get());
    }
}

class Outer{
    private int num = 0;
    class Member{
        void add(int n){
            num += n;
        }
        int get(){
            return num;
        }
    }
}
```

위의 코드에서 알 수 있듯이, Member 클래스 내에서는 Outer 클래스의 인스턴스 변수에 접근이 가능하다. __멤버 클래스의 인스턴스는 외부 클래스의 인스턴스에 종속적이기 때문이다.__

```java
public class Main{
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Member member = outer.new Member();
        Outer.Member member2 = outer.new Member();
        member.add(5);
        System.out.println(member2.get()); //5
    }
}
```

또한, 위의 코드에서 알 수 있듯이 member와 member2 인스턴스는 outer 인스턴스가 참조하는 인스턴스의 멤버에 접근할 수 있다. 즉, 위의 두 인스턴스는 outer가 참조하는 인스턴스의 멤버를 공유하게 된다.

그렇다면 __멤버 클래스는 언제 유용하게 사용하는 것일까?__

아래의 코드를 봐보자.

```java
interface Printable{
    void print();
}

class Paper{
    private String str;
    public Paper(String s){
        str = s;
    }
    public Printable getPrint(){
        return new Printer();
    }
    private class Printer implements Printable{
        public void print(){
            System.out.println(str);
        }
    }
}
public class Main{
    public static void main(String[] args) {
        Paper paper = new Paper("오늘의 날씨는 맑음");
        Printable prn = paper.getPrint();
        prn.print();
    }
}
```

Printer 클래스는 private으로 선언되어있고, private으로 선언되어있기 때문에 이 클래스는 이 클래스 정의를 감싸는 클래스 내에서만 인스턴스 생성이 가능하다.

그렇기 때문에, 우리는 paper.getPrint()라는 메소드를 통해서 Printer 인스턴스를 생성하지만, getPrint 메소드가 어떠한 인스턴스의 참조 값을 반환하는 지 알지 못한다. 다만 반환되는 참조 값의 인스턴스가 Printable을 구현하고 있어서 Printable의 참조 변수로 참조할 수 있다는 사실만 알 뿐이다.

이렇듯 __클래스의 정의가 감추어진 상황__ 에서 멤버 클래스는 유용하게 사용된다.

또한 클래스의 정의를 감추면, getPrint 메소드가 반환하는 인스턴스가 다른 클래스의 인스턴스로 변경되어도 외부 코드는 전혀 수정할 필요가 없는 유연성을 갖추게 된다.

## 이너 클래스 - 2. 로컬 클래스
로컬 클래스는 멤버 클래스와 매우 유사하지만, 클래스의 정의 위치가 if문이나 while문 또는 메소드 몸체와 같은 블록 안에 정의된다는 점이 멤버 클래스와 구분된다.

```java
public Printable getPrint() {
    class Printer implements Printable {
        public void print() {
            System.out.println(str);
        }
    }
    return new Printer();
}
```

위의 멤버 클래스 예제와 똑같지만, 이 경우 getPrint메소드 안에 클래스를 정의하였다. 이런 경우 로컬 클래스가 되고, 메소드 내에서 클래스를 정의하면 메소드 내에서만 인스턴스 생성이 가능하기 때문에, private 선언은 의미가 없어지게 된다.

## 마무리
여기까지 네스티드 클래스와 이너 클래스 중 멤버 클래스와 로컬 클래스를 알아보았다.

익명 클래스의 경우 람다와 밀접한 관련이 있으므로, 다음 포스팅에서 같이 포스팅하도록 하겠다.

## 출처

__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__














