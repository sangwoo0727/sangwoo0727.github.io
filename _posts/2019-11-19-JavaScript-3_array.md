---
title: "[JavaScript] 배열(array)"
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
* C언어 등 타입이 엄격한 언어들을 먼저 공부한 사람들은 자바스크립트의 배열을 보면 당혹스러우면서도 흥미로울 것이다.
* 바로 들어가보자!

### <span style="color:#34495e">배열</span>
* 자바스크립트 배열은 타입이 엄격한 다른 언어와 달리 __문자열, 숫자, 객체 심지어 다른 배열__ 등 어떤 타입의 값이라도 담을 수 있는 그릇.
* 배열의 크기는 미리 정하지 않고도 선언할 수 있다.

### <span style="color:#34495e">주의해야 할 사항 - 1</span>

* __빈 슬롯이 있는 구멍난 배열__ 은 조심해야 한다.

```javascript
let a = [];
a[0]=1;
a[1];
a[2]=3;
a.length; // 3
```

* 중간에서 건너뛴 빈슬롯, a[1]의 값은 undefined가 될 것 같지만, 명시적으로 a[1] = undefined와 같지 않다.
* 배열 값에 delete연산자를 적용하면, 슬롯을 제거할 수 있지만, 마지막 원소까지 제거해도 length 프로퍼티 값까지 바뀌지 않는다!

### <span style="color:#34495e">주의해야 할 사항 - 2</span>
* 배열 인덱스는 숫자인데, 배열 자체도 하나의 객체여서 키/프로퍼티 문자열을 추가할 수 있다.
* __하지만, 배열 length가 증가하지는 않는다.__
* 다만, 키로 넣은 문자열 값이 __표준 10진수 숫자로 타입이 바뀌면__, 마치 문자열 키가 아닌 숫자 키를 사용한 것 같은 결과가 초래된다.

```javascript
let a = [];
a["15"]= 3;
a.length; //16이 출력됨.
```

* 즉 __배열에 문자열 타입의 키/프로퍼티를 두는건 비추!! 이런 경우 객체를 쓰고, 배열 원소의 인덱스는 숫자만 쓰자는게 결론!__

### <span style="color:#34495e">유사 배열</span>
* 유사 배열이 무엇일까?
* 쉽게 말해 배열이 아닌데 배열인척 하는 것을 __유사배열 객체 혹은 유사배열__ 이라고 부른다.

```javascript
var array = [1,2,3];
array;
var nodes = document.querySelectorAll('div');
var els = document.body.children;

Array.isArray(array); // true
Array.isArray(nodes); // false
Array.isArray(els); // false
```

* Array.isArray() 메서드를 통해 확인해볼 수 있다.
* nodes나 els 처럼 []로 감싸져있지만 배열이 아닌 것들을 유사배열이라고 한다.

```javascript
var arrayLikeObjects = {
  0: 'a';
  1: 'b';
  2: 'c';
  length: 3
};
```

* 키가 숫자고, length라는 속성을 가지고 있는 이러한 객체가 바로 유사배열이다.
* 배열도 객체라는 성질을 이용한 트릭.
* __배열과 유사배열을 구분해야 하는 이유는__, 유사배열의 경우 배열의 메서드를 쓸 수 없기 때문.
* 유사배열에 forEach같은 배열 메서드를 사용하면 에러가 발생한다.(nodes 의 경우 프로토타입에 forEach가 있어서 가능.), 즉 배열이 아니므로 발생하는 에러이다.
* 이런 경우, __배열 프로토 타입__ 에서 forEach 메서드를 빌려와서 사용할 수 있다.(call이나 apply)
* 이렇듯, 유사 배열 값을 진짜 배열로 바꾸고 싶을 때가 있다.
* 이런 경우 배열 유틸리티 함수(indexOf(), concat(), forEach() 등)을 사용하여 해결하는 것이 일반적이다.
* DOM 쿼리작업을 수행하여 유사배열형태의 DOM원소 리스트가 반환되면, 배열로 변환하는 것이다.

```javascript
// arguments 객체를 사용하여 인자를 리스트로 가져오는 것(ES6부터 비권장)
function foo(){
  var arr = Array.prototype.slice.call(arguments);
  arr.push("bam");
  console.log(arr); // [ 'bar', 'baz', 'bam' ]
}
/* cf. 
   Array.prototype.slice.call(arguments) 는 Array.prototype.slice.call(arguments, 0)과 같다.
   즉, 첫번째 원소부터 끝까지 잘라내므로 배열을 복사하는 것이나 다름 없다. */

foo("bar","baz"); 
```

* ES6부터는 기본 내장 함수 Array.from()으로 편하게 유사배열을 진짜 배열로 바꿔준다.
* __var arr = Array.from(arguments);__

### 출처

__You Don't Know JS, 카일 심슨 저, 한빛미디어__

[배열과 유사배열](https://www.zerocho.com/category/JavaScript/post/5af6f9e707d77a001bb579d2)

[[javascript] 유사배열](https://sub0709.tistory.com/13)