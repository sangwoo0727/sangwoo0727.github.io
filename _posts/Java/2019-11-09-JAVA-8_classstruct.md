---
title: "[Java] 클래스의 구성과 인스턴스화"
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
## 인스턴스 변수와 인스턴스 메소드
__[클래스란?](https://sangwoo0727.github.io/java/JAVA-7_classinstance/)__ 

위 포스팅에서 클래스 정의를 위해 구현하였던 코드를 다시 봐보자.

```java
class BankAccount{
  int balance = 0; // 인스턴스 변수

  // 인스턴스 메소드
  public int deposit(int amount){...}
  public int withdraw(int amount){...}
  public int checkMyBalance(){...}
}
```

클래스 내에 위치한 변수와 메소드를 가리켜 __인스턴스 변수__ 와 __인스턴스 메소드__ 라고 부른다.

인스턴스 변수 : 클래스 내에 선언된 변수

인스턴스 메소드 : 클래스 내에 정의된 메소드

인스턴스 변수와 지역 변수는 다른 개념!

인스턴스 변수가 선언되는 위치는 메소드 내부가 아니므로 지역 변수와 다르다.

__인스턴스 변수는 같은 클래스 내에 위치한 메소드 내에서 접근이 가능하다__

인스턴스 변수는 __멤버 변수__ 혹은 __필드__ 라고 불리기도 한다.

## '클래스를 정의하다'
클래스의 정의는 __틀을 구성하는 것__

붕어빵과 같이 무언가를 찍어내는 __틀__ 에 비유할 수 있다.

붕어빵 틀은 먹을 수 있는 대상이 아니지만, 빵을 찍어낼 수 있다.

이렇듯, 클래스가 정의되어 있다고 해서 그 안에 위치한 변수나 메소드를 사용할 수 있는 것은 아니다.

틀을 이용해서 __인스턴스__ 라는 것을 찍어내야 사용이 가능하다.

__인스턴스__ 는 __객체__ 라고도 한다.

__클래스의 인스턴스화__ : new BankAccount();

위의 문장을 실행하여 __인스턴스__ 를 만들면, BankAccount에 정의된 변수와 메소드를 담고 있는 인스턴스가 만들어진다.

다만, 위의 문장은 메모리상에 인스턴스를 만들기만 한것.

만들어진 인스턴스를 참조할 수 있는 __참조 변수__ 가 필요하다.

## 참조 변수 (Reference Variable)

참조 변수를 선언하고, 이를 통해서 새로 생성되는 인스턴스(객체)를 가리키게 할 수 있다.

```java
BankAccount myAcnt1; //참조변수 myAcnt1 선언
BankAccount myAcnt2; //참조변수 myAcnt2 선언

myAcnt1 = new BankAccount(); //참조변수 myAcnt1이 새로 생성되는 인스턴스를 가리킴
myAcnt2 = new BankAccount(); //참조변수 myAcnt2이 새로 생성되는 인스턴스를 가리킴
```


키워드 new를 통해서 인스턴스를 생성하면 __생성된 인스턴스의 주소값이 반환__ 된다.

즉, 참조 변수에는 생성된 인스턴스의 주소값이 저장된다. (이런 경우, 주소 값은 참조 변수에 저장된 값이므로 참조 값이라고도 한다)

__참조 변수는 인스턴스를 참조한다__

__참조 변수는 인스턴스를 가리킨다__

위의 두 문장으로 표현하는 것이 좋다.

## 참조 변수의 특성
변수는 저장된 값을 바꿀 수 있다.

참조 변수 역시 참조하는 인스턴스를 바꿀 수 있다.

또한 참조 변수가 지니고 있는 값을 다른 참조 변수에 대입하여 하나의 인스턴스를 둘 이상의 참조 변수가 동시에 참조하는 것도 가능.

```java
BankAccount refV = new BankAccount();
// ...
refv = new BankAccount(); // ref가 새로운 인스턴스를 참조한다.

// 하나의 인스턴스를 둘 이상의 참조변수가 동시에 참조 가능
BankAccount ref1 = new BankAccount();
BankAccount ref2 = ref1;
```

## 참조 변수의 매개변수 선언
메소드를 호출할 때 값을 전달할 수 있고, 이 값은 매개변수에 저장된다.

이처럼 메소드를 호출하면서 인스턴스의 참조 값을 전달하는 것도 가능.

```java
class BankAccount{
	int balance = 0;
	public int deposit(int amount) {
		balance += amount;
		return balance;
	}
	public int withdraw(int amount) {
		balance -= amount;
		return balance;
	}
	public int checkMyBalance() {
		System.out.println(balance);
		return balance;
	}
}
public class PassingRef {
	public static void main(String[] args) {
		BankAccount ref = new BankAccount();
		ref.deposit(3000);
		ref.withdraw(300);
		check(ref); // 참조값 전달
	}
	
	public static void check(BankAccount acc) {
		acc.checkMyBalance(); //acc가 참조하는 인스턴스의 메소드 호출
	}
}
```

check 메소드의 매개변수로 BankAccount의 참조변수가 선언되었다.

이 메소드는 인자로 인스턴스의 참조 값을 전달받는다.

메소드 내에서는 전달된 참조 값의 인스턴스를 대상으로 메소드를 호출할 수 있다.

## 참조변수에 null 대입
참조변수가 참조하는 인스턴스와의 관계를 끊고 아무런 인스턴스도 참조하지 않도록 하는 것.

BankAccount ref = new BankAccount();

ref = null;  //ref가 참조하는 인스턴스와의 관계를 끊는 것.

## 출처
__윤성우의 열혈 Java 프로그래밍, 오렌지 미디어__


