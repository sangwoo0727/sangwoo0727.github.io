---
title: "[JavaScript] 숫자와 특수 값"
categories:
  - JavaScript
read_time: false
tags:
  - JavaScript
comments:
  - true
toc: true
toc_sticky: true
---

* 이 책은 __You Don't Know JS, 카일 심슨 저, 한빛미디어__ 를 공부하며 작성한 글입니다.
* 단순히 블로그만 읽는 것은 추천하지 않습니다.

### <span style="color:#34495e">[들어가기 전]</span>

이번 포스팅은 숫자와 특수 값에 대한 자바스크립트 엔진에서의 특이한 점들에 대해 다룬다.

궁금한 사람만 읽으시면 될 듯!

### <span style="color:#34495e">[숫자]</span>

자바스크립트의 숫자 타입은 number가 유일하며, 정수와 부동 소수점 숫자를 모두 아우른다.

자바스크립트에서 정수는 부동 소수점 값이 없는 값이다.

즉, 42.0 === 42 으로, 자료형으로 구분하지 않는다.

또한 숫자 값은 Number 객체 래퍼로 박싱할 수 있기 때문에, Number.prototype 메서드로 접근 할 수 있다.

예를 들어보자.

```javascript
let a = 17.63;
console.log(a.toFixed(0)); // 18
console.log(a.toFixed(1)); // 17.6
console.log(a.toFixed(2)); // 17.63
```

실제로는 숫자 값을 문자열 형태로 반환한다.

위와 같은 메소드를 사용할 때 주의해야할 점은, __숫자 리터럴에서 바로 접근할 수 있으므로 굳이 변수를 만들어 할당하지 않아도 되지만 .이 소수점인지 프로퍼티 접근자인지 잘 판단해서 사용해야 한다.__

```javascript
42.toFixed(3); //Syntax Error
(42).toFixed(3); // 올바른 구문
42..toFixed(3); //올바른 구문
```

정수에 대해서 이어서 설명을 진행하겠다.

자바스크립트는 정말 큰 수까지 표시할 수 있기 때문에, 정수는 __안전 값의 범위__ 가 정해져 있다.

__Number.MAX_SAFE_INTEGER__ 와 __Number.MIN_SAFE_INTEGER__ 이다.

반면, 정수인지 확인하기 위해 __Number.isInteger()__ 메소드를 통해 ES6 부터는 정수 여부를 확인할 수 있다.

### <span style="color:#34495e">[특수 값 1. Undefined, null]</span>

Undefined 타입의 값은 Undefined 밖에 없다. null 타입 역시 값은 null뿐이다.

둘은 타입과 값이 항상 같다.

알아두면 좋은 잡지식? 인 것 같은데, undefined에는 값을 할당 할 수 있다. 저자는 절대 추천하지 않는 방식으로 강조하고 있다.

```javascript
let undefined = 2;
console.log(undefined); //2
```

실제 브라우저 콘솔창에서는 작동하지 않더라


### <span style="color:#34495e">[특수 값 2. void]</span>

void는 자바스크립트에서는 일반적인 프로그래밍 언어에서 사용하는 void의 의미와 다르다.

자바스크립트에서 void는 __어떤 값이든 무효로 만들어 항상 결과값을 undefined__ 로 만든다.

```javascript
let a = 50;
console.log(void a); //undefined
```

void 연산자는 어떤 표현식의 결과값이 없다는 걸 확실히 밝혀야 할 때 긴요하다고 한다. 아직 그런 상황을 겪어보질 않아서 잘 모르겠다..

### <span style="color:#34495e">[특수 값 3. 특수 숫자]</span>

__NaN 이란__ Not A Number의 약자로서, 연산과정에서 잘못된 입력을 받았음을 나타내는 기호이다. 글자 그대로는 숫자 아님인데, 실질적으로는 __유효하지 않은 숫자__ , __실패한 숫자__ 등으로 생각해야 이해가 편하다.

왜냐하면, NaN 에 typeof 연산자를 적용시키면 __Number__ 가 나와버림..

ES6부터는 __Number.isNaN()__ 으로 안전하게 NaN여부를 체크할 수 있다.

### <span style="color:#34495e">[특수 값 4. 무한대]</span>

전통적인 컴파일 언어는 어떤 값을 0으로 나눈 결과 값을 출력하려 하면, 에러가 난다.

자바스크립트는 에러 없이 __Infinity__ 결과값이 나온다.

### <span style="color:#34495e">[특수 값 4. 영]</span>

자바스크립트는 또한 음의 영이 존재한다.....

```javascript
let a = 0 / -3;; // -0
```

그런데 또 특이하게, -0을 문자열화 하거나, 비교 연산 결과는 0이 나온다.

```javascript
let a = 0 / -3;
a.toString(); // "0"
JSON.stringify(a); //"0"

let b = 0;
console.log(a === b); //true
console.log(b > a); //false
```

### <span style="color:#34495e">[특이한 동등 비교]</span>

두 값이 절대적으로 동등한지를 확인하는 ES6부터 지원하는 유틸리티 __Object.is()__ 

0과 -0이 같은지 다른지 비교해주고,

NaN을 비교해준다.

직접 실습해봐랏!

### 출처

__You Don't Know JS, 카일 심슨 저, 한빛미디어__