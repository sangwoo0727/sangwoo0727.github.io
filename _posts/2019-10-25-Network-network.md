---
title: "[네트워크 프로그래밍] TCP/IP 4계층"
categories:
  - Network
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
## [네트워크 프로그래밍] TCP/IP 4계층
* 네트워크를 통한 전송은 데이터의 논리적인 특성뿐만 아니라 네트워크의 물리적 특성까지 고려하여 주의 깊게 다뤄야 하는 복잡한 작업.
* 애플리케이션 개발자나 소프트웨어 사용자들에게 이러한 복잡한 과정을 노출시키지 않기 위해, 네트워크 통신의 여러가지 기능을 몇 개의 계층으로 분리하였다.
* 이론적으로 각 계층은 인접한 위아래 계층하고만 대화한다.


### '개방형 시스템간 상호 접속(OSI)' 참조 모델 - 7계층
* 흔히 말하는 OSI 7계층은 자바 네트워크 프로그램에서 다루기에는 불필요하게 세분화되어 있다.
* OSI 모델과, TCP/IP 4계층의 가장 큰 차이점은 OSI 모델이 호스트-투-네트워크 계층을 데이터 링크 층과 물리 층으로 나누며, 애플리케이션 계층과 전송 계층 사이에 표현 계층과 세션 계층을 두고 있다는 점.
* OSI 모델이 복잡하긴 하지만, TCP/IP 모델에 비해 조금 더 일반적이며, 비 TCP/IP 네트워크 환경에 적합한 구성.
* 자바 네트워크 클래스는 TCP/IP 네트워크에서 애플리케이션 계층과 전송 계층에서만 동작한다.
* 그렇기에 TCP/IP 4계층에 대해 다뤄보려 한다.


### TCP/IP 4계층

![](/assets/img/Network/1910261.jpg)

### 1. 호스트-투-네트워크 계층
* 자바 프로그래머 레이더 아래에는 실제 많은 일이 일어나며, IP 기반 인터넷(자바가 실제로 이해하는 유일한 네트워크 종류) 표준 참조 모델에서 네트워크의 잘 알려지지 않은 부분은 호스트-투-네트워크계층에 포함되어 있다.
* 호스트-투-네트워크 계층은 이더넷 카드나 와이파이 안테나 같은 특정 네트워크 인터페이스가 물리적인 연결을 통해 로컬 네트워크 및 외부로 IP데이터그램을 보내는 방법을 정의한다.
* 호스트-투-네트워크 계층의 일부는 회선,광케이블,전파,연기 신호 같은 다른 컴퓨터를 서로 연결하는 하드웨어로 구성되어 있으며, 때로는 물리 계층이라고 불린다.
* 자바는 물리 계층을 다룰 수 없다.
* 자바 프로그래머가 물리 계층을 생각해 볼 이유나 필요가 생긴다면 그건 성능 때문일 것.
* 어떤 물리적인 연결과 마주하더라도 네트워크 통신을 위해 사용하는 API는 동일하다. 이것을 가능하게 하는 것이 인터넷 계층.

### 2. 인터넷 계층
* OSI 모델에서 인터넷 계층은 네트워크계층으로 불린다.
* 네트워크 계층 프로토콜은 데이터의 비트와 바이트를 좀 더 큰 그룹인 패킷으로 구성하는 방법과 다른 컴퓨터가 서로 찾을 수 있도록 주소 체계를 정의한다.
* 인터넷 프로토콜(IP)은 세상에서 가장 널리 사용되는 네트워크 계층 프로토콜이며, 자바가 이해할 수 있는 유일한 네트워크 계층 프로토콜.
* IP에는 32비트 주소를 사용하는 IPv4와 128비트 주소와 패킷의 경로 탐색을 돕기 위한 몇 가지 기술적인 요소들을 포함하고 있는 IPv6가 있다.
* 이 두개의 프로토콜은 같은 네트워크 안에서 특별한 게이트웨이나 터널링 프로토콜 없이는 상호운용될 수 없는 매우 다른 네트워크 프로토콜이지만 자바는 이 프로토콜의 차이를 사용자에게 노출시키지 않으며 투명하게 사용할 수 있다.
* IPv4와 IPv6 모두에서 데이터는 데이터그램이라는 패킷으로 인터넷 계층을 통해 보내진다. IPv4 데이터그램은 20바이트에서 60바이트 길이의 헤더와 최대 65,515바이트의 페이로드를 포함하고 있다.

![](/assets/img/Network/1910262.jpg)

* 위 그림은 데이터그램의 각 요소들의 정렬 방법을 나타낸다. 모든 비트와 바이트는 빅 엔디안으로 정렬된다.
* 라우팅 및 주소 지정 외에, 인터넷 계층의 두 번째 목적은 다른 종류의 호스트 투 네트워크 계층이 서로 통신할 수 있게 하는것.
* 인터넷 계층은 같은 종류의 프로토콜을 사용하여 이기종 네트워크를 서로 연결할 책임이 있다.

### 3. 전송 계층
* 데이터그램은 전송이 보장되지 않으며, 목적지까지 데이터그램이 전송되더라도 전송 중에 손상될 가능성이 있다는 단점이 있다.
* 체크섬은 헤더의 손상만 발견할 수 있을 뿐, 데이터그램의 데이터 부분에 대한 손상은 발견할 수 없다. 
* 또한 데이터그램이 손상없이 목적지에 도착하더라도 반드시 전송한 순서대로 도착하지 않는다.(각각의 데이터그램은 출발지에서 목적지까지 서로 다른 경로로 전달된다)
* 전송 계층은 패킷이 손실되거나 손상되지 않고 보낸 순서대로 수신되도록 보장할 책임이 있다. 패킷이 손실된다면, 전송 계층은 해당 패킷에 대해 송신자에게 재전송을 요청할 수 있다.
* IP 네트워크는 이러한 기능을 각각의 데이터그램에 몇몇 정보를 포함하고 있는 부가적인 헤더를 추가하여 구현.
* 즉, 전송계층에는 전송 제어 프로토콜(TCP, Transmission Control Protocol 손실되거나 손상된 데이터의 재전송과 데이터의 순서를 보장하는 오버헤드가 높은 프로토콜)과 사용자 데이터그램 프로토콜(UDP, User Datagram Protocol 수신자가 패킷의 손상을 감지할 수는 있지만 수신된 패킷의 순서는 보장하지 않는다.) 두 가지 주요 프로토콜이 있다.
* UDP는 TCP보다 빠른 성능을 보여주기도 하며, TCP는 신뢰할 수 있는 프로토콜, UDP는 신뢰할 수 없는 프로토콜.

### 4. 애플리케이션 계층
* 사용자에게 데이터가 전달되는 계층.
* 애플리케이션 계층 아래에 놓인 세 계층은 하나의 컴퓨터에서 다른 컴퓨터로 데이터가 전송되는 방법을 정의하기 위해 모두 함께 동작하며, 애플리케이션 계층은 데이터가 전송된 후 해당 데이터로 어떤 일을 할지 결정한다.
* 매우 많은 종류의 애플리케이션 프로토콜이 존재하며, HTTP 이외에도 SMTP, POP, IMAP, FTP, FSP 등등 많은 프로토콜이 있다.
* 필요한 경우 스스로 애플리케이션 계층 프로토콜을 정의하여 사용할 수도 있다.


#### 출처 : 자바 네트워크 프로그래밍 , O'REILLY, 제이펍