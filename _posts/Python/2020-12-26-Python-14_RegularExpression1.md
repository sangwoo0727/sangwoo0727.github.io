---
title: "[Python] 정규표현식 1. 기본 사용법"
categories:
  - Python
read_time: false
tags:
  - Python
toc: true
toc_sticky: true
---

> 회사 ojt 기간에 데이터 수집을 위해 크롤링을 하였는데, 정규표현식을 많이 사용해서, 이 기회에 정리를 해두려 한다.

## 정규표현식이란
* 정규표현식은 문자열을 처리하는 방법 중 하나로 특정한 조건의 문자를 검색하거나 치환하는 과정을 매우 간편하게 처리할 수 있도록 하는 수단이다.

### 1. 일대일 매칭되는 문자
* 정규표현식에서의 모든 문자는 일반 문자열 하나와 매칭된다.
* 단, 메타 문자는 제외된다.
* 예를 들어, a는 문자열에서 a와 매칭이 되고, b는 a와 매칭이 되지 않는다.

### 2. 메타 문자
* 내가 사용한 re 패키지에는 12개의 메타문자가 존재한다.

```
$ ( ) * + . ? [ \ ^ { |
```

[공식 문서](https://docs.python.org/3/library/re.html)

* 메타 문자는 예약어라고 생각하면 쉽다. 즉, 특수한 기능을 하는 문자이다.
* 메타 문자는 문자열의 하나의 문자와 매칭되지 않는다.
* 만약 문자열에 내가 찾고자하는 문자에 메타 문자가 있으면 어떡할까?

> 메타 문자에 백슬래시 \ 를 붙혀주면, 하나의 문자에 매칭된다. \+ 이 문자는 일반 문자열의 + 에 매칭이 된다.

* 추가적으로, 특수한 상황에서 메타문자 역할을 하는 문자들이 있다.
* 예를 들어, ], -, } 등이 있는데, 닫힌 괄호는 열린 괄호에 대응이 된다.


## re 패키지
* 파이썬에서는 정규표현식을 위해 re 패키지를 제공한다.
* re 패키지에서 제공하는 메소드들에 대해 다뤄보자.

### 1. re.match(pattern, string, flags)

```python
def match(pattern, string, flags=0):
    """Try to apply the pattern at the start of the string, returning
    a Match object, or None if no match was found."""
    return _compile(pattern, flags).match(string)
```

* 정의된 match 함수를 보면 __문자열의 처음부터 시작하여 패턴이 있는지를 확인한다.__
* 반환 값으로 Match object를 반환하고, 매치되는게 없을 경우 None을 반환한다.

```python
import re

print(re.match('ab', 'abcabc'))  # <re.Match object; span=(0, 2), match='ab'>
print(re.match('ab', 'bcabc'))  # None
```

* 결과를 print로 바로 출력하지는 않고, 보통은 match object를 활용한다고 한다.
* 이 부분에 대해서는 추후에 다시 다루겠다.

### 2. re.search(pattern, string, flags)

```python
def search(pattern, string, flags=0):
    """Scan through string looking for a match to the pattern, returning
    a Match object, or None if no match was found."""
    return _compile(pattern, flags).search(string)
```

* search 함수는 match와 다르게 __반드시 문자열의 처음부터 일치해야되는 것은 아니다.__
* 중간부터 일치하는 패턴이 있어도 관계없다.

```python
print(re.search('ab', 'bcabc'))  # <re.Match object; span=(2, 4), match='ab'>
print(re.search('ab', 'bcacc'))  # None
```

* 결과를 보면 span=(2,4) 가 있는 것을 알 수 있는데, 2번 인덱스부터 4번 인덱스 전까지 일치하는 문자패턴이 있다는 것을 의미한다.
* 즉, match 함수의 경우는 span의 결과가 항상 0부터 시작할 것이다.

### 3. re.findall(pattern, string, flags)

```python
def findall(pattern, string, flags=0):
    """Return a list of all non-overlapping matches in the string.

    If one or more capturing groups are present in the pattern, return
    a list of groups; this will be a list of tuples if the pattern
    has more than one group.

    Empty matches are included in the result."""
    return _compile(pattern, flags).findall(string)
```

* findall 함수는 문자열 중, 패턴과 일치하는 모든 부분을 찾는다.
* 함수 설명을 보면 __Return a list of all non-overlapping matches in the string.__ 이라고 쓰여있다.
* 즉, 반환되는 리스트는 서로 겹치지 않는다.

```python
print(re.findall('ab', 'abcabcabc'))  # ['ab', 'ab', 'ab']
print(re.findall('aaa', 'aaaa'))  # ['aaa']
```

* 반환되는 리스트는 서로 겹치지 않으므로, 두번째 예제 같은 경우에서 리스트는 한개만 반환이 되었다.

### 4. re.finditer(pattern, string, flags)

```python
def finditer(pattern, string, flags=0):
    """Return an iterator over all non-overlapping matches in the
    string.  For each match, the iterator returns a Match object.

    Empty matches are included in the result."""
    return _compile(pattern, flags).finditer(string)
```

* findall과 같은 규칙으로 찾지만, 일치된 문자열 리스트 대신, matchObj 리스트를 반환한다.


```python
print(re.finditer('ab', 'abcabcabc'))  # <callable_iterator object at 0x7fb35e169760>

matchIter = re.finditer('ab', 'abcabcabc')

for matchObj in matchIter:
    print(matchObj)

# <re.Match object; span=(0, 2), match='ab'>
# <re.Match object; span=(3, 5), match='ab'>
# <re.Match object; span=(6, 8), match='ab'>
```

### 5. re.fullmatch(pattern, string, flags)

```python
def fullmatch(pattern, string, flags=0):
    """Try to apply the pattern to all of the string, returning
    a Match object, or None if no match was found."""
    return _compile(pattern, flags).fullmatch(string)
```

* fullmatch 함수는 패턴과 문자열이 완벽히 일치하는 지를 검사한다.
* 즉 같은 문자열인지 확인한다.

```python
print(re.fullmatch('aa', 'aaa'))  # None
print(re.fullmatch('aa', 'aa'))  # <re.Match object; span=(0, 2), match='aa'>
```

### match object의 메소드
* re의 함수의 반환값으로 match object가 많이 반환된다.
* match object를 그대로 print해서 사용하지는 않고, 주로 match object가 제공하는 메소드를 사용한다.

> group() : 일치된 문자열을 반환한다.

> start() : 일치된 문자열의 시작 위치 인덱스를 반환한다.

> end() : 일치된 문자열의 끝 위치 다음 인덱스를 반환한다.

> span() : 일치된 문자열의 시작위치, 끝위치 튜플을 반환한다.

```python
match_obj = re.search('ab','jqieuabcd')

print(match_obj.group())  # ab
print(match_obj.start())  # 5
print(match_obj.end())  # 7
print(match_obj.span())  # (5,7)
```

## 출처
* [Gorio Learning](https://greeksharifa.github.io/)

