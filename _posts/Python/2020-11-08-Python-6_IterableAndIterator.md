---
title: "[Python] Iterable 객체와 Iterator 객체"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 iterable 객체와 iterator 객체에 대해 알아보겠다.

## iter() 함수
* 어떠한 자료형에 저장된 값들을 하나하나 꺼내보는 일은 중요하고, 주로 for 루프를 기반으로 처리한다.
* for 루프외에도 아래와 같은 방법으로 값을 꺼낼 수 있다.

```python
arr = [1,2,3,4]
ir = iter(arr) # iterator 객체 얻는 방법

print(next(ir)) # 1 
print(next(ir)) # 2
```

* iter라는 이름의 함수를 호출하며, 리스트를 전달하였고, ir이라는 변수가 어떠한 객체를 참조하게 된다.
* 이렇게, iter함수가 생성하여 반환하는 객체를 iterator 객체라고 하고, 이는 리스트에서 값을 꺼내는 기능을 제공하는 객체이다.

## iterator 객체
* iterator 객체는 iter함수를 통해 반환되어진 객체이며, 값을 차례대로 꺼내는 기능을 제공하는 객체이다.
* ir = iter(arr) 이라는 명령어를 통해서, iter 함수는 리스트 arr에 접근하는 도구인 iterator 객체를 생성해서 반환한다.
* next 함수를 통해서, 객체를 전달하며 리스트에 저장된 값을 하나씩 꺼낼 수 있게된다.
* 마지막까지 값을 얻었는데, next 함수를 호출하고나면, StopIteration 예외가 발생한다.
* 만약 next(ir, 10) 등으로 인자를 주면, 마지막까지 값을 얻고난 후에 StopIteration 예외가 발생하지 않고, 10이라는 값이 계속해서 반환이 된다.
* 만약 처음부터 값을 다시 얻고 싶으면 어떻게 해야할까?
* iterator 객체를 새롭게 다시 얻으면 된다.

## iterable 객체
* iterator 객체에 대해서는 알았는데, iterable 객체는 무엇일까?
* iterable 객체는 iterator 객체를 얻을 수 있는 반복 가능한 객체 등을 가리켜 iterable 객체라고 한다.
* 대표적인 iterable한 타입은 list,dict,set,str,tupe 등이 있다.
* 결론적으로 __iterable 객체를 대상으로 iter함수를 호출해서 iterator 객체를 만든다__

## 스페셜 메소드
* 위에서 iterable 객체의 원소를 하나하나 꺼내보기 위한 작업을 진행하며, next() 함수와 iter() 함수를 보았다.
* 이 두 함수는 어디서 온 것일까?
* 이러한 함수들을 __스페셜 메소드__ 라고 한다.(매직 메소드라고도 한다)

```python
arr = [1,2,3,4]
ir = iter(arr)  # 내부적으로 ir = arr.__iter__() 이 호출된다.
next(ir)  # 내부적으로 ir.__next__() 이 호출된다.
```

* 위의 주석에서 알 수 있듯이, 실제로는 다른 모양의 함수가 호출되는데, 이를 가리켜 스페셜 메소드라고 한다.
* 이렇게 직접 호출하지 않아도 파이썬 인터프리터에 의해 호출되는 메소드를 스페셜 메소드라고 한다.
* 객체 생성 시 자동으로 호출되는 __init__메소드 역시 스페셜 메소드이다.
* dir(변수이름) 이라는 명령어를 통해 리스트 객체 안에 있는 모든 것의 이름을 확인할 수 있는데, 여기에 __iter__메소드가 존재한다면 이는 iterable 객체가 되는 것이다.

``` python
arr = [1,2,3,4]
print(dir(arr))

['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']

Process finished with exit code 0
```

## for 루프
* 사실 for문 역시도 iterable 객체에서 도움을 받는다.
* 우리가 사용하는 for문은 내부적으로 iterator 객체를 얻어서 사용한다.
* 즉, for문의 반복 대상은 무조건 iterable 객체여야 한다. 

## 출처
* 열혈 파이썬 - 윤성우
* [WikiDocs](https://wikidocs.net/16068)

