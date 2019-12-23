---
title: "[라즈베리파이] 기본예제. LED 동작 구현"
categories:
  - RaspberryPi
tags:
  - RaspberryPi
comments:
  - true
toc: true
toc_sticky: true
---

## [라즈베리파이] 기본예제. LED 동작 구현

* 라즈베리파이로 프로젝트를 진행하기 위해서 기본예제들을 공부중이다.
* 제일 먼저 설치 후에 하드웨어적인 부분에 대한 지식이 없어서 실습과 공부를 할 겸 LED를 동작시켜보았다.
* 먼저 실습을 할 디렉토리로 이동한 후, 코드를 작성하기위해 nano 명령어를 이용하여 파일을 만든다.
 
![](/assets/img/Rasp/08301.png)

* 파이썬 코드를 이용하였기때문에 파일의 확장자는 .py가 된다.
* gpio_test1.py의 코드는 아래와 같다.

```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)

print "Setup LED with GPIO"

GPIO.setup(23, GPIO.OUT)
GPIO.setup(24, GPIO.OUT)

try:
    while True:
        GPIO.output(23, True)
        time.sleep(1)
        GPIO.output(23, False)
        time.sleep(1)
        GPIO.output(24, True)
        time.sleep(1)
        GPIO.output(24, False)
        time.sleep(1)
except KeyBoardInterrupt:
    GPIO.cleanup()

```
* 코드는 어렵지는 않다. RPi.GPIO 라이브러리를 import 시킨 후, 실습을 하는 과정에서 BCM의 23, 24번 포트에 연결하였다.
* 23번 포트에 연결된 다이오드를 켜준후, 1초후 꺼준다.
* 마찬가지로 24번 포트에 연결된 다이오드를 켜준 후, 1초후 끈다. 
* 코드를 작성한 후, 실행은 sudo python gpio_test1.py 명령어를 통해 실행한다.
* 아래 동영상은 시현영상이다.
  
{% include 0830.html id="https://www.youtube.com/watch?v=GCSLqmMmUZ0" %}

