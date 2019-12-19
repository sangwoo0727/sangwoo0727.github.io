---
title: "[NodeJS로 API 서버 구축하기] 3. Express 사용하기"
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

## Express란?

[지난 포스팅](https://sangwoo0727.github.io/nodejs/Nodejs-2_Nodeapi/)에서 Express를 왜 써야하는지, 어떤 역할을 하는지에 대해 간략하게 알아보았다.

다시 한번 정리하자면, 이전 포스팅에선 if문을 통해서 요청 url을 분기하고, 조건별로 응답을 다르게 해주는 구문을 작성하였다.

이렇게 코드를 작성하여도 되지만, 다양한 분기를 하나의 파일에 작성해야 하고, 코드가 길어지고, 중복된 코드 역시 늘어나게 되는 단점이 생기게 된다.

npm에는 서버 제작 시 불편함을 해소하고, 편의 기능을 추가한 Express.js라는 프레임워크가 존재한다.

[Express 공식 홈페이지](http://expressjs.com/ko/)가 궁금한 사람들은 들어가 보는 것을 추천한다.

## Express 밖에 없나?

익스프레스 외에도 koa나 hapi 같은 웹 서버 프레임워크가 존재한다. 하지만 npm 패키지의 다운로드 수를 비교할 때 익스프레스가 압도적으로 많다.

많은 사람이 사용할수록 버그도 적고, 기능 추가나 유지 보수도 활발하게 일어난다. 

![](/assets/img/Nodejs/20191219_1.png)



## Express.js

Express.js는 Node.js를 위한 빠르고 간편한 웹 프레임워크이다.

__"Express.js는 Node.js의 핵심 모듈인 http와 Connect 컴포넌트를 기반으로 하는 웹 프레임워크다. 그러한 컴포넌트를 미들웨어(middleware)라고 하며, 설정보다는 관례(convention over configuration)와 같은 프레임워크의 철학을 지탱하는 주춧돌에 해당한다. 즉, 개발자들은 특정 프로젝트에 필요한 라이브러리를 어떤 것이든 자유롭게 선택할 수 있으며, 이는 개발자들에게 유연함과 수준 높은 맞춤식 구성을 보장한다."__

## Express 용어: 어플리케이션

익스프레스 인스턴스를 어플리케이션이라고 한다.

아래에서 배울 서버에 필요한 기능인 미들웨어는 어플리케이션에 추가한다.

라우팅 설정을 할 수 있고, 서버를 요청 대기 상태로 만들 수 있다.

어플리케이션을 한번 만들어보자.

```javascript
const express = require('express'); //express 모듈을 가져온다.
const app = express();  //express 객체를 app변수에 넣는다.
```

이전 포스팅에서는 http 모듈을 사용하였지만, express 모듈을 가져와 그 객체를 app에 넣음으로써 어플리케이션을 만든 것을 알 수 있다.


## Express 용어: 라우팅

Express에는 __라우팅__ 이라는 개념이 있다.

클라이언트가 서버로 접속할때는 특정한 URL를 통해 접속한다. 서버에서는 이 URL에 해당하는 자원을 클라이언트로 보내준다. 

뒤에서 다루겠지만, POST 요청일 경우는 자원을 만들기도 한다. 이러한 클라이언트 요청을 위한 URL스키마를 라우트라고 한다. 

서버에서는 라우팅 작업을 통해 클라이언트와 통신의 인터페이스를 제공해준다.

익스프레스에는 이러한 라우팅 모듈이 존재한다.

## Express 용어: 미들웨어

미들웨어는 익스프레스의 핵심이다. 요청과 응답의 중간에 위치하여 미들웨어라고 부른다.

라우터와 에러 핸들러 역시 미들웨어의 일종이므로 미들웨어는 익스프레스의 전부라고 해도 과언이 아니다.

미들웨어는 요청과 응답을 조작하여 기능을 추가하기도 하고, 나쁜 요청은 걸러내기도 한다.

미들웨어는 함수들의 연속이다.

아래 코드를 먼저 보고 설명을 이어나가도록 하겠다.

```javascript
const express = require('express');
const app = express();

function logger(req,res,next){ // 미들웨어는 인터페이스가 req,res,next로 정해져있다.
    console.log('I am logger');
    next(); //미들웨어는 자신이 할일을 다한다음에 next()함수를 호출해야한다.
    //호출해야지만 다음 로직을 수행한다. 
}

function logger2(req,res,next){
    console.log("I am logger2");
    next();
}

app.use(logger); //미들웨어를 추가할 때는 use라는 함수를 사용한다.
app.use(logger2);

app.listen(3000,function(){
    console.log('Server is running');
});
```

미들웨어는 순차적으로 위에서 아래로 실행된다. logger와 logger2라는 함수를 미들웨어로 사용하였고, 미들웨어는 req,res,next 인자를 가진다.

미들웨어 함수 안에서 함수의 역할이 끝나고나면 꼭 next() 함수를 호출해줘야 다음 로직을 수행할 수 있게 된다.

next()를 호출하지 않으면, 다음 로직을 수행하지 않고, 클라이언트 역시 응답을 받지 못하게 된다.

next() 함수는 미들웨어의 흐름을 제어하는 핵심적인 함수이다.

next() 함수에는 인자의 종류에 따라 몇 가지 기능이 구분된다. 먼저 인자를 넣지 않으면 단순하게 다음 미들웨어로 넘어간다. 인자로 route를 넣게 되면, 특수한 기능을 한다. 이런 경우에는 라우터에 연결된 나머지 미들웨어들을 실행하지 않게 된다. route이외의 다른 값을 넣으면, 다른 미들웨어나 라우터를 건너 뛰고 바로 에러 핸들러로 이동하게 된다. 넣어준 값은 에러에 대한 내용으로 간주된다.

![](/assets/img/Nodejs/20191220_1.png)

결과를 보면, 3000번 포트로 요청을 보내면, 미들웨어의 흐름에 따라 log가 찍히는 것을 알 수 있다.

## 마무리

다만 위의 코드는 아직까지, 요청과 응답에 대한 어떠한 처리도 하지 않았고, 단순히 미들웨어의 로직만 살펴보는 코드였다. 

다음 포스팅부터는 이러한 Express의 기본 구조와 미들웨어의 실행 순서들을 바탕으로 더욱 깊은 주제로 HelloWorld에 적용해나가보도록 하겠다.

## 출처

[WebFrameworks](http://webframeworks.kr/getstarted/expressjs/)

[Express.js - 2. 라우팅](http://jeonghwan-kim.github.io/express-js-2-%eb%9d%bc%ec%9a%b0%ed%8c%85/)
