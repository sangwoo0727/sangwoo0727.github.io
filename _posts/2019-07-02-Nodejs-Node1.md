---
title: "[Node.js] 노드 시작하기 1"
categories:
  - Node.js
tags:
  - Node.js
  - javascript
  - server
comments:
  - true
---
## [Node.js] 노드 시작하기 1

### 핵심 개념

* Node.js는 크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임. Node.js는 이벤트 기반, 논블로킹 I/O 모델을 사용해 가볍고 효율적. Node.js의 패키지 생태계인 npm은 세계에서 가장 큰 오프소스 라이브러리 생태계. (노드의 공식 사이트)

#### 1. 서버

* 서버란 네트워크를 통해 클라이언트에 정보나 서비스를 제공하는 컴퓨터 또는 프로그램
* 클라이언트란 요청을 보내는 주체(브라우저, 데스크톱 프로개름, 모바일 앱, 다른 서버에 요청을 보내는 서버 일 수 있음)
* 서버의 역할 예시: 웹사이트 주소를 주소창에 입력(요청)하면 브라우저는 그 주소에 해당하는 컴퓨터 위치를 파악. 그 컴퓨터에서 웹 사이트 페이지를 받아와 요청자의 클라이언트에 띄워줌(응답)
* 즉, 사용자의 데이터와 서비스의 데이터가 서버에 저장되고, 서버에서 클라이언트로 데이터를 받아오게 된다.
*  다른 서버에 요청을 보낼 때는, 요청을 보낸 서버가 클라이언트.
*  노드는 자바스크립트 애플리케이션이 서버로서 기능하기 위한 도구를 제공하므로 서버역할 수행 가능.


#### 2. 자바스크립트 런타임

* 런타임이란 특정 언어로 만든 프로그램들을 실행할 수 있는 환경.(노드는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있게 해준다.)
* 기존의 자바스크립트 프로그램을 인터넷 브라우저 위에서만 실행할 수 있었다.
* 구글이 V8엔진을 사용하여 크롬을 출시할 때, 다른 자바스크립트 엔진과 달리 매우 빨랐고, 속도 문제가 해결되자 라이언 달은 2009년 엔진 기반의 노드 프로젝트를 시작.
* 노드는 V8과 더불어 libuv라는 라이브러리를 사용.
* libuv 라이브러리는 노드의 특성인 이벤트 기반, 논블로킹 I/O 모델을 구현하고 있다. 

#### 3. 이벤트 기반
* 이벤트가 미리 발생할 때 미리 지정해둔 작업을 수행하는 방식.
* 이벤트 기반 시스템에서는 특정 이벤트가 발생할 때 무엇을 할지 미리 등록해두어야 하는데, 이것을 이벤트 리스너에 콜백 함수를 등록한다고 표현.
* 노드 역시 이벤트 기반 방식으로 동작하므로, 이벤트가 발생하면 이벤트 리스너에 등록해둔 콜백 함수를 호출. 발생한 이벤트가 없거나 발생했던 이벤트를 다 처리하면 다음 이벤트가 발생할 때까지 대기.
* 이벤트 루프는 여러 이벤트가 동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지를 판단. 이벤트 발생 시 호출할 콜백 함수들을 관리하고, 호출된 콜백 함수의 실행 순서를 결정하는 역할을 담당. 노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복해서 루프라함.
* 태스트 큐 : 이벤트 발생 후 호출되어야 할 콜백 함수들이 기다리는 공간. 콜백 큐라고도 부름
* 백그라운드 : 타이머나 I/O 작업 콜백 또는 이벤트 리스너들이 대기하는 곳.

#### 4. 논블로킹 I/O
* 오래 걸리는 함수를 백그라운드로 보내서 다음 코드가 먼저 실행되게 하고, 그 함수가 다시 태스크 큐를 거쳐 호출 스택으로 올라오기를 기다리는 방식. 
* 논블로킹이란 이전 작업이 완료될 때까지 멈추지 않고 다음 작업을 수행함을 의미.
* 싱글 스레드라는 한계때문에 자바스크립트의 모든 코드가 논블로킹 방식으로 시간적 이득을 보지는 못함.
* I/O는 입력/출력을 의미. 파일 시스템 접근(파일 읽기,쓰기, 폴더만들기 등)이나 네트워크 요청 같은 작업이 I/O의 일종. 이런 작업을 할 때 노드는 논블로킹 방식으로 동작.
* setTimeout(콜백,0) : 밀리초를 0으로 설정해서 바로 실행되는 것으로 착각할 수 있지만 브라우저와 노드에서는 기본적인 지연 시간이 있어서 바로 실행되지는 않는다.

#### 5. 싱글 스레드
* 스레드는 간단히 말해서 컴퓨터 작업을 처리할 수 있는 일손.
* 싱글 스레드는 주어진 작업을 혼자서 처리(노드)
* 멀티 스레드는 여러 개의 스레드가 일을 나눠서 처리.
* 자바스크립트와 노드에서 논블로킹이 중요한 이유는 바로 싱글 스레드이기 때문.
* 한 번에 한 가지 일밖에 처리하지 못하므로 어떠한 작업에서 블로킹이 발생하면 다음 일을 처리하지 못한다.
* 노드 또한 싱글 스레드 여러 개를 사용해 멀티 스레딩과 비슷한 기능을 하게 할 수 있는데 엄밀히 말하면 멀티 프로세싱에 가깝다.
* 프로세스는 운영체제에서 할당하는 작업의 단위. 프로세스 간에는 메모리 등의 자원을 공유하지 않는다.
* 스레드는 프로세스 내에서 실행되는 흐름의 단위. 하나의 프로세스는 스레드를 여러개 가질 수 있다. 스레드는 부모 프로세스의 자원을 공유한다. 즉 같은 메모리에 접근할 수 있다.
* 노드는 스레드를 늘리는 대신, 프로세스 자체를 복사해 여러 작업을 동시에 처리하는 멀티 프로세싱 방식을 택함. 자바스크립트 언어 자체가 싱글 스레드 특성을 지니고 있기 때문.


---

#### 출처 Node.js 교과서[조현영, 길벗]