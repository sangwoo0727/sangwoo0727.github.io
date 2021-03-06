---
title: "[OSI 참조모델과 TCP/IP 기초] TCP/IP"
categories:
  - Network
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
## [OSI 참조모델과 TCP/IP 기초] TCP/IP
* TCP/IP는 인터넷에서 표준으로 사용되고 있는 네트워크 프로토콜로, OSI 참조 모델의 3계층(네트워크 계층)의 IP를 중심으로 한 여러 프로토콜의 집합체를 총칭하여 부르는 것.
* 주로 제 4계층의(전송계층)의 TCP와 조합하여 구성되며, 인터넷 상의 대표적인 서비스인 HTTP등은 이 프로토콜을 기반으로 동작한다.

![](/assets/img/Network/1910311.jpg)

* 통신 상에서 주고받는 데이터는 패킷이라는 단위로 나눠져 각각에 보낼 주소(상대방의 IP주소)가 붙어있다.

### IP
* OSI 참조모델에서 제 3계층인 네트워크 계층에 위치하는 네트워크 프로토콜.
* 네트워크 상에 있는 각 기계의 주소 할당이나 그 주소를 가지고 패킷을 전송하는 역할을 한다.
* 기기의 주소를 정하고 거기까지 데이터를 전달하기 위한 프로토콜.
* IP는 송신 데이터가 되는 패킷을 TCP나 UDP와 같은 상위 계층으로부터 전달 받으면, IP 헤더 정보를 추가하여 네트워크에 보낸다.
* IP 헤더란, 송신자와 수신자의 IP주소를 중심으로 한 정보의 집합. 네트워크 상을 흐르는 패킷은 이 헤더를 바탕으로 올바른 수신자에게 전달.
* IP에는 경로를 선택하는 방법에 대해서도 정의되어 있어서 여러 개의 네트워크를 걸친 통신도 가능.
* 실제로, LAN과 외부 네트워크를 연결하는 기기인 라우터가 IP 경로 선택(라우팅)을 지원하고 있으며, 라우터로부터 수신자가 속한 네트워크의 라우터로 패킷이 송출됨으로써 목적지에 도착할 수 있게 된다.

![](/assets/img/Network/1910312.jpg)

### TCP
* OSI 참조 모델의 제 4계층인 전송 계층에 위치하는 네트워크 프로토콜.
* 신뢰성이 높고 확실한 데이터 통신을 보증.
* 신뢰성이 높다는 것은 데이터의 결손이 없으며 상대방에게 확실히 전달 되는 것을 의미.
 
![](/assets/img/Network/1910313.jpg)


* TCP에서는 5계층 세션계층 이상의 프로토콜로부터 데이터를 받아 패킷으로 분할한다. 그 후 패킷을 IP에게 전달한다.
* 패킷이 송출된 순서대로 전달되면 좋지만, 실제로 패킷 송신을 수행하는 IP는 이러한 보증을 하지 않는다.
* 즉, 네트워크의 혼잡 상황에 따라 패킷의 결손, 지연에 의해 순서가 바뀌는 일 등이 일어날 수 있음.
* TCP는 이러한 점을 방지하고자 몇 가지 방법을 사용하여 데이터 통신에 신뢰성을 확보하고 있다.
 1. 통신 데이터를 패킷으로 분할할 때 시퀀스 번호를 붙여 두고, 수신 측에서 이 번호를 체크하고 정렬하여 패킷의 순서가 올바르게 되도록 보증함.
 2. 수신 측에서는 수신했다는 것을 나타내는 통지패킷(ACK 패킷)을 송신 측에 돌려보냄.
 3. 송신 측은 송출한 패킷이 전달되었는지 아닌지 판단할 수 있으며, 일정 시간을 기다려도 답이 오지 않는 경우에는 패킷을 재송출시켜 결손을 막음.

![](/assets/img/Network/1910314.jpg)

* TCP 프로토콜의 경우 수신 확인이나 패킷의 재전송과 같은 절차 때문에 상당히 무겁다.
* 반면, UDP 프로토콜의 경우 신뢰성보다 가볍고 빠른 처리를 중시하는 프로토콜.

![](/assets/img/Network/1910315.jpg)


#### 출처 : 그림 한 장으로 보는 최신 네트워크 용어 해설 , Ryeji Kitami , 정보문화사