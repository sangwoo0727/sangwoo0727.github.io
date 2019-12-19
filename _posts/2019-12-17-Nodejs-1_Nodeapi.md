---
title: "[NodeJS로 API 서버 구축하기] 1. HelloWorld"
categories:
  - NodeJS
read_time: false
tags:
  - NodeJS
comments:
  - true
---

## 들어가기 전

__NodeJS로 API 서버 구축하기__ 포스팅은 Nodejs와 express를 사용하여 간단하게 서버의 구조와 흐름을 잡아보는 포스팅입니다.

제목과 약간은 이질적이지만 거창한 프로젝트가 아닌, 공부한 이론을 정리하며 쓰는 글입니다.

## 노드js의 "Hello World"

[노드js 공식문서](https://nodejs.org/dist/latest-v12.x/docs/api/synopsis.html) 에 들어가면 hello-world.js 코드가 있다. 코드를 먼저 살펴보자.

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

결론부터 말하면, 코드를 작성한 후, 터미널에서 __node hello-world.js를__ 입력하면, 서버가 실행된다.

하나씩 살펴보자.

먼저 __const http = require('http');__ 문장은 노드의 기본모듈 중 __http 모듈을 가져오는 코드를__ 의미한다.
그 후, host 변수와 port 변수에 호스트네임과 포트번호를 할당시킨다.

이번엔 server 변수에 할당 된 것을 코드를 통해 봐보자.

```javascript
const server = http.createServer((req, res) => { 
  res.statusCode = 200; //상태코드 : 성공을 의미함
  res.setHeader('Content-Type', 'text/plain'); 
  res.end('Hello World!\n');
});
```

먼저 http 모듈에는 __createServer라는 메서드가__ 있다. 인자로 요청에 대한 콜백 함수를 넣을 수 있다.

즉, __요청이 들어올 때마다 콜백 함수가 매번 실행__ 되는 것.

콜백 함수 부분에는 req, res라는 매개변수가 존재하는데, req와 res는 각각 request와 response의 약자이다.

__req 객체는 요청에 관한 정보들을, res 객체는 응답에 관한 정보들을 담고 있다__.

res.end 라는 메서드를 사용했는데, 사실은 res.write 와 res.end 메서드를 사용할 수 있다.

__res.write의 첫 번째 인자에는 클라이언트로 보낼 데이터를 넣는다__.

__res.end는 응답을 종료하는 메서드이다__. 또한 res.write처럼 인자가 있다면, 그 데이터를 클라이언트에게 보내고 응답을 종료하게 된다.

다음으로 listen 메서드에 대해 알아보자.

```javascript
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

createServer 메서드 뒤에 listen 메서드를 붙히고, 포트번호와 호스트이름, 그리고 포트 연결 완료 후 실행 될 콜백 함수를 넣어 줄 수 있다.

__listen 메서드는 서버를 요청 대기 상태로 만들어 주는 역할을 하는 메서드이다__. 메서드 이름 그대로, 요청이 들어오는 지를 듣고 있다는 것으로 이해하면 쉽게 연상할 수 있다!

추가적으로, listen 메서드에 콜백함수를 넣는 대신에 listening 이벤트 리스너를 붙여도 된다.

```javascript
server.listen(port);
server.on('listening',()=>{
  console.log("Server running!");
});
server.on('error',(error)=>{
  console.error(error);
})
```

이와 같이 작성을 한 후 콘솔창에 node Hello-world.js 를 입력해보자.

![](/assets/img/Nodejs/20191217_1.png)

이렇게 입력을 하고 나면, 서버가 위에서 말한 것 처럼 요청 대기 상태가 된다.

콘솔창에서 CURL 명령어를 통해 요청을 보내보자.

![](/assets/img/Nodejs/20191217_2.png)

우리는 server가 3000번 포트에서 요청 대기를 하고 있다가 클라이언트에서 요청을 보내면, res.end 메서드에 인자로 "hello world"를 넣어 응답을 하기로 했었고, 그에 대한 결과를 위의 화면을 통해 확인할 수 있다.

![](/assets/img/Nodejs/20191217_3.png)

브라우저에서도 127.0.0.1:3000 으로 접속하면, 똑같은 응답을 받아서 화면에 출력되는 것을 알 수 있다.

## 마무리

여기까지 노드js에서의 HelloWorld에 대해 공부해보았다.

서버를 요청 대기 상태로 만들고, 클라이언트가 요청을 하면 그에 대한 응답을 보내는 간단한 코드였다.

다만, 이렇게 짠 코드는 항상 서버의 루트경로로 밖에 접근할 수 밖에 없다.

즉, 127.0.0.1:3000/users 나 127.0.0.1:3000/id 로 접속을 해도 똑같은 응답만 받게 된다.

다음 포스팅에서는 127.0.0.1:3000/users 이런 다양한 요청들에 대해 서버에서 요청에 맞는 응답을 하는 방법에 대해 다뤄보겠다.






