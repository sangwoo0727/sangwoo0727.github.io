---
title: "[Python] 파이썬 Dictionary setdefault 메소드와 defaultdict"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 딕셔너리와 관련하여 코드를 단순하게 짜는 방법에 대해 다룬다.
* 제목을 짓기가 조금 어려웠다.

## 일반적인 딕셔너리 기본 처리 구문
* 일반적으로 우리는 딕셔너리의 값을 참조할 때, 아래와 같은 코드로 작성한다.

```python
d = {'james':11, 'stef': 21, 'eden': 20}

d['james'] = 12

print(d)  # {'james': 12, 'stef': 21, 'eden': 20}
```

* 위와 같이 d['james'] 라는 참조 방법을 통해, 해당 키가 존재하지 않으면 새로운 키와 값의 추가가 진행된다.
* 만약 해당 키가 존재하면 해당하는 키의 값을 수정하는 결과가 이루어진다.
* 하지만, 저장되어 있는 값을 참조하는 경우는 예외를 조심해야 한다.

```python
d = {'james':11, 'stef': 21, 'eden': 20}

d['hazard'] += 1  # 존재하지 않는 키를 참조하므로 예외가 발생
```

* 이런 예외를 예방하기 위해서 아래와 같이 처리를 해주어야 한다.

```python
s = 'Hello my name is james'
d = {}

for k in s:
    if k in d:  # 키가 존재하면
        d[k]+=1
    else:
        d[k]=1

print(d)
```

## setdefault 메소드
* 위와 같은 방식은 if 문을 사용해야한다.
* 파이썬에서는 조금 더 간단하게 처리할 수 있는 방법이 있는데, 그 중 한가지가 setdefault 메소드를 사용하는 것이다.
* 문법은 다음과 같다. d.setdefault(k,v)
* k에 해당하는 키가 있을 때는 그 키의 값을 반환하고,
* 키가 없을 때는 딕셔너리에 k:v를 저장하고 v를 반환한다.

```python
s = 'Hello my name is james'
d = {}

for k in s:
    d[k] = d.setdefault(k, 0) + 1

print(d)
```

## defaultdict
* setdefault 말고 두번째 방법은 __디폴트 값을 갖는 딕셔너리__ 이다.
* 이러한 딕셔너리를 사용하기 위해서는 collections 모듈에서 defaultdict를 가져와야 한다.

```python
from collections import defaultdict

s = 'Hello my name is james'
d = defaultdict(int)

for k in s:
    d[k] += 1

print(d) 
```

* defaultdict 함수는 __디폴트 값을 갖는 딕셔너리__ 를 생성해서 반환한다.
* defaultdict(int)는 디폴트 값을 생성하는 함수로 int 를 사용했다.
* n = int() 로 아무 값을 전달하지 않으면 n에는 0이 전달된다.
* 즉, defaultdict에서 디폴트 값을 얻는데 int 함수가 사용되었다.
* int 함수처럼, 직접 함수를 구현하여도 가능하다.

```python
from collections import defaultdict


def make_default_value():
    return 20


d = defaultdict(make_default_value)
d['james']  # james라는 키가 없으므로 'james':20이 등록된다.

print(d)
```

* 추가적으로, make_default_value라는 디폴트 값을 생성하는 함수를 lambda로 구현하는 것 역시 당연하게도 가능하다.

```python
from collections import defaultdict

d = defaultdict(lambda: 20)
d['james']  # james라는 키가 없으므로 'james':20이 등록된다.

print(d)
```

## 출처
* 열혈 파이썬 - 윤성우
* [docs.python.org](https://docs.python.org/3/library/collections.html)



