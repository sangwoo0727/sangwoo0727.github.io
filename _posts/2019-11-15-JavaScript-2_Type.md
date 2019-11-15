---
title: "[JavaScript] 타입(Type)"
categories:
  - JavaScript
read_time: false
tags:
  - JavaScript
comments:
  - true
---

* 이 책은 __You Don't Know JS, 카일 심슨 저, 한빛미디어__ 를 공부하며 작성한 글입니다.
* 단순히 블로그만 읽는 것은 추천하지 않습니다.

### <span style="color:#34495e">들어가기 전</span>
* 바닐라 자바스크립트에 대한 필요성을 느껴 요즘은 자바스크립트 공부를 하고 있다.
* 너무 재밌어서 시간이 가는 줄 모르겠다..
* 간단한 토이 프로젝트들도 진행하며 자바스크립트의 고수가 되야지
* __이 기술, 이 언어가 왜 필요한지, 왜 공부해야 하는지를 항상 머리속에 되뇌자__

### <span style="color:#34495e">CHAPTER 1. 타입</span>
* 1장은 자바스크립트같은 [동적 언어](https://sangwoo0727.github.io/javascript/JavaScript-1_DynamicStaticLang/)는 타입 개념이 없다고 생각하는 개발자가 많으며, 저자는 이것에 대해 반박을 하며 시작한다.
* ECMAScript 표준 명세서에 따르면 __'타입'이란 자바스크립트 엔진, 개발자 모두에게 어떤 값을 다른 값과 구별할 수 있는 고유한 내부 특성의 집합이다__ 라고 정의하고 있다.
* 즉, 42(숫자)와 "42"(문자열)은 다르다!
* __타입별로 내재된 특성을 제대로 알고 있어야 값을 다른 타입으로 변환하는 방법을 정확히 이해할 수 있다.__

### <span style="color:#34495e">1.1 내장 타입</span>
* 자바스크립트에는 __null / undefined / boolean / number / string / object / symbol(ES6부터)__ 7가지의 내장 타입이 있다.
* 값 타입은 typeof 연산자로 알 수 있다.
* 