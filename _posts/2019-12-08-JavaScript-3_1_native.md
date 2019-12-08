---
title: "[JavaScript] 네이티브-1"
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

### <span style="color:#34495e">[들어가기 전]</span>

네이티브는 내장 함수를 가리킨다.

네이티브란 단어가 자바스크립트에서 여기저기 남용되지만, 기본적인 정의를 내린다면 __특정 환경에 종속되지 않은, ECMAScript 명세의 내장 객체를 말한다__

즉, Object, Function, Array 등은 네이티브지만, Window, Button 등은 네이티브가 아니다.

가장 많이 쓰는 네이티브는 __String(), Number(), Boolean(), Array(), Object(), Function(), RegExp(), Date(), Error(), Symbol()__ 등이 있다.

### <span style="color:#34495e">[네이티브 사용법]</span>

코드를 먼저 봐보자.

```javascript
let s = new String("Hello World");
console.log(s.toString()); //"Hello World"
```

네이티브는 이렇게 생성자처럼 사용할 수 있다. 

그렇다면, 이렇게 만드는 것과 let s = "Hello World"; 로 선언하는 것이 무슨 차이가 있을까?

이 포스팅을 통해 천천히 알아가보자.

```javascript
let s = new String("abc");
console.log(typeof s); //"object"
```

먼저 타입에 있어서 "object"라는 결과값이 나온다. 네이티브를 생성자처럼 사용하면 그에 대한 결과는 원시값 "abc"를 감싼 객체 래퍼가 된다.

즉, 객체이므로 프로퍼티나 메서드를 사용할 수 있게 된다. (.length 등)

하지만 let s = "hello"; 역시 .length를 사용할 수 있지 않나? 이에 대한 설명을 아래 부분에서 마저 진행하겠다.

일단 핵심은 new String("abc")는 "abc"를 감싸는 __문자열 래퍼 를 생성하며 원시 값 "abc"는 아니라는 점__ 이다.

### <span style="color:#34495e">[내부 [Class]]</span>

typeof 가 "object"인 값에는 [[Class]]라는 내부 프로퍼티가 추가로 붙는다.

이 프로퍼티는 직접 접근할 수 없고, Object.prototype.toString() 이라는 메서드에 값을 넣어 호출함으로써 확인할 수 있다.

대부분 내부 [[Class]]는 해당 값과 관련된 내장 네이티브 생성자를 가리킨다.

그렇다면 원시값에도 내부 [[Class]]가 존재할까?

```javascript
Object.prototype.toString.call("abc"); //[object String]
Object.prototype.toString.call(42); //[object Number]
```

원시값에도 내부 [[Class]]가 존재한다. 이 말을 단순 원시값은 해당 객체 래퍼로 자동 박싱됨을 알 수 있게 해주는 대목이다.

즉 원시타입에 대응하는 래퍼객체가 존재하고, 래퍼객체는 원시타입을 감싸는 용도로 사용된다.

그렇기때문에, "hello".toUpperCase(); 처럼 "hello"가 string 타입의 원시자료형임에도 불구하고, 메소드를 호출할 수 있는 이유는 __String 자료형을 래퍼 객체로 임시로 변환하기 때문이다.__

### <span style="color:#34495e">[래퍼 박싱하기]</span>

객체 래퍼는 중요한 용도로 쓰인다. 원시 값엔 프로퍼티나 메소드가 없으므로,

.length 나 .toString() 등의 메소드를 사용하려면, 원시 값을 객체 래퍼로 감싸줘야한다.

하지만, 자바스크립트는 __원시값을 알아서 박싱해준다__

```javascript
let a = "abc"

a.length();
a.toUpperCase();
```

즉, 이렇기 때문에 굳이 직접 객체 형태를 써야할 이유는 거의 없다. 필요시 엔진이 알아서 암시적으로 박싱하게 하는 것이 성능 면에서도 더 좋다.

특별한 경우가 아니면, 보기 좋고 알기 쉽게 원시값을 사용하자!!

### 출처

__You Don't Know JS, 카일 심슨 저, 한빛미디어__
