---
title: "[컴퓨팅사고] 프로그래밍과 논리/수학"
categories:
  - Computational Thinking
read_time: false
tags:
comments:
  - true
toc: true
toc_sticky: true
---
## 서론
PS(Problem Solving)을 처음 접할 때 어려운 상황은 크게 두가지이다.

첫째, 프로그래밍 언어 문법과 라이브러리의 사용

둘째, 논리

첫번째의 경우는 훈련에 비례하여 실력이 올라가고, 훈련의 필요성에 대한 반감이 없다.

하지만 두번째 상황인 논리는 어려움이 많다.

__논리적으로 정확하게 확인하는 과정이__ 필요하다.

논리적으로 사고해야하는 문제를 일상 문제로 똑같이 옮기면 사람들은 훨씬 수월하게 풀 수 있다.

이때 많은 사람들은 논리를 사용한 것이 아닌, 직관을 사용한 것이다.

직관은 논리보다 익숙한 상황에서 빠르게 해결할 수 있지만, 논리적인 느낌이고, 익숙한 상황이기 때문에 강한 착각을 일으킬 수 있으며, 착각으로 인해 틀릴 수 있다는 것이다.

프로그래밍에서는 직관이 아닌 정확한 논리를 이용해서 풀어야 한다.

포스팅을 통해서 그런 논리적인 훈련을 하는 과정들을 올려보려 한다.

틀린 증명이나 틀린 풀이가 있을 수 있으니 그런 경우 댓글로 알려주시면 감사드리겠습니다.

## 문제 1. 다음을 명제식 형태로 쓰고 참인지 거짓인지 판단하시오

<i class="far fa-sticky-note"></i> **1번 :**  만약 0이 홀수라면, 미국에서 2080년 월드컵이 열린다.
{: .notice--primary}
{: .text-justify}


<i class="far fa-sticky-note"></i> **2번 :**  만약 1989382793827489가 Prime Number라면, 2는 짝수이다.
{: .notice--primary}
{: .text-justify}


![](/assets/img/ct/1.jpg) 

## 문제 2. p와 q가 명제이고, p->q가 거짓이라고 하자. 다음 명제식의 참 거짓은 어떻게 되는가?

<i class="far fa-sticky-note"></i> **1번 :**  ~p -> q
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **2번 :**  p V q
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **3번 :**  q -> p
{: .notice--primary}
{: .text-justify}

![](/assets/img/ct/2.jpg) 

## 문제 3. 다음 명제들의 역,이,대우를 쓰시오

<i class="far fa-sticky-note"></i> **1번 :**  만약 0이 홀수라면, 미국에서 2080년 월드컵이 열린다.
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **2번 :**  만약 19928394872181이 Prime Number라면, 2는 짝수이다.
{: .notice--primary}
{: .text-justify}

__1번__

* 역 : 미국에서 2080년 월드컵이 열린다면, 0은 홀수이다.
* 이 : 0이 짝수라면, 미국에서 2080년 미국에서 월드컵이 열리지 않는다.
* 대우 : 미국에서 2080년 월드컵이 열리지 않는다면, 0은 짝수이다.

__2번__

* 역 : 2가 짝수라면, 19928394872181은 Prime Number이다.
* 이 : 19928394872181이 Prime Number가 아니라면, 2는 홀수이다.
* 대우 : 2가 홀수라면, 19928394872181는 Prime Number가 아니다.

## 문제 4. 다음 명제식의 진리표를 만드시오.

<i class="far fa-sticky-note"></i> **1번 :**  p ∧ (q -> ~p)
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **2번 :**  (p ∧ ~q) -> r
{: .notice--primary}
{: .text-justify}

![](/assets/img/ct/3.jpg)

## 수학적 귀납법

### 수학적 귀납법의 기본형

P(1)이 참이고, P(n) -> P(n+1)이 참이면 P(n)은 모든 자연수 n에 대해서 참이다.

### 수학적 귀납법의 강한 형태

P(1)이 참이고, P(1) ∧ P(2) ∧ ... P(n) -> P(n+1)이 참이면 P(n)은 모든 자연수 n에 대해서 참이다.

## 증명 연습 1

<i class="far fa-sticky-note"></i> **Note :**  Trivial Proof : ∀x, P(x) -> Q(x)를 증명하려 할 때, Q(x)가 항상 참인 경우.
{: .notice--info}
{: .text-justify}

* 다음 명제를 증명하시오.

<i class="far fa-sticky-note"></i> **1번 :**  실수 x에 대해, 만약 x < -1 이면, x^2 + 1/4 > 0이다.
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **2번 :**  n이 홀수이면, 4n^3 + 6n^2 + 12는 짝수이다.
{: .notice--primary}
{: .text-justify}

![](/assets/img/ct/4.jpg)

## 증명 연습 2

<i class="far fa-sticky-note"></i> **Note :**  Vacuous Proof : ∀x, P(x) -> Q(x)를 증명하려 할 때, P(x)가 항상 거짓인 경우.
{: .notice--info}
{: .text-justify}

* 다음 명제를 증명하시오.

<i class="far fa-sticky-note"></i> **1번 :**  실수 x에 대해, 만약 2x^2 - 4x + 4 < 0 이면, x > 8 이다.
{: .notice--primary}
{: .text-justify}

<i class="far fa-sticky-note"></i> **2번 :**  4n^3 + 6n^2 + 11이 짝수이면 n이 홀수이다.
{: .notice--primary}
{: .text-justify}

![](/assets/img/ct/5.jpg)


## 출처

* [SW Expert Academy](https://swexpertacademy.com/main/learn/course/subjectList.do?courseId=AVuPCwCKAAPw5UW6&subjectId=AV1lGbkqAAQCFAb_)






