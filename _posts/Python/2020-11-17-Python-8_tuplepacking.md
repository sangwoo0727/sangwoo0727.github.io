---
title: "[Python] 튜플의 패킹(packing)과 언패킹(unpacking)"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
## 패킹과 언패킹
* 패킹은 말그대로 묶는다는 뜻이고, 언패킹은 묶여있는 것을 풀어내는 뜻이다.
* 튜플로 값을 묶는 것을 튜플 패킹,
* 튜플로 묶여있는 값을 풀어내는 것을 튜플 언패킹이라한다.

```python
human = (180,75) # 사람의 키와 몸무게로 튜플을 묶으면 튜플 패킹
print(human) # (180, 75)
human = 180,75 # 튜플 패킹은 소괄호가 없어도 된다.
print(human) # (180, 75)
```

* 튜플 패킹은 간단하다.
* 언패킹도 간단하다. 아래 예시를 보자.

```python
height, weight = human # 튜플 언패킹
print(height, weight) # 180 75
```

* 언패킹의 과정에서 둘 이상의 값을 리스트로 묶는 것도 가능하다.
* 중요한 점은, 튜플이 아닌 리스트로 묶인다는 사실이다.
* 아래 예시를 보자.

```python
num = 1, 2, 3, 4, 5, 6, 7
n1, n2, *others = num

print(n1)  # 1
print(n2)  # 2
print(others)  # [3,4,5,6,7]
```

* 맨 뒤쪽에 있는 값들만 리스트로 묶을 수 있는 것이 아니라, 중간 값이나 앞에 위치한 값들도 묶을 수 있다.
* 단 일부를 리스트로 묶을 때는, * 를 사용한다.
* 참고로, 리스트 역시 언패킹이 가능하다.

```python
num = [1, 2, 3]
n1, n2, n3 = num
print(n1) # 1
```

## 함수에서의 패킹,언패킹
* 함수의 호출, 반환에서도 튜플 패킹과 언패킹을 할 수 있다.

```python
def make_tuple():
    return 1, 2, 3

nums = make_tuple()
print(nums)  # (1,2,3)

n1, *others = make_tuple() # 반환되는 튜플을 언패킹하여 저장할 수도 있다.
print(n1) # 1
print(others) # [2, 3]
```

* 지금까지 *를 사용하면 남은 값들을 리스트로 패킹하는 형태로 사용되었다.
* 하지만 *는 사용위치에 따라 튜플 패킹이나 언패킹으로 쓰일 수 있다.

```python
def sum(*others):
    print(others) # (1,2,3,4,5) -> 튜플 패킹

sum(1, 2, 3, 4, 5)

def show(height, weight):
    print(height)
    print(weight)

human = 180,75
show(*human) # human에 담긴 값을 언패킹하여 각각 매개변수에 전달한다.
```

## 중첩 튜플의 패킹, 언패킹
* 튜플안에 튜플이 존재하는 중첩 튜플에서 패킹, 언패킹은 어떻게 진행할까?
* 모양 그대로 하면된다.
* 바로 예시로 살펴보자.

```python
tu = ((1, 2), 3, (4, 5))
(t1, t2), t3, (t4, t5) = tu
print(t1, t2, t3, t4, t5) # 1 2 3 4 5
```

## 언패킹의 관례
* 언패킹 과정에서, 모든 값이 필요하지 않고 필요한 정보만 사용하고 싶을 땐 어떻게 할까?
* 관례적으로 사용하지 않는 값들에 대해서는 _ 변수에 담는다.

```python
human = ('james', 180, 75, 20) # 순서대로 이름, 키, 몸무게, 나이라고 하자

_, _, _, age = human # 나이만 변수에 담고싶을 때는 관례적으로 이렇게 한다.
print(age) # 20
```

* 물론 _ 역시도 변수이지만, 잘쓰지 않는 형태이기때문에 관례적으로 이런 방식을 사용한다.

## for 루프에서의 언패킹
* for 루프에서도 역시 언패킹을 할 수 있다.

```python
data = [('james',20),('john',21),('peter',15)]

for name,age in data:
    print(name, age)
```

## 출처




