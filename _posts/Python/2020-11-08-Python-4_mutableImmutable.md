---
title: "[Python] Mutable vs Immutable"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 객체가 지닌 값의 수정이 불가능한 immutable 객체와 수정이 가능한 mutable 객체에 대해 알아보겠다.

## mutable과 immutable
* 파이썬은 모든 것이 객체(Object)인데, 그 속성이 mutable과 immutable로 구분된다.
* 객체가 지닌 값의 수정이 불가능한 객체를 가리켜 __immutable 객체__ 라고 하며, 대표적으로 튜플(tuble), 문자열(string), 숫자(number) 등이 있다.
* 값의 수정이 가능한 객체를 가리켜 __mutable 객체__ 라고 하며, 대표적으로 리스트(list), 딕셔너리(dictionary) 등이 있다.

## immutable 객체에 대한 확인
* 먼저 숫자를 확인해보자.

```python
x = 1
y = x
y += 2
print(x) # 1
print(y) # 3
```

* y = x 라인에서 1이라는 동일한 객체를 x와 y가 가리키고 있다.
* y += 2 로 변경하는 순간, y는 3을, x = 1을 가리키게된다. 
* y = x인 시점에서는 동일한 객체를 가리키다가, immutable 타입인 y를 변경하였을 때 변경이 된다.
* 확인을 해보자.

```python
x = 1
y = x
print(id(1))  # 4506704224
print(id(x))  # 4506704224
print(id(y))  # 4506704224
y += 2
print(id(x))  # 4506704224
print(id(y))  # 4506704288 , id값이 변한다.
```

* 튜플 역시 값을 수정할 수 없다.
* 튜플에 저장된 값을 수정하면, 새로운 튜플이 만들어진다.

```python
t = (1, 2)
print(id(t))  # 140381802534656

t += (3, 4)
print(t)  # (1, 2, 3, 4)
print(id(t))  # 140381820007344
```

## mutable 객체에 대한 확인
* mutable 타입은 쓰기가 가능한 컨테이너로 이해할 수 있다.
* 튜플의 경우 읽기만 가능한 컨테이너이기 때문에 immutable이다.
* mutable 객체의 대표적인 타입인 list를 통해 확인을 해보자.

```python
r = [1, 2]
print(id(r))  # 140726858462336

r += [3, 4]
print(r)  # [1, 2, 3, 4]
print(id(r))  # 140726858462336
```

* 값을 수정하여도, id가 변하지 않는 것을 알 수 있다.

```python
r = [1, 2, 3]
l = r
print(id(l))  # 140559750083712
print(id(r))  # 140559750083712
l += [4]
print(l)  # [1, 2, 3, 4]
print(r)  # [1, 2, 3, 4]
print(id(l))  # 140559750083712
print(id(r))  # 140559750083712
```

* 두번째 줄까지 실행을 하면, l과 r은 모두 [1,2,3]을 가리키게 된다.
* 이 후, l을 수정하면 r 역시도 변하게 된다.
* 쓰기 가능한 컨테이너는 mutable이기 때문에, shallow copy가 되는 것을 알 수 있다. 
* 깊은 복사와 얕은 복사에 대해서는 다음 포스팅에서 다루겠다.


## 출처
* 열혈 파이썬 - 윤성우
* [견우와 직녀](https://ledgku.tistory.com/54)

