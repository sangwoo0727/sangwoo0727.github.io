---
title: "[라즈베리파이] 기본예제. picamera를 이용한 사진찍기"
categories:
  - RaspberryPi
tags:
  - RaspberryPi
comments:
  - true
---

## [라즈베리파이] 기본예제. picamera를 이용한 사진찍기
* 카메라 모듈을 이용하여 라즈베리파이에선 사진을 찍을 수 있다.
* 여기서 말하는 카메라 모듈을 라즈베리파이 전용으로 나온 Pi Camera 모듈이다.

### 카메라 모듈을 사용하기 위한 전처리 작업.
* sudo apt-get update
* sudo apt-get upgrade
* 두개의 명령어를 통해 라즈베리 파이의 패키지들을 업데이트를 한번 해준다.
* sudo apt-get install python-picamera 명령어를 통해 패키지를 확인한다. 이미 있으면 already.. 출력문이 나올 것.
* 그 후 sudo raspi-config 명령어를 입력하여 카메라를 켜주는 옵션을 설정한다.

### 기본적인 촬영 방법
* raspistill -v -o test.jpg 를 입력하면, 현재 실행한 디렉토리에 test.jpg라는 파일로 사진이 찍힐 것이다.
* raspivid -o video.h264 명령어를 통해서는, 현재 실행한 디렉토리에 video.h264라는 파일로 동영상이 찍힐 것이다. 다른 명령어를 입력하지 않으면 default 값으로 5초간 촬영된다.
* raspivid -o video.h264 -t 10000 처럼 뒤에 명령어를 추가하면 그 시간동안 촬영이 진행된다. -t 뒤에 나오는 숫자는 단위가 millisecond로 10000을 입력하면 10초간 촬영이 되는 것을 의미한다.

### 파이썬을 통한 카메라 제어
* 카메라 미리보기 기능을 위해 파이썬 코드를 작성해본다.

```python

# nano picam_oneshot.py
# 한장만 사진 촬영하는 파이썬 코드


import picamera #라이브러리 임포트
import time
import datetime

with picamera.PiCamera() as camera: #파이카메라 객체 생성
        camera.resolution = (1024,768) #카메라 화질 설정
        now = datetime.datetime.now()
        filename = now.strftime('%Y-%m-%d %H:%M:%S') #파일이름 날짜를 통해 설정하기
        camera.start_preview() #프리뷰 보여주기 시작
        time.sleep(10)
        camera.stop_preview() #프리뷰 멈추기
        camera.capture(filename + '.jpg') #사진 촬영 후 파일명으로 저장.


```

* python picam_oneshot.py 명령어를 실행시키면 카메라가 작동할 것.
* 단 카메라 미리보기 기능은 라즈베리파이가 모니터에 직접 연결될 때에만 동작한다. 원격연결이 된 경우 카메라 미리보기 기능이 작동하지 않는다.

### 파이썬 코드를 이용한 동영상 촬영 및 제어
* 위와 거의 같다.
  
```python
# nano picam_record.py
# 동영상 촬영하는 파이썬 코드

import picamera
import time
import datetime

with picamera.PiCamera() as camera:
        camera.resolution = (640,480)
        now = datetime.datetime.now()
        filename = now.strftime('%Y-%m-%d %H:%M:%S')
        camera.start_recording(output = filename + '.h264') #동영상 촬영 시작
        camera.wait_recording(5) #5초간 기다린다
        camera.stop_recording() #동영상 촬영 종료

```

* 역시 python picam_record.py를 통해 명령어를 실행시킨다.


![](/assets/img/Rasp/09031.png)

* 생각보다 화질이 좋다.
