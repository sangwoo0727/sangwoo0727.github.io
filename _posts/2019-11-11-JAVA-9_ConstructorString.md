---
title: "[Java] 생성자(Constructor)"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
---

* 앞선 자바 포스팅들에서 정의한 클래스에는 문제점이 몇 가지 있다.
* __인스턴스를 생성하는 과정에서 적절한 초기화를 진행하지 못했다__ 는 것.

### 인스턴스를 구분할 수 있는 유일한 정보 필요!
* 앞의 포스팅에서 정의한 BankAccount 클래스에는 누구의 계좌인지 구분하기 위한 정보가 없다.
* 계좌번호, 주민번호를 통해 누구의 계좌인지를 구분해보자.

```java
class BankAccount{
	String accNumber;
	String ssNumber;
	int balance = 0;
	
	public void initAccount(String acc, String ss, int bal) { //계좌 개설 시 초기화
		accNumber = acc;
		ssNumber = ss;
		balance = bal;
	}
	public int deposit(int amount) {
		balance += amount;
		return balance;
	}
	public int withdraw(int amount) {
		balance -= amount;
		return balance;
	}
	public int checkMyBalance() {
		System.out.println(balance + " " + accNumber +" "+ ssNumber);
		return balance;
	}
}
public class BankAccountUniID {
	public static void main(String[] args) {
		BankAccount kang = new BankAccount(); //계좌 생성
		kang.initAccount("12-34-56","9999999-1234567",1000);
		kang.deposit(3000);
		kang.withdraw(1000);
		kang.checkMyBalance();
		}
}
```

* 클래스에 계좌 개설 시 초기화하는 메소드가 추가되었다.
* 이 메소드는 인스턴스 초기화를 위한 메소드.
* 인스턴스 생성 시 반드시 한번 호출해서 초기화를 해야한다.
* __하지만!__ 위와 같이 메소드를 정의하지 않고 __생성자(Constructor)__ 을 정의하여 __인스턴스의 초기화를 진행할 수 있다__.
* 생성자는 __인스턴스 생성 과정에서 초기화를 위해 자동으로 호출되는 일종의 메소드__.

### 생성자
* 생성자의 모습은 __메소드의 모습과 같다.__
* 하지만 생성자는 메소드와 약간의 차이가 있다.
* __생성자의 이름은 클래스의 이름과 동일해야 한다.__
* __생성자는 값을 반환하지도 않고, 반환형을 표기하지도 않는다.__
* 위의 코드에서 인스턴스의 초기화를 위한 메소드가 생성자가 되려면 아래와 같아지면 된다.

```java
public BankAccount(String acc, String ss, int bal){ //생성자의 이름은 클래스 이름과 동일! , 반환형 선언 없음!
    accNumber = acc;
    ssNumber = ss;
    balance = bal;
}
```

* 생성자를 만들고 나면 인스턴스의 생성 문장을 아래와 같이 변경할 수 있다.
* __BankAccount kang = new BankAccount("12-34-56","999999-1234567",1000);__
* 소괄호 안의 값들은 생성자가 호출될 때 __생성자의 매개변수로 전달__ 된다.
* 위와 같은 생성자와 호출문을 통하여 __생성자가 호출되었음__ 과 __원하는 값으로 인스턴스가 초기화되었음__ 을 알 수 있다.
* 즉, __인스턴스의 생성의 마지막 단계는 생성자 호출이다.__ , __어떤 이유로든 생성자 호출이 생략된 인스턴스는 인스턴스가 아니다__.

### 디폴트 생성자
* 위의 마지막 문장을 보면 __어떤 이유로든 생성자 호출이 생략된 인스턴스는 인스턴스가 아니다__ 라고 하였다.
* 하지만, 이전 포스팅을 보면 우리는 생성자가 없는 클래스를 정의하였고, 이를 통해 인스턴스를 생성했는데? 라고 생각할 수 있다.
* 정확히 말하면, 생성자를 생략한 상태의 클래스를 정의하면 자바 컴파일러가 __디폴트 생성자__ 라는 것을 클래스의 정의에 넣게 된다.
* 디폴트 생성자가 정의된다고 하더라도 __생성자는 직접 정의해주는 것이 좋다!!!__