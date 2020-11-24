---
title: "[Python] 딕셔너리 저장순서와 OrderedDict"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 딕셔너리의 저장순서를 유지하는 방법을 알아본다.

## 파이썬 딕셔너리의 저장 순서
* 파이썬 3.6버전까지는 딕셔너리에 데이터를 삽입한 순서대로 데이터를 얻을 수 없었다.
* 하지만, 3.7버전부터는 딕셔너리가 데이터의 저장 순서를 유지한다.
* 3.7버전 이전에서는 데이터의 저장 순서를 유지하기 위하여 OrderedDict 객체를 생성하여 사용했다.

## OrderedDict
* collections 모듈에 존재하는 클래스이다.
* 딕셔너리의 저장 순서를 보장받을 수 있다.
* 파이썬 공식문서의 정의를 보자.

<i class="far fa-sticky-note"></i> **OrderedDict :**  Return an instance of a dict subclass that has methods specialized for rearranging dictionary order.
{: .notice--primary}
{: .text-justify}

* 예제 코드를 봐보자

```python
from collections import OrderedDict

od = OrderedDict()

od['james'] = 20
od['eden'] = 30
od['ethan'] = 31

print(od)

for atom in od.items():
    print(atom) 
```

* OrderedDict를 사용하면 이렇게 저장 순서를 유지하는 것을 알 수 있다.
* 하지만, 파이썬 3.7부터는 딕셔너리 자체가 저장순서를 유지하기 때문에 OrderedDict가 필요없게 되었다.
* 그럼에도 불구하고, OrderedDict는 여전히 필요성이 있다.
* 언제일까?

## 동등성 비교
* 딕셔너리에서는 저장 순서에 상관없이 데이터의 키와 값이 동일하면 값 동등성 비교에서 true를 반환한다.
* 반면 __OrderedDict__ 의 경우는 동등성 비교에 저장 순서도 중요하다.

```python
from collections import OrderedDict

d1 = {'james': 20, 'eden': 30, 'ethan': 31}
d2 = {'james': 20, 'ethan': 31, 'eden': 30}

print(d1 == d2)  # True

od1 = OrderedDict(james=20, eden=30, ethan=31)
od2 = OrderedDict(james=20, ethan=31, eden=30)

print(od1 == od2)  # False
```

* 물론 객체 비교는 is로 한다.
* 객체의 내용 비교이므로 ==을 사용한다.
* 내용 비교에 있어서, 딕셔너리의 저장 순서가 유의미하다면, OrderedDict를 사용하자.

## OrderedDict 메소드 - popitem
* OrderedDict에는 popitem이라는 메소드가 있다.
* 공식 문서의 정의를 먼저 살펴보자.

<i class="far fa-sticky-note"></i> **popitem(last=True) :**  The popitem() method for ordered dictionaries returns and removes a (key, value) pair. The pairs are returned in LIFO order if last is true or FIFO order if false.
{: .notice--primary}
{: .text-justify}

* 인자로 들어가는 last가 true일 경우, LIFO 방식으로, 아니면 FIFO 방식으로 item을 반환하고 제거하는 메소드이다.

```python
from collections import OrderedDict

od1 = OrderedDict(james=20, eden=30, ethan=31)

print(od1.popitem(True))  # ('ethan', 31)
print(od1)  # OrderedDict([('james', 20), ('eden', 30)])

print(od1.popitem(False))  # ('james', 20)
print(od1)  # OrderedDict([('eden', 30)])
```

## OrderedDict 메소드 - move_to_end
* 존재하는 해당 키를 딕셔너리의 맨 뒤로 옮기는 메소드이다.
* 공식 문서의 정의로 보자.

<i class="far fa-sticky-note"></i> **move_to_end(ket, last=True) :**  Move an existing key to either end of an ordered dictionary. The item is moved to the right end if last is true (the default) or to the beginning if last is false. Raises KeyError if the key does not exist:
{: .notice--primary}
{: .text-justify}

* 두번째 last 인자가 True일 경우 맨 뒤로 보내고, False일 경우 맨 앞으로 보낸다.

```python
from collections import OrderedDict

od1 = OrderedDict(james=20, eden=30, ethan=31)

od1.move_to_end('james')
print(od1)  # OrderedDict([('eden', 30), ('ethan', 31), ('james', 20)])

od1.move_to_end('james', False)
print(od1)  # OrderedDict([('james', 20), ('eden', 30), ('ethan', 31)])
```

## 출처
* 열혈 파이썬 - 윤성우
* [docs.python.org](https://docs.python.org/3/library/collections.html#collections.OrderedDict)
