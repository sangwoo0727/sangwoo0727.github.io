---
title: "[네트워크 프로그래밍] HTTP 메소드"
categories:
  - Network
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
## [네트워크 프로그래밍] HTTP 메소드
* HTTP 서버와의 통신은 요청-응답 패턴을 따른다. 하나의 무상태(stateless) 요청과 뒤이어 오는 하나의 무상태 응답으로 구성된다. 

### HTTP 요청
* HTTP 요청은 둘 또는 세 요소로 구성된다.
1. 첫 번째 줄은 HTTP 메소드와 메소드가 실행될 리소스의 경로를 포함하고 있다.
2. 이름-값 필드로 구성된 헤더는 인증 자격과 선호하는 데이터 타입과 같은 메타정보를 제공한다.
3. 요청 본문은 요청된 리소스의 실제 데이터를 포함하고 있다(POST와 PUT에서만 해당)

### HTTP 메소드 1. GET
* 요청한 리소스를 읽어드린다. (Read에 대응)
* GET 메소드는 요청에 실패해도 추가적인 부작용(side effect)이 발생하지 않기 때문에, 실패에 대한 걱정 없이 반복적으로 요청할 수 있다.
* GET의 결과는 종종 캐시에 저장된다.
* GET 요청은 어렵지 않게 북마크해 두거나 미리 가져올 수 있다.
* URL안에 필요한 모든 정보를 포함하고 있기 때문에, 북마크를 하거나 링크를 만들 때 또는 웹 스파이더링 등에 이용될 수 있다.

### HTTP 메소드 2. PUT
* PUT 메소드는 URL에 명시된 서버로 리소스를 업로드한다(UPDATE에 대응) 
* PUT 메소드는 부작용(side effect)에 대해 자유롭지 않지만, 멱등성(idempotence)를 가지고 있다. 즉, 실패 여부에 상관없이 반복해서 요청할 수 있다. 같은 문서를 같은 서버의 같은 위치에 연속해서 두 번 올리는 것은 한 번만 올렸을 때와 동일한 상태가 된다.

### HTTP 메소드 3. DELETE
* DELETE 메소드는 지정된 URL의 리소스를 삭제한다. (DELETE에 대응)
* 부작용으로부터 자유롭지 않지만, 멱등성을 가지고 있다.
* 삭제 요청의 성공 여부가 확실하지 않은 경우(예를 들어, 요청을 보내고 응답을 받기 전에 소켓 연결이 끊어진 경우) 요청을 다시 보내기만 하면 된다.
* 동일한 리소스를 두번 삭제하는 것은 문제가 되지 않는다.

### HTTP 메소드 4. POST
* 가장 일반적으로 사용되는 메소드 (CREATE에 대응)
* 클라이언트가 리소스의 위치를 지정하지 않았을 때 리소스를 생성하기 위해 사용하는 메소드.
* indempotence하지 않다. 추가적으로 요청할 경우 새로운 리소스가 생성되기 때문이다.
* 반면, PUT은 indempotence 하다. 몇 번을 요청해도 같은 결과를 보장하기 때문이다.
* URL에 명시된 서버로 리소스를 업로드 하지만, 새로 업로드된 리소스로 서버가 해야 할 일을 명시하지 않는다.
* 예를 들어, 서버는 업로드된 리소스에 대해 해당 URL로 반드시 접근 가능하도록 만들어야 하는 것은 아니다. 대신 다른 URL로 이동시키거나, 완전히 다른 리소스의 상태를 변경하는 데 업로드된 리소스를 사용할 수도 있다.
* POST는 구매하기와 같이 반복 요청에 대해 안전하지 않은 동작에 사용해야 한다.


#### 아직 공부 중인 부분이므로, 공부를 더 하는 대로 보완을 해서 완성도 있는 포스팅을 하겠습니다.