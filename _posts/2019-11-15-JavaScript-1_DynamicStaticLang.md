---
title: "[JavaScript] 동적언어와 정적언어"
categories:
  - JavaScript
read_time: false
tags:
  - JavaScript
comments:
  - true
---

### <span style="color:#34495e">들어가기 전</span>
* 자바스크립트에 대해 공부를 하며 포스팅을 하기로 마음먹었다.
* 먼저 자바스크립트는 대표적인 동적언어인데, 이에 대한 포스팅을 하며 자바스크립트의 문을 열어보려 한다.

### <span style="color:#34495e">정적 언어 (Statically Typed Language)</span>
* __컴파일 시간에 변수의 타입이 결정되는 언어.__
* __타입__ 즉, 자료형을 __컴파일 시에 결정하는 것__.
* C, C++, Java 등은 대표적인 정적 언어이다.
* 정적 언어는 변수에 들어갈 값의 형태에 따라 자료형을 지정해주어야 한다.
* 컴파일 시에 자료형에 맞지 않는 값이 들어있을 경우 컴파일 에러가 발생한다.
* 컴파일 시간에 변수의 타입을 체크하므로 사소한 버그들을 쉽게 체크할 수 있는 장점이 있다.
* 즉 타입 에러로 인한 문제점을 초기에 발견할 수 있어 타입의 안정성이 올라간다.

### <span style="color:#34495e">동적 언어 (Dynamically Typed Language)</span>
* __런타임에 타입이 결정되는 언어.__
* 즉, 소스가 빌드될 때 자료형을 결정하는 것이 아니라 __실행 시 결정된다.__
* 매번 타입을 써줄 필요가 없기 때문에 프로그래머가 빠르게 코드를 작성할 수 있다.
* JavaScript, Ruby, Python 등은 대표적인 동적 언어이다.
* 런타임까지 타입에 대한 결정을 끌고 갈 수 있기 때문에 선택의 여지가 있다.
* 실행 도중에 변수에 예상치 못한 타입이 들어와 Type Error가 발생하는 경우가 생길 수 있다.

### <span style="color:#34495e">마치며</span>
* 이번 포스팅에선 동적 언어와 정적 언어에 대해 간단히 알아보았다.
* 자바스크립트는 대표적인 동적 언어이지만, 자바스크립트 역시 Type에 대해 재밌는 부분들이 많다. 
* 다음 포스팅은 Type에 대한 이야기로 시작하겠다.

### <span style="color:#34495e">참고</span>
* [StackOverflow](https://stackoverflow.com/questions/1517582/what-is-the-difference-between-statically-typed-and-dynamically-typed-languages)
* [정적언어(타입)과 동적언어(타입)](https://itmining.tistory.com/65)

