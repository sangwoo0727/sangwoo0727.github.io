---
title: "[Python] 파이썬의 자료형과 Set, frozenset"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 set 타입에 대해 다룬다.
* 먼저 set 타입에 대해 다루기 전에, 파이썬에서 제공하는 자료형들에 대해 잠시 알아보겠다.

## Sequence Type
* sequence type의 가장 큰 특징은 저장된 값의 순서 정보(위치 정보)가 존재한다.
* 대표적인 sequence type에는 list, tuple, range, str 등이 있다.
* range 역시 값의 범위 정보를 저장하지만 범위 정보에도 시작과 끝이라는 순서가 존재한다.
* sequence type에서 사용할 수 있는 opertations는 공식 문서를 참고하자.

[docs.python.org](https://docs.python.org/3/library/stdtypes.html)

## Mapping Type

<i class="far fa-sticky-note"></i> **Mapping Types :**  A mapping object maps hashable values to arbitrary objects. Mappings are mutable objects.
{: .notice--primary}
{: .text-justify}

* 매핑 타입은 본질적으로 저장된 값의 순서 또는 위치 정보를 기록하지 않는다.
* 하지만, 파이썬 3.7부터는 딕셔너리 역시 저장된 값의 순서를 유지하긴 한다.
* sequence type과 달리 저장된 값의 순서 정보 등이 매핑 타입의 본질은 아니다.
* 딕셔너리가 있다.

## Set Type

<i class="far fa-sticky-note"></i> **Set Types :**  A set object is an unordered collection of distinct hashable objects.
{: .notice--primary}
{: .text-justify}

* 공식 문서에 따르면 set type의 정의는 위와 같다.
* 즉, 저장 순서를 유지하지 않고, 중복된 값의 저장을 허용하지 않는다는 뜻이다.
* 수학의 집합적인 의미를 차용한 듯하다.
* set과 frozenset이 있다.
* set 객체를 기반으로 수학의 집합처럼 합집합, 교집합, 차집합 대칭 차집합 연산이 가능하다.

```python
set1 = {'a', 'b', 'c', 'd'}  # set의 생성
set2 = {'a', 'b', 'd', 'e'}

print(set1 - set2)  # 차집합
print(set1 & set2)  # 교집합
print(set1 | set2)  # 합집합
print(set1 ^ set2)  # 대칭 차집합 (set1-set2) U (set-set1)

```

## set객체 생성 방법
* 첫번째 방법은 위에서 생성한 것과 같이 집합처럼 생성하는 방법이 있다.
* 두번째 방법은, set comprehension 방법이다.
* 세번째 방법은, 생성자를 이용하여 생성하는 방법이다.
* 2번째 방법과 3번째 방법은 iterable한 객체를 전달하여 생성한다.

```python
A = {'a','b','c'}
B = set('abc')
C = {c for c in 'abc'}

print(A)
print(B)
print(C)
```

* 추가적으로, set은 iterable한 객체를 전달하여 생성한다고 하였는데, 중복된 값이 존재하는 iterable 객체는 어떨까?
* set은 저장된 값들 중에서 중복된 값들을 제거하여 저장이 된다.

```python
l = [1, 1, 1, 1, 2, 2, 2, 2, 3]
s = set(l)
print(s)  # {1, 2, 3}

```

## frozenset
* frozenset과 set은 모든 문법과 기능이 거의 동일하다.
* 둘의 차이는 이름에서도 알 수 있듯이, __mutable 하냐__ 의 차이이다.
* set은 mutable 객체이고,
* frozenset은 immutable 객체이다.
* 즉, set은 새로운 값의 추가 또는 삭제가 가능하지만, frozenset은 불가능하다.

```python
s = {1, 2, 3, 4}
fs = frozenset([1, 2, 3, 4])

s.add(5)
#  fs.add(5) 불가능
s.discard(5)
#  fs.discard(5) 불가능

s.update({5, 6, 7})
#  fs.update({5,6,7}) 불가능

print(s)  # {1, 2, 3, 4, 5, 6, 7}
print(fs)  # frozenset({1, 2, 3, 4})
```

* set에 관한 추가적인 메소드는 공식문서를 참조하자.
* [docs.python.org](https://docs.python.org/3/library/stdtypes.html#set)

## 출처
* 열혈 파이썬 - 윤성우
* [docs.python.org](https://docs.python.org/3/library/stdtypes.html)
