---
title: "[Python] 리스트와 함수"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
## 리스트와 함수
* 파이썬을 공부하면서, 리스트형 변수 st가 있다고 할 때, len(st) 와 같이 그대로 호출하는 함수와, st.remove(2)처럼 호출하는 함수에 대해 궁금해졌다.

## 객체(Object)를 통한 이해
* 자바에서 객체에 대해 공부하였지만, 다시 한번 정리를 해보자.

```
Object:

Any data with state (attributes or value) and defined behavior (methods). 
Also the ultimate base class of any new-style class.
```

* __출처 : [Python Glossary](https://docs.python.org/3/glossary.html)__
* 객체는 __어떠한 속성값과 행동을__ 가지고 있는 데이터이다.
* 파이썬에서는 __모든 것들이 여러 속성과 행동을 가지고 있는 객체__ 이다.
* 속성 값과 행동 즉, 데이터와 함수가 묶여서 존재하는 덩어리를 객체라고 한다.
* 위에서 언급한 리스트형 변수 st 역시 객체이다.
* 객체이므로, remove라는 행동 즉 메소드를 호출할 수 있는 것이다.
* 객체 안에 존재하는 함수는 같은 객체에 있는 데이터를 대상으로 일을 한다.

## 리스트 객체의 여러 메소드
* s.append(x) : 리스트 s의 끝에 x를 추가
* s.extend(x) : 리스트 s의 끝에 리스트 t의 내용을 전부 추가
* s.clear() : 리스트 s의 내용 전부 삭제
* s.insert(i, x) : s[i]에 x를 저장
* s.pop(i) : s[i]를 반환 및 삭제
* ... 등등

```
cf) 리스트와 비슷하게 쓰이는 문자열은 리스트와 달리 그 내용을 바꾸지 못한다.
s = 'happy'
# s[1] = x
# 이렇게 작성하면, 's' object does not support item assignment 에러가 발생한다.
```

## 결론
* 객체 안에서 선언하고 사용하는 함수들은 해당 객체만을 대상으로 동작한다는 특징이 있다. 즉, __해당 객체에 특화되어 있다__
* 반면, len(st)처럼 객체 밖에 있는 함수들은 다양한 값이나 객체들을 대상으로 동작한다.

## 출처
* [Python Glossary](https://docs.python.org/3/glossary.html)
* 윤성우의 열혈 파이썬