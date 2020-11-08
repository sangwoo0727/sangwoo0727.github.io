---
title: "[Python] 깊은 복사 vs 얕은 복사"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* 이번 포스팅에서는 깊은 복사와 얕은 복사에 대해 알아보겠다.

## 두 객체를 비교할 때 사용하는 연산자
* 파이썬은 두 객체를 비교할 때 사용하는 연산자가 2가지 있다.
* __v1 == v2__ -> v1과 v2가 참조하는 객체의 내용이 같은가?
* __v1 is v2__ -> v1과 v2가 참조하는 객체는 동일한 객체인가?
* 예를 들어보자.

```python
r1 = [1, 2, 3]
r2 = [1, 2, 3]

print(r1 is r2)  # r1과 r2가 참조하는 객체는 같은 객체인가? False
print(r1 == r2)  # r1과 r2가 참조하는 객체에 담긴 값이 같은가? True

r1 = r2
print(r1 is r2)  # True
```

* 동일한 값을 담고있는 두 객체를 생성하여 비교를 할 경우, 둘은 참조하는 객체는 다르다.
* 하지만 r2가 참조하는 리스트에 r1이라는 이름을 하나 더붙힐 경우 둘은 같은 객체를 참조하게 된다.
* 이번에는 조금 더 응용하여 보자.

## 얕은 복사(Shallow Copy)

```python
user1 = ['james', ('male', 'korea'), [180,80]]
user2 = list(user1)

print(user1 is user2)  # False
print(user1[0] is user2[0])  # True
print(user1[1] is user2[1])  # True
print(user1[2] is user2[2])  # True
```

* 유저정보를 담은 리스트를 생성하고, list 함수를 통해, user2에 같은 리스트를 전달하였다.
* 이렇게되면 같은 내용이 담긴 리스트가 user2에 전달되게 된다.
* 결과를 보면 user1과 user2가 참조하는 객체는 다르지만, 하나하나의 원소들에 대해서는 참조하는 객체가 같다는 결과가 나온다.
* 왜그럴까?
* 리스트를 생성하게 되면, 리스트 안에 선언된 값들이 리스트에 들어가있는 형태가 아니라, 각 값들을 리스트 내에서 참조하는 형태가 된다.

![](/assets/img/python/20201108_1.png)

* 이렇게 생성된 객체에 대해서 list함수를 호출하며 user2 에 리스트를 전달하면, 아래와 같이 새로운 리스트가 만들어진다.

![](/assets/img/python/20201108_2.png)

* 이러한 형태의 복사를 가리켜 __얕은 복사(shallow copy)__ 라고 한다.
* 리스트는 별도로 생성하지만, 그 안에 들어가는 내용은 원래와 같은 객체라는 것이다.
* 앝은 복사는 파이썬이 복사를 진행하는 기본 방식이다.
* 문자열 객체와 튜플 객체 등의 immutable 객체는 얕은 복사를 하는 것이 더 합리적이다(값의 변경이 불가능하기 때문에)
* 하지만 [180,80]인 내부 객체를 대상으로 얕은 복사를 하는 것은 문제가 될 수 있다.
* 리스트는 mutable하기 때문에, 그 안에 담긴 값을 수정할 수 있기 때문이다.
* mutable한 내부객체의 얕은 복사 문제를 해결하기 위해서는 깊은 복사(deep copy)를 진행해야 한다.

## 깊은 복사(Deep Copy)
* 위의 코드를 통해 예시를 보자.

```python
user1 = ['james', ('male', 'korea'), [180,80]]
user2 = list(user1)

user2[2][1] += 5
print(user2) # ['james', ('male', 'korea'), [180, 85]]
print(user1) # ['james', ('male', 'korea'), [180, 85]]
```
* user2는 james의 1년뒤 모습을 담은 객체라고 할 때, 몸무게가 5가 늘어나 user2정보에 +5한 값을 저장했다고 하자.
* 이 때, user1 정보는 현재 james의 모습이고 몸무게는 80 그대로가 출력되어야 한다.
* 하지만 얕은 복사로 인하여 user1의 정보 역시 변하게 되었다.
* 결론을 내리면, mutable한 객체에 저장된 값들은 변경될 수 있어서 이들에 대해서는 복사 대상을 하나 더 생성하는 깊은 복사를 하는 것이 바람직하다.

![](/assets/img/python/20201108_3.png)

* 깊은 복사를 구현하기 위해서는 copy 모듈의 deepcopy 함수를 사용한다.
* 참고로, 얕은 복사를 위해서 copy 모듈의 copy 함수를 사용할 수 있다.

```python
import copy

user1 = ['james', ('male', 'korea'), [180,80]]
user2 = copy.deepcopy(user1)

user2[2][1] += 5
print(user1)  # ['james', ('male', 'korea'), [180, 80]]
print(user2)  # ['james', ('male', 'korea'), [180, 85]]

print(user1[0] is user2[0])  # True
print(user1[1] is user2[1])  # True
print(user1[2] is user2[2])  # False
```

## 정리
* 단순 복제는 완전히 동일한 객체이다.
* 얕은 복사는 껍데기만 복사, 내용은 같은 객체를 참조
* 깊은 복사는 내용 역시 복사를 하게 된다.

## 참조
* 열혈 파이썬 - 윤성우
* [파이썬에서의 복사 얕은 복사와 깊은 복사](https://pinkwink.kr/1234)