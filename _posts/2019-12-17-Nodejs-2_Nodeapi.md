---
title: "[NodeJS] NodeJS로 API 서버 구축하기 - 2"
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

[지난 포스팅](https://sangwoo0727.github.io/nodejs/Nodejs-1_Nodeapi/)에서는 공식문서에서 노드js의 Hello World라 불리는 코드를 통해 서버를 만들고, 요청 대기상태로 두고 클라이언트가 요청을 할 때마다 응답을 보내주는 기본적인 동작 방식을 알아보았다.

## 사용자의 요청에 따른 분기

클라이언트는 127.0.0.1:3000만 요청하지 않는다. 로그인을 한다던가, 웹에 있는 버튼을 누르는 등의 다양한 요청을 보낸다.

그럴때, __사용자가 요청한 정보의 경로명에 따라 서버는 그 경로에 맞게 응답을 보내는 역할을 해야한다__.

즉, __요청에 따른 분기가 필요하다__.

아래 코드를 봐보자.

```javascript
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});
```

이 코드에서 createServer 메소드의 인자는 요청이 들어올 때마다 실행이 되는 콜백함수이다.

이 부분을 요청한 정보의 경로명에 따라 응답을 하는 코드로 수정을 해야한다. 

먼저, 위의 코드를 통해 경로가 어떻게 들어오는지 살펴보자.

```javascript
const server = http.createServer((req, res) => {
  console.log(req.url);
  //req 객체의 url 메소드는 요청 정보에 대한 url 경로를 반환.  
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});
```

서버를 실행시킨 후, 콘솔창에 curl -X GET 127.0.0.1:3000/users 명령어를 입력하면 아래와 같은 화면을 볼 수 있다.

![](/assets/img/Nodejs/20191217_4.png)

if문과 url 메소드를 통해 경로명에 따라 분기처리를 해보자.

```javascript
const server = http.createServer((req, res) => { 
    if(req.url === '/'){
        res.statusCode = 200; 
        res.setHeader('Content-Type', 'text/plain'); 
        res.end('Hello World!\n');
    }
    else if(req.url === '/users'){
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('User list');
    }
    else{
        res.statusCode = 404;
        res.end('Not Found');
    }
});
```

req.url 메서드를 통해 반환된 요청 경로가 '/' 일때와 '/users', 그리고 그외의 경우에 대해 if문을 통해 분기 처리를 하였다.

결과를 봐보자.

![](/assets/img/Nodejs/20191217_5.png)

루트 경로로 요청을 보냈을 경우, /users 일 경우, 그외의 경우에 대해서 응답을 각각 다르게 하는 것을 볼 수 있다!!

이전 포스팅과 비교를 하였을 때 정말 혁신이다.

하지만, 이런 코드 역시 문제가 있다. __이렇게 개발을 하게 될 경우, 분기문은 엄청나게 많아질 것이고, 이 하나의 파일은 굉장히 커지게 될 것이다. 또한, 중복코드 역시 너무 많이 늘어날 것이다__.

이러한 문제점을 해결해주는 역할을 하는 프레임워크가 바로 Express.js이다.

다음 포스팅에선 Express.js 프레임워크에 대해 알아보고, 이를 통해 더욱 예쁜 코드를 만들어보자.







