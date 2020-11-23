---
title: "[Python] 네임드 튜플"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
## 네임드 튜플이란?
* 네임드튜플은 일반 튜플이 인덱스로 접근하는 것에 비해, 이름으로 접근이 가능한 튜플을 말한다.
* 공식 레퍼런스에 나온 소개를 보자

```
Named tuples assign meaning to each position in a tuple and allow for more readable, self-documenting code. 
They can be used wherever regular tuples are used, and they add the ability to access fields by name instead of position index.
```

* 공식 레퍼런스에서는 객체를 사용하는 것보다 튜플 활용을 추천하고 있다.
* 네임드 튜플이 유용한 이유는 다음과 같다.
* 튜플의 기본 성질인 불변 객체
* 일반 객체 형태보다 적은 메모리 사용
* 다양한 접근법 지원
* Dictionary key와 같이 사용

## 네임드 튜플 사용법
* 네임드 튜플을 사용하지 않은 코드를 한번 살펴보자.

```python
human = (180,75) # 사람의 키와 몸무게
print(human) # (180, 75)
```

* 주석이 아니었다면, 저 값이 어떤 것을 의미하는지 파악이 어려울 것이다.
* 왼쪽에 있는 값이 키이고, 오른쪽에 있는 값이 몸무게라는 정보를 튜플에 새겨넣고 싶을때 네임드 튜플을 사용한다.

```python
from collections import namedtuple  # collections 모듈에서 namedtuple 불러옴

human = namedtuple('Human', ['height', 'weight'])  # 네임드 튜플 클래스 만듬

h = human(180, 75)  # 네임드튜플 객체 생성
print(h[0], h[1])  # 일반 튜플과 같은 접근법
print(h.height, h.weight)  # 이름으로 접근하는 방법
```

* 네임드 튜플 클래스를 만들 때는, 첫번째 값(Human)이 클래스 이름이 된다.
* 클래스라고 하지만, 기본 골격은 튜플이다.
* 이후 나오는 부분들이 일반 튜플과 다르게 갖는 이름이다.
* 만들어진 클래스를 가지고, 객체를 생성할 수 있다.

## 네임드 튜플의 성격
* 네임드 튜플은 일반 튜플과 마찬가지로, 저장된 값을 수정하지 못한다.
* 수정하려 하면 오류가 발생하는데, 위에서 선언해줬던 클래스 이름이 이때 보인다.

```
TypeError : 'Human' object does not support item assignment
```

* 클래스 이름이 나오기 때문에 조금 더 오류를 찾기 쉬워진다.
* 이때, 권장하는 방법은 클래스의 이름과 변수의 이름을 같게 선언하는 것이다. 
* 무엇이 맞다고 할 수는 없지만, 둘의 이름을 같게 하면 혼동을 줄일 수 있다.

```python
human = namedtuple('human','height weight')  # 네임드 튜플 이름 지정시 문자열에 담아도 된다.
h = human(180,75)
```

## 출처
* 열혈 파이썬 - 윤성우
* [Python Documentation](https://docs.python.org/3/library/collections.html#namedtuple-factory-function-for-tuples-with-named-fields)

