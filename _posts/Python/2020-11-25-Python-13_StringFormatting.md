---
title: "[Python] String Formatting"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---
* String Formatting은 문자열을 더 잘 표현하기 위해 편집하듯이 하나의 문자열을 구성해 내는 것을 말한다.
* 파이썬에서는 이러한 String Formatting 방법이 몇가지 있다.
* 이에 대해 이번 포스팅에서 소개해보려 한다.

## String Formatting을 사용하지 않을 때
* 물론 String Formatting을 사용하지 않고도 충분히 코드를 작성할 수 있다.
* 하지만, 직관적이지 않고 때로는 불편할 수 있다.

```python
members = [('james', 'male', 30), ('eden', 'male', 30), ('jessie', 'female', 3), ('laura', 'female', 3)]

for member in members:
    print('내 이름은 ', member[0], '. 나는 ', member[1], '이고 나이는 ',member[2])

```

## printf-style String Formatting
* % 연산자를 사용하는 String Formatting이다.
* c언어에서 사용되는 방법이므로, c언어 스타일이나 printf 스타일이라고 부른다.
* 코드가 지저분해보일 수 있다.
* 파이썬 초기에 사용되던 방식이다.

```python
members = [('james', 'male', 30), ('eden', 'male', 30), ('jessie', 'female', 3), ('laura', 'female', 3)]

for member in members:
    print('내 이름은 %s. 나는 %s이고 나이는 %d' % member)
```

* 뒤에 오는 값은 %s, %d와 같은 conversion specifier에 맞게 튜플형태로 제공해야 한다.
* 물론 풀어서 넣어도 된다.
* 튜플 말고 딕셔너리 형태로 출력 대상을 지정하는 방법도 존재한다.

```python
members = [('james', 'male', 30), ('eden', 'male', 30), ('jessie', 'female', 3), ('laura', 'female', 3)]

for member in members:
    d = dict(name=member[0], s=member[1], age=member[2])
    print('내 이름은 %(name)s. 나는 %(s)s이고 나이는 %(age)d' % d)
```

## '{}'.format 방식
* 메소드 호출 방식이라고 하며, str.format() 이라고도 한다.
* 조금 더 파이썬스러운 방법.
* 중괄호에 들어갈 출력 대상을 .format 메소드 안에 입력한다.

```python
members = [('james', 'male', 30), ('eden', 'male', 30), ('jessie', 'female', 3), ('laura', 'female', 3)]

for member in members:
    print('내 이름은 {0}. 나는 {1}이고 나이는 {2}'.format(*member))
```

* 메소드를 호출하는 방식이므로, 위와 같이 언패킹 방식으로 전달할 수 있다.
* {}안에 숫자는 쓰지 않아도되지만, 만약 출력 대상 순서를 변경하고 싶다면 숫자를 바꿔주면 된다.

## f-Strings
* 새로운 String Formatting 방식이다.
* 메소드 호출방식과 사용 방식이 비슷하지만 조금 더 깔끔하다.
* 변수를 먼저 만든 후, 중괄호에 변수명을 넣는다.
* 문자열 앞에는 f(F)를 입력해야 한다.

```python
members = [('james', 'male', 30), ('eden', 'male', 30), ('jessie', 'female', 3), ('laura', 'female', 3)]

for member in members:
    name = member[0]
    s = member[1]
    age = member[2]
    print(f'내 이름은 {name}. 나는 {s}이고 나이는 {age}')
```

* 중괄호 안에는 문자, 숫자 뿐만 아니라 함수 등 다양한 타입을 넣을 수 있다.

```python
upper_name = 'EDEN KANG JAMES'

def f(name):
    return name.lower()

print(f'lower_name 은 {f(upper_name)}')
```

## 출처
* 열혈 파이썬 - 윤성우
* [docs.python.org](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)