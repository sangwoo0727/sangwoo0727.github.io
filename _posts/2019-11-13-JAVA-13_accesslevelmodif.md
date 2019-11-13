---
title: "[Java] 접근 수준 지시자(Access-level Modifiers)"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
---

* [이전 포스팅](https://sangwoo0727.github.io/java/JAVA-12_informationhiding/) 에서 인스턴스 변수를 대상으로 private 선언을 하였다.
* 이러한 유형의 키워드를 가리켜 __접근 수준 지시자(Access-level Modifiers)__ 라고 한다.
* __접근의 허용 수준을 결정할 때__ 사용하는 키워드.

### <span style="color:#34495e">네 가지 종류의 접근 수준 지시자</span> 
* __public, protected, private, default__ , 네 종류의 Access-level Modifiers가 존재한다.
* __default__ 는 키워드가 아닌, __아무런 선언도 하지 않은 상황__ 을 의미한다.
* Access-level Modifiers를 선언할 수 있는 대상은 아래의 두가지이다.
* 1. __클래스의 정의__
* 2. __클래스의 인스턴스 변수와 메소드__

### 클래스의 정의 대상의 Access-level Modifiers 선언이 갖는 의미
* 클래스의 정의 대상은 4가지의 접근 수준 지시자 중, __public과 default__ 선언이 가능하다.
* 먼저 public으로 선언된 클래스는 __위치에 상관없이 어디서든 해당 클래스의 인스턴스를 생성할 수 있다.__
* default로 선언된 클래스는 __동일 패키지로 묶인 클래스 내에서만 인스턴스 생성이 가능하다.__
* 즉 클래스 정의에 대한 __public__ 과 __default__ 선언이 갖는 의미는 다음과 같다.
* public : __어디서든 인스턴스 생성이 가능하다__.
* default : __동일 패키지로 묶인 클래스 내에서만 인스턴스 생성을 허용한다__.
* 아래 예시 코드를 통해서 이해해보겠다.

```java
// Cat.java
package zoo;

class Duck{ 
    //Duck은 default로 선언되었으므로 동일한 패키지 내에서만 인스턴스 생성 가능
}

public class Cat{ //Cat은 public으로 선언 -> 어디서든 인스턴스 생성 가능
    public void makeCat(){
        Duck quack = new Duck();
        //Duck과 같은 패키지로 묶여 있으니 Duck 인스턴스 생성 가능
    }
}
```

```java
//Dog.java

package animal;

public class Dog{
    public void makeCat(){
        //Cat은 public으로 선언되었으므로 어디서든 인스턴스 생성 가능
        zoo.Cat yaong = new zoo.Cat();
    }
    public void makeDuck{
        //Duck은 default로 선언되었으므로 이 위치에서는 인스턴스 생성 불가
        zoo.Duck quack = new zoo.Duck(); //컴파일 오류 발생
    }
}
```

* 위의 글에 대한 설명은 코드를 통하여 이해해보면 쉽게 이해할 수 있을 것이다.
* 다만, 클래스의 public 선언과 관련하여 다음 두 가지 사항을 지켜야 한다.
* 1. __하나의 소스파일에 하나의 클래스만 public으로 선언한다.__
* 2. __소스파일의 이름과 public으로 선언된 클래스의 이름을 일치시킨다.__

### 인스턴스 멤버 대상의 Access-level Modifiers 선언
* 인스턴스 멤버(변수,메소드)는 __public, protected, private, default__ 중 하나로 선언된다.
* 인스턴스 멤버 대상의 public과 default 선언이 갖는 의미는 아래와 같다.
* __public : 어디서든 접근이 가능하다.__
* __default : 동일 패키지로 묶인 클래스 내에서만 접근이 가능하다.__
* 위에서 말하는 '접근'은 변수의 경우 말 그대로 접근이 되지만, 메소드의 경우'__호출__'을 의미한다.
* 간단한 코드를 보며 이해해보자

```java
//Cat.java
package zoo;

public class Cat{
    //public으로 선언된 클래스는 어디서든 인스턴스 생성 가능
    public void makeSound(){
        //public으로 선언된 메소드는 어디서든 호출 가능
        System.out.println("야옹!");
    }
    void makeHappy(){
        //default로 선언된 메소드
        //동일 패키지로 묶인 클래스 내에서 호출 가능
        System.out.println("Smile!");
    }
}
```

```java
//Dog.java
package animal;

public class Dog{
    public void welcom(zoo.Cat c){
        c.makeSound(); //호출 가능
        c.makeHappy(); //호출 불가, 컴파일 에러
    }
}
```

* makeHappy() 메소드는 default로 선언되었기때문에, 동일 패키지가 아닌 클래스에서는 호출이 불가능하다.
* 오류없이 컴파일을 완료할 수 있는 방법에 대해 생각해보자.
* 1. Dog.java의 패키지를 zoo로 수정한다.
* 2. makeHappy 메소드를 public으로 선언한다.

### 인스턴스 멤버의 private 선언이 갖는 의미
* __[정보 은닉 포스팅](https://sangwoo0727.github.io/java/JAVA-12_informationhiding/)__ 에서 private 선언의 기능과 의미에 대해 알아보았다.
* 클래스에 private로 선언된 인스턴스 변수는 클래스 내부에서만 접근이 허용되었다.
* 마찬가지로, private로 선언된 메소드 역시 클래스 내부에서만 호출이 가능하다.
* 아래 코드를 보자

```java
class Duck{
    private int numLeg = 2; // 클래스 내부에서만 접근 가능
    public void md1(){
        System.out.println(numLeg); //접근 가능
        md2(); //호출 가능
    }
    private void md2(){
        System.out.println(numLeg); //접근 가능
    }
    void md3(){
        System.out.println(nulLeg); //접근 가능
        md2(); //호출 가능
    }
}
```

### 인스턴스 멤버의 protected 선언이 갖는 의미
* __상속__ 을 학습한 후에 protected를 다시 공부하면 더욱 이해가 잘 될 것.
* __protected 선언은 default 선언이 허용하는 접근을 모두 허용한다__.
* 더불어 __protected는 default가 허용하지 않는 '하나의 영역'에서의 접근도 허용한다__
* 여기서 말하는 '하나의 영역'이 __클래스의 상속 관계__ 에서 만들어진다.
* 클래스의 상속 관계에 대한 포스팅을 한 후에 다시 protected에 대한 설명을 자세히 하겠다.
* 상속 관계에 있는 두 개의 클래스를 간단한 예제로 봐보자
  
```java
//AAA.java

package alpha;

public class AAA{
    int num;
}
```

```java
//ZZZ.java
//ZZZ 클래스는 디폴트 패키지에 묶여있고, AAA 클래스는 alpha 패키지에 묶여있다.
public class ZZZ extends alpha.AAA{
    public void init(int n){
        num = n; //상속된 변수 num의 접근! -> 컴파일 에러
    }
}
```

* 위의 경우, AAA 클래스에 있는 인스턴스 변수 num은 default로 선언되어있다.
* 즉, 동일 패키지로 묶여있는 클래스 내에서만 접근이 가능하다.
* 이는 상속을 하였어도 마찬가지이다.
* 그렇기 때문에 num=n; 문장은 컴파일 에러가 발생하게 된다.
* 이때 AAA 클래스의 인스턴스 변수 num을 protected로 선언하면 정상적으로 컴파일 가능.

```java
//AAA.java

package alpha;

public class AAA{
    protected int num; //상속 관계에 있는 클래스에서 접근 가능
```
* 위에서 말한 default는 허용하지 않지만 protected는 허용하는 '하나의 영역'이 어디인지 확인할 수 있게 되었다.
* __'protected로 선언된 멤버는 상속 관계에 있는 다른 클래스에서 접근이 가능하다'__
* 이러한 접근은 상속관계에 있는 두 클래스가 서로 다른 패키지로 묶여 있어도 가능하다.

### 인스턴스 멤버를 대상으로 하는 public, protected, private, default에 대한 정리
* 네 개의 Access-level Modifiers에 대해 허용하는 접근의 수준을 정리해보자

| <center>지시자</center> | <center>클래스 내부</center> | <center>동일 패키지</center> | <center>상속 받은 클래스</center> | <center>이외의 영역</center>|
| <center>private</center> | <center>O</center> | <center>X</center> | <center>X</center> | <center>X</center> |
| <center>default</center> | <center>O</center> | <center>O</center> | <center>X</center> | <center>X</center> |
| <center>protected</center> | <center>O</center> | <center>O</center> | <center>O</center> | <center>X</center> |
| <center>public</center> | <center>O</center> | <center>O</center> | <center>O</center> | <center>O</center> |

* 여기서 말하는 이외의 영역은 다른 패키지에 속한 클래스를 뜻한다.

#### 출처 
* 윤성우의 열혈 Java 프로그래밍, 오렌지 미디어