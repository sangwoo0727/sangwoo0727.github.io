---
title: "[OSI 참조모델과 TCP/IP 기초] UDP"
categories:
  - Network
tags:
  - Network
comments:
  - true
---
## [OSI 참조모델과 TCP/IP 기초] UDP
* User Datagram Protocol
* OSI 참조 모델의 제 4계층인 전송 계층에 위치하는 네트워크 프로토콜
* 커네션리스형(데이터그램형) 통신을 지원한다.
* 커네션리스형이란 정보를 보낸다는 것을 상대편에게 통지하지 않고 갑자기 송신해버리는 방법.
* 통신의 신뢰성은 낮아지지만 TCP와는 달리 프로토콜 자체의 처리가 가볍게 끝나 고속으로 통신할 수 있다.
* 제 3계층9(네트워크 계층)의 IP를 제 5계층(세션 계층)이상의 프로토콜에서 직접 사용할 수 있도록 하기 위한 다리 역할.
* 상위 계층의 애플리케이션으로부터 받은 데이터를 패킷으로 분할하여 IP를 사용하여 송출하기만 할 뿐, TCP처럼 수신 확인을 하는 일은 하지 않는다.
* 송신 측에서는 결국 패킷이 도착했는지 알지 못하며, 실제로 전달되지 않는 일도 일어날 수 있다.
* 처리가 가벼워 주로 작은 크기의 패킷을 주고 받는 애플리케이션이나 시간적 연속성이 중요한 애플리케이션에서 이용하는 프로토콜.
* DNS,DHCP나 음성 통화 동영상 배포 등에 해당됨.

![](/assets/img/Network/1910316.jpg)

#### 출처 : 그림 한 장으로 보는 최신 네트워크 용어 해설 , Ryeji Kitami , 정보문화사