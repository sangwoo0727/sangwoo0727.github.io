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
* 간단한 토이 프로젝트들도 진행하며 자바스크립트의 고수가 되어야지.
* __이 기술, 이 언어가 왜 필요한지, 왜 공부해야 하는지를 항상 머리속에 되뇌자.__

### <span style="color:#34495e">CHAPTER 1. 타입</span>
* 1장은 자바스크립트같은 [동적 언어](https://sangwoo0727.github.io/javascript/JavaScript-1_DynamicStaticLang/)는 타입 개념이 없다고 생각하는 개발자가 많으며, 저자는 이것에 대해 반박을 하며 시작한다.
* ECMAScript 표준 명세서에 따르면 __'타입'이란 자바스크립트 엔진, 개발자 모두에게 어떤 값을 다른 값과 구별할 수 있는 고유한 내부 특성의 집합이다__ 라고 정의하고 있다.
* 즉, 42(숫자)와 "42"(문자열)은 다르다!
* __타입별로 내재된 특성을 제대로 알고 있어야 값을 다른 타입으로 변환하는 방법을 정확히 이해할 수 있다.__

### <span style="color:#34495e">1.1 내장 타입</span>
* 자바스크립트에는 __null / undefined / boolean / number / string / object / symbol(ES6부터)__ 7가지의 내장 타입이 있다.
* 값 타입은 typeof 연산자로 알 수 있다.
* 하지만, typeof 반환 값은 7가지의 내장 타입과 1:1로 정확하게 매치되지 않는다.
* 그 이유는 바로 null에 대한 typeof 연산 결과는 "object"이기 때문이다.
* 그렇기에 , 타입으로 null값인지 정확히 확인하려면 조건이 하나 더 필요하다.
  
```javascript
let a = null;
console.log(!a && typeof a === "object");
```

* 즉, __null은 falsy한 유일한 원시 값이지만, 타입연산의 결과는 "object" 로 반환되는 특별한 존재__ 이다.
* 또한 typeof 반환 값은 위의 내장 타입말고 "function"이 있다.

```javascript
console.log(typeof function a(){});
```

* typeof 반환 값을 보면 마치 function이 최상위 레벨의 내장 타입처럼 보이지만 명세를 읽어보면 실제로는 object의 __하위 타입__ 이다.
* 즉, 함수는 __호출 가능한 객체(내부 프로퍼티 [call]로 호출할 수 있는 객체)__ 라고 명시되어 있다.
* 함수가 객체라는 증거는 아래의 코드로 알 수 있다.

```javascript
function f(b,c){};
console.log(f.length);
```

* length 프로퍼티는 함수에 선언된 인자 개수를 반환한다.
* 배열 역시 __typeof의 반환값은 "object"__ 다.
* 배열은 __키가 문자열인 객체와 반대로 숫자 인덱스를 가지며, length 프로퍼티가 자동으로 관리되는 등 추가 특성을 지닌 객체의 '하위 타입'이라 할 수 있다__.

### <span style="color:#34495e">1.2 값은 타입을 가진다</span>
* __값에는 타입이 있지만 변수엔 따로 타입이 없다.__
* 이 문장은 C++나 JAVA를 공부하는 입장에서 굉장히 흥미로웠다.
* 자바스크립트는 타입 강제(Type Enforcement)를 하지 않는다. 변수값이 처음에 할당된 값과 동일한 타입일 필요는 없다.
* JAVA와 아주 다르다.
* 예를 들어 보자 JAVA는 long num = 42; 문장에서도 컴파일 에러가 발생한다. (리터럴 상수를 다른 표기 없이 사용하면 int형으로 인식하기 때문에 42L로 해야한다)
* 이렇게 타입에 엄격한 JAVA와 달리 javaScript의 변수는 어떤 타입이든 다 담을 수 있다.
* 처음 자바스크립트를 공부하는 분들에겐 분명 흥미있는 내용일 것이다.
* __즉, 변수에 typeof 연산자를 대보는 건 변수의 타입을 묻는 것이 아닌 변수에 들어있는 값의 타입을 물어보는 것이다.__
* 또한 typeof 연산자의 반환값은 언제나 문자열이다.

### <span style="color:#34495e">1.3 값이 없는 VS 선언되지 않은</span>
* 값이 없는 변수(undefined)와 선언되지 않은(undeclared)변수의 typeof 연산 결과는 둘다 "undefined"로 나온다.

```javascript
let a;
console.log(typeof a); //undefined;
console.log(typeof b); //undefined;
```

* 저자는 이것은 typeof만의 __독특한 안전가드__ 라고 칭한다.
* 즉, 선언되지 않은 변수에 사용해도 에러를 내지 않기 때문에 잘 사용하면 제법 쓸만하다는 것이다.

### 출처

__You Don't Know JS, 카일 심슨 저, 한빛미디어__

