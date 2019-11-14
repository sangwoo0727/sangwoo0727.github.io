---
title: "[Java] 캡슐화(Encapsulation)"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
---

### <span style="color:#34495e">들어가기 전</span>
* 캡슐화는 문법적인 내용이 아니다!
* 프로그램의 퀄리티를 올리기 위한 이론적인 내용!


### <span style="color:#34495e">캡슐화란?</span>
* 캡슐화는 __하나의 목적을 이루기 위해 관련 있는 모든 것을 하나의 캡슐에 담아 두는 것__.
* 객체지향 관점에서 캡슐은 __클래스__ 에 해당.
* 즉, __하나의 목적을 이루기 위해 관련 있는 모든 것을 하나의 클래스에 담아 두는 것__.
* 클래스들을 적절히 캡슐화시키면 __프로그램이 간결해진다__.
* 아래 코드는 캡슐화가 잘 이뤄진 일상에서의 사례를 프로그래밍한 코드이다.

```java
class SinusCap{
	void sniTake() {
		System.out.println("콧물엔 이거");
	}
	void sneTake() {
		System.out.println("재채기엔 이거");
	}
	void snuTake() {
		System.out.println("코막힘엔 이거");
	}
	void take() {
		sniTake();
		sneTake();
		snuTake();
	}
}

class ColdPatient{
	void takeSinus(SinusCap cap) {
		cap.take();
	}
}

class OneClassEncapsulation {
	public static void main(String[] args) {
		ColdPatient suf = new ColdPatient();
		
	}
}
```

* 이 코드를 해석해보자면 아래와 같다.
* 감기 걸린 환자가 약을 복용한다.
* 약을 먹으면, 약 안에 콧물,재채기,코막힘 부분 약이 순서대로 복용된다.
* 만약 캡슐화를 시키지않으면 콧물약,재채기약,코막힘약을 따로따로 복용해야 할 것이다.
* 또한 약 먹는 순서가 정해져있다고 할 때 어려움이 있을 것이다.
* 즉, __감기 치료라는 동일한 목적을 위해 하나의 캡슐에 여러 약을 담아 한번에 복용할 수 있게 된다__.
* 캡슐화는 __절대로 클래스를 크게 만들라는 것이 아니다.__
* 캡슐화는 클래스의 크기가 목적이 아닌, __관련있는 내용을 하나의 클래스에 논리적으로 정확하게 담는 것이다__.
* cf) 하나의 클래스가 다른 클래스의 인스턴스를 멤버로 가질 수 있는데 이러한 관계를 가리켜 __포함관계__ 라고 한다.


#### 출처 
* 윤성우의 열혈 Java 프로그래밍, 오렌지 미디어
