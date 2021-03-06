---
title: "[네트워크 프로그래밍] 클라이언트/서버 모델"
categories:
  - Network
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
## [네트워크 프로그래밍] 클라이언트/서버 모델
* 클라이언트/서버 애플리케이션은 대부분의 프로그램 로직과 사용자 인터페이스가 서버에 비해 상대적으로 저렴한 개인용 컴퓨터에서 실행되는 클라이언트 소프트웨어에서 처리된다.
* 반면, 많은 양의 데이터는 일반적으로 비싼 고성능 서버나 클라우드 서버에 저장한다.
* 대부분의 경우에 서버는 주로 데이터를 보내고, 클라이언트는 주로 서버가 보낸 데이터를 받는다. 그러나, 프로그램이 오직 데이터를 보내기만 하거나 받기만 하는 경우는 거의 없다.
* 명확한 차이점은 서버는 클라이언트와 대화를 시작하기 위해 클라이언트의 요청을 기다리는 반면, 클라이언트는 먼저 대화를 요청한다.
* 간단한 웹 동작을 통해 클라이언트/서버 모델을 이해해보겠다.
1. 아파치 같은 웹 서버들은 파이어폭스 같은 웹 클라이언트가 보낸 요청에 응답한다.
2. 데이터는 웹 서버에 저장되어 있다가 요청이 들어오면 클라이언트로 보내진다.
3. 페이지에 대한 초기 요청 패킷을 제외하고는 거의 모든 데이터가 서버에서 클라이언트로 전송되며, 클라이언트로부터 서버로 전송되는 데이터는 거의 없다.

* 모든 애플리케이션들이 서버/클라이언트 모델에 적합한 것은 아니다. 게임에서는 두 게이머가 서로 연결되어 데이터를 주고받는다. 이러한 연결 종류를 P2P(peer to peer)라 한다.
* 자바 자체의 네트워킹 API에서는 명시적으로 P2P 통신을 위한 기능을 제공하지 않는다. 
* 하지만 일반적인 방법으로 애플리케이션이 서버와 클라이언트 역할을 모두 수행하는 방법으로 P2P 통신 기능을 제공할 수 있다.
* 또 하나의 방법으로는 피어간에 데이터를 전달해주는 중개 서버 프로그램을 이용해 통신하는 방법이 있다.

