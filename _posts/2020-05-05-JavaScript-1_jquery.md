---
title: "[JavaScript] JQuery 기본"
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
## 들어가기 전
제이쿼리는 DOM을 조작하거나 Ajax 요청을 실행할 때 __널리 쓰였던__ 라이브러리이다. 널리 쓰였던 이라고 표현한 이유에 대해서는 잠시 후 설명하겠다.

제이쿼리 역시 DOM API를 이용하여 만든 것이기 때문에 제이쿼리로 할 수 있는 일은 모두 DOM API로 할 수 있다.

그럼에도 제이쿼리의 장점을 살펴보면 다음과 같다.

* 브라우저 호환성
* Ajax API의 단순성
* 내장된 DOM API를 단순하게 바꾼 메소드 제공

이런 장점이 있음에도 불구하고 제이쿼리를 널리 쓰였던 이라고 표현한 이유는 무엇일까?

DOM API와 브라우저 지원이 개선되면서 순수 자바스크립트(바닐라 자바스크립트)의 성능이 제이쿼리에 비해 좋아지고, 다양한 프론트 프레임워크 등이 등장하였다.

이에 따라 제이쿼리를 사용하던 기업들은 점점 제이쿼리를 걷어내는 추세이고, 새로운 기업들 역시 제이쿼리의 사용을 줄여나가고 있는 추세다.

하지만 제이쿼리 -> 다른 자바스크립트 프레임워크로 바꿔나가며 유지보수를 하고 있는 기업에 입사를 하게 될 경우, 제이쿼리를 모른다면 꽤나 힘들 것이다.

그렇기 때문에 여전히 제이쿼리를 아는 것은 중요하고, 여전히 널리 쓰이고 있기는 하기 때문에 익혀두는 것이 좋다고 생각한다.

## 제이쿼리 불러오기
제이쿼리는 CDN을 통하여 불러올 수 있다.

아래 중 어떤 것을 사용해도 상관없다.

<script src="https://code.jquery.com/jquery-3.5.0.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js"></script>


## DOM 기다리기
먼저 제이쿼리의 문법에 대해 간단히 알아보겠다.

```javascript
$(selector).action()
```

* (selector) : html 요소를 선택
* action() : html 요소와 관계된 동작 수행

브라우저가 HTML 파일을 읽고 해석하고 랜더링 하는 과정은 복잡하다.

제이쿼리는 브라우저가 페이지를 완전히 읽고 DOM을 구축한 다음에만 호출되는 콜백 안에 코드를 작성할 수 있다.

```javascript
$(document).ready(function(){
    // DOM이 로딩 완료된 시점에 실행된다.
});

//위의 코드는 아래와 같이 줄여서 선언이 가능하다.
$(function(){
    // DOM이 로딩 완료된 시점에 실행된다.
});
```

개인적으로는 이런 코드는 줄이지 않고 사용하는 것을 선호한다.

## 선택자
제이쿼리는 HTML 객체(DOM)을 탐색하는 방법으로 CSS 선택자와 유사한 방식을 사용한다.

* $("*") : 전체 선택자 , 모든 DOM을 선택한다.
* $("#id") : 아이디 선택자, 해당되는 id를 가진 DOM을 선택한다.
* $("elementName") : 해당되는 이름을 가지는 DOM을 선택한다.
* $(".className") : 클래스 선택자, 해당되는 클래스 이름을 가지는 DOM을 선택한다.

간단하게 예시를 살펴보겠다.

```html
<head>
    <script type="text/javascript">
        $(document).ready(function(){
            alert($("#unique2").html());
        });
        // 제이쿼리의 html() 메서드를 통해 태그의 값을 얻어내기 때문에
        // alert창에는 제이쿼리 입니다. 가 뜰 것이다.
    </script>
</head>
<body>
    <div id="unique">제이쿼리입니다.</div>
</body>
```

## 제이쿼리 문서 객체 조작 - 1
html() 메소드와 text() 메소드는 각각 선택된 html 요소의 내부에 html을 set/get 하거나, text를 set/get 할 수 있다.

```html
<head>
    <script>
        $(document).ready(function(){
            let html = $('h1').html();
            alert(html); // header1 출력 / 첫 번째 문서 객체의 내용물을 출력한다.
            let text = $('h1').text();
            alert(text); // header1 header2 header3 출력 / 선택자로 선택한 모든 문서의 객체의 글자를 이어서 출력한다.
        });
    </script>
</head>
<body>
    <h1> header1 </h1>
    <h1> header2 </h1>
    <h1> header3 </h1>
<body>
```

위의 코드가 get에 해당하는 내용이었다면 아래 예시 코드는 set에 관련된 내용이다.

```html
<head>
    <script>
        $(document).ready(function(){
            $('div').html('<h1>$().html() Method</h1>');
            //모든 div태그에 html 태그 <h1>$().html() Method</h1>가 삽입된다.

            $('div').text('<h1>$().html() Method</h1>');
            //text() 메소드는 html 태그를 인식하지 않고 문자열로 인식하여 한다.
        });
    </script>
</head>
<body>
    <div></div>
    <div></div>
</body>
```

css() 메소드 역시 동일하다. 
$('h1').css('color','red'); 는 h1 태그의 color 속성에 red를 입력하는 것이다.

## 문서 객체 조작 - 2
* addClass() : 문서 객체의 클래스 속성을 추가한다.

```html
<head>
    <script>
        $(document).ready(function(){
            $('h1').addClass('item');
        });
    </script>
</head>
<body>
    <h1>head1</h1> <!-- <h1 class="item">head1</h1>-->
    <h1>head2</h1> <!-- <h1 class="item">head2</h1>-->
</body>
```

* removeClass() : 문서 객체의 클래스 속성을 제거한다. 매개변수에 아무것도 입력하지 않으면, 모든 클래스를 제거한다.

```html
<head>
    <script>
        $(document).ready(function(){
            $('h1').removeClass('select');
        });
    </script>
</head>
<body>
    <h1 class="item">head1</h1>
    <h1 class="item select">head2</h1> <!-- <h1 class="item">head2</h1>-->
    <h1 class="item">head3</h1>
</body>
```

* attr() : 선택된 html 요소의 내부에 attribute를 set/get 하는 메소드

```javascript
// 문법 : .attr(attributeName)

$('div').attr('class'); // div요소의 class 속성 값을 가져온다.(set의 경우에는 첫 번째 문서 객체의 속성을 출력한다)

$('h1').attr('title','hello'); // h1요소에 title 속성을 추가하고 속성의 값은 hello로 한다.

$('img').attr('width',200); // get에 관련해서는 img 태그의 width 속성이 변경된다.
```

* removeAttr() : 선택된 html 요소의 attribute를 삭제한다.

```html
<head>
    <script>
        $(document).ready(function(){
            $('h1').removeAttr('data-index');
        });
    </script>
</head>
<body>
    <h1 data-index="0">head1</h1> <!-- <h1>head1</h1> -->
    <h1 data-index="1">head2</h1> <!-- <h1>head1</h1> -->
    <h1 data-index="2">head3</h1> <!-- <h1>head1</h1> -->
</body>
```

* removeClass() 메서드와 removeAttr() 메서드 모두 클래스 속성을 제거할 수 있지만, removeAttr() 메서드를 사용하면 모든 클래스 속성이 한번에 제거된다. 반면, removeClass()는 여러개의 클래스 속성 중에서 선택적으로 제거 가능.


## 마무리
간략한 부분에 대해서만 일단 정리를 해놨다. 추가로 이벤트 메소드나 순회 메소드 등에 대해서는 따로 적어 놓지는 않겠다.

그럼 이만~
