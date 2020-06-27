---
title: "[Java] 익명 클래스(Anonymous Class)"
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
이번 포스팅에서는 익명 클래스에 대해 포스팅하려 한다.

익명 클래스는 이전 포스팅에서 소개한 Non-static 네스티드 클래스인 이너 클래스 중에 하나이다.

이너클래스에는 멤버 클래스, 로컬 클래스, 익명 클래스가 있는데 멤버 클래스와 로컬 클래스는 이전 포스팅에서 다뤘고, 이번 포스팅은 람다와도 관련이 있는 익명 클래스에 대해 다룰 것이다.

## 익명 클래스란?
말그대로 익명 클래스는 이름이 없는 클래스이다. 왜 이름이 없는 지가 중요하다.

클래스에 이름이 존재한다는 것은 다른 의미로 다음번에 이름을 통해 이 클래스를 다시 재사용할 수 있다는 의미이기도 하다.

만약, 이 클래스를 통해 딱 한번 객체를 만들고 재사용하지 않을 것 같은 클래스인 경우 굳이 클래스를 정의해놔야 싶을 때 우리는 익명 클래스를 사용할 수 있다.

이전 포스팅에서 다뤘던 예제를 잠시 보자.

```java
interface Printable{
    void print();
}
class Papers{
    private String con;
    public Papers(String s){
        con = s;
    }
    public Printable getPrinter(){
        class Printer implements Printable{
            public void print(){
                System.out.println(con);
            }
        }
        return new Printer();
    }
}
```

우리는 getPrinter() 메소드를 통해 Printable 인터페이스를 구현하는 정의된 Printer 클래스를 통해 Printer 인스턴스를 받았다.

그런데 가만보면, Printer 클래스의 이름은 정의할 때 딱 한번 사용되고 사용하지 않는다.

또한, Printer 인스턴스를 딱 한번 만든다고 할 때 이렇게 클래스를 굳이 정의해놔야 싶을 수 있다.

이때 익명 클래스를 통해 더욱 깔끔하게 코드를 작성할 수 있게 된다.

```java
public Printable getPrinter(){
    return new Printable() {
        @Override
        public void print() {
            System.out.println(con);
        }
    };
}
```

익명클래스 작성법을 통해 클래스와 인스턴스의 생성을 하나로 묶어버리는 것이다.

이 코드를 분석해보면, getPrinter() 메소드는 Printable 인터페이스를 구현한 클래스의 인스턴스를 반환해야 한다.

new Printable(); 이라고만 쓰면 인터페이스 Printable을 대상으로 인스턴스를 생성하므로, 문제가 된다.

그러므로 그 뒤에 Printable 인터페이스를 구현하는 클래스의 정의를 덧붙혀 인스턴스를 생성하는 것이다.

그리고 우리는 new Printable() 뒤에 등장하는 이름 없는 클래스의 정의를 가리켜 __익명 클래스__ 라고 한다.

또 다른 예를 봐보자.

```java
public class Practice {
    public static void main(String[] args) {
        List<Pair> list = new ArrayList<>();
        list.add(new Pair(2,3));
        list.add(new Pair(2,1));
        list.add(new Pair(1,10));
        Collections.sort(list, new Comparator<Pair>() { //오름 차순 정렬
            @Override
            public int compare(Pair o1, Pair o2) {
                if(o1.x==o2.x) return o1.y-o2.y;
                else return o1.x-o2.x;
            }
        });
        for(Pair p: list){
            System.out.println(p.x+ " "+p.y);
        }
        Collections.sort(list, new Comparator<Pair>() { //내림 차순 정렬
            @Override
            public int compare(Pair o1, Pair o2) {
                if(o1.x==o2.x) return o2.y - o1.y;
                else return o2.x - o1.x;
            }
        });
        for(Pair p: list){
            System.out.println(p.x + " "+p.y);
        }
    }
    static class Pair{
        int x, y;
        public Pair(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
}
```

위의 코드처럼 list 배열을 오름차순으로 정렬한 결과와 내림차순으로 정렬한 결과를 출력하는 과정이 있다고 해보자.

이 때, 익명 클래스를 사용하지 않는다고 하면, 우리는 내림차순 정렬을 정의한 클래스, 오름차순 정렬을 정의한 클래스를 만들어야 한다.

하지만, 여러번 사용하는 것이 아니라면, 이렇게 익명 클래스를 통해 구현하는 것이 훨씬 간편할 것이다.

그리고 위의 코드는 이후에 포스팅할 람다가 등장하기 이전에 주로 사용하던 코드 스타일이다.

## 출처

__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__
