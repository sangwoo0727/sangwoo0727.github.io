---
title: "[Java] 클래스란?"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
---

### 클래스란?
* 클래스의 일반적 정의는 __동일한 속성과 행위를 수행하는 객체의 집합__
* 프로그래밍적으로 표현하면 __어떤 객체의 변수__ 와 __메소드__ 의 집합이다.
* 클래스(Class) = 데이터(Data) + 메소드(Method)
* 데이터란, __프로그램상에서 유지하고 관리해야 할 데이터__
* 기능은 __데이터를 처리하고 조작하는 기능__
* 데이터는 __변수의 선언__ 을 통해 유지 및 관리가 된다.
* 변수에 저장된 데이터는 __메소드의 호출__ 을 통해 처리가 된다.

### 예시를 통한 클래스 설명
* 학교에는 학생과 교수가 존재.
* 학생과 교수는 동일한 일을 하지 않기 때문에, __동일한 일을 수행하는 집합으로 표현 불가__.
* 즉, 학생과 교수는 __서로 다른 클래스__
* 학생은 여러 학생이 있다.
* 하지만 학생은 학번, 이름, 성별, 수강 과목이라는 동일한 속성을 가지고, 학교에 속학 학생은 모두 과목수강이라는 행위를 하게됨.
* 그렇기에 개개인의 학생을 모두 하나의 학생이라는 객체의 집합으로 묶을 수 있다.

![](/assets/img/java/class_instance_201911081.png)

* 위의 그림처럼 학생 클래스이며, 학생이 가지고 있는 속성(변수)를 가지고 있고, 맨 아래에는 학생이 하는 행위(메소드)가 존재한다.
* 아래의 간단한 은행계좌 코드를 통해 구체적으로 살펴보겠다.

```java
public class BankAccountPO {
	static int balance = 0;
	public static void main(String[] args) {
		deposit(10000); // 예금 진행 
		checkMyBalance(); //잔액 확인
		withdraw(3000); // 출금 진행
		checkMyBalance();
	}
	
	public static int deposit(int amount) { //입금 담당 메소드
		balance += amount;
		return balance;
	}
	public static int withdraw(int amount) { //출금 담당 메소드
		balance -= amount;
		return balance;
	}
	public static int checkMyBalance() { //예금 조회 메소드
		System.out.println("잔액 : " + balance);
		return balance;
	}
}
```

* 변수 balance 는 프로그램 상의 __데이터__ 이다.
* 메소드 deposit, withdraw, checkMyBalance 는 프로그램 상의 __기능__ 즉 메소드이다.
* 클래스의 구조인 변수 + 메소드의 구조로 되어 있다.
* 그리고 이 변수와 메소드들은 서로 뗄 수 없는 관계이다.
* 즉, __연관 있는 변수와 메소드를 묶기 위해 클래스가 존재하는 것__ 이다.

### 이어서 공부한 내용은 포스팅이 길어져서 새로운 포스팅에 작성하겠습니다.

#### 참고
* 윤성우의 열혈 Java 프로그래밍, 오렌지 미디어
* [[JAVA객체지향디자인패턴] 클래스(Class) 란 무엇인가?](https://javacpro.tistory.com/29)
