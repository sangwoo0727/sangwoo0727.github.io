---
title: "[알고리즘설계] Log"
categories:
  - Algorithm
tags:
  - Algorithm
  - Log
comments:
  - true
---

## [알고리즘설계] Log

### 기본 로그관련

* log(base a)xy = log(base a)x + log(base a)y

* log(base a)b = log(base c)b / log(base c)a

* log(base a)n^b = blog(base a)n

* log(base 2)2^n = n

* n개의 수를 표현하기 위한 비트 수 = log(base 2)n

* 완전 이진 트리의 높이는 log(base 2) n

### 로그의 다양한 활용 -1 : a^n 빨리 계산하기

* 실제로 a^n을 계산하려면 a 를 n번 곱하는 연산횟수가 필요하다.

* (a^n/2)^2 로 나누어 계산과정을 반복한다.(n이 홀 수 일땐 a를 한번 더 곱한다.)

* ex) a^17 = (a^8)^2 * a = ((a^4)^2)^2 * a = (((a^2)^2)^2)^2 * a

* 즉 log(base 2)17 인 대략 4번 정도면 계산을 할 수 있다.

### Harmonic Number 구하기

* 이 부분은 수식과 그래프가 많아서 따로 한글파일로 정리한 사진을 첨부하겠다.

![](/assets/img/Algorithm/HarmonicNum.png)


### log(base2)n! 구하기

* 역시 한글파일로 정리한 사진을 첨부하겠다.

![](/assets/img/Algorithm/nfac.png)

---


* 틀린 부분이 존재할 수 있으니 참고해주시기 바랍니다.
