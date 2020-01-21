---
title: "[Java] MacOS, JDK 설치하기"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
toc: true
toc_sticky: true
---
## Install JDK
Java개발을 위해서는 JDK가 필요한데, MacOS에서 JDK를 설치하는 방법을 포스팅하려 한다.

설치하는 시점에 따라 버전이 다를 수 있으니 이 부분은 버전에 맞게 설치하면 된다.

## Oracle
JDK는 Oracle 홈페이지에서 다운을 받으면 된다.

[Oracle에서 JDK설치하기](https://www.oracle.com/technetwork/java/javase/overview/index.html)

다운로드하려는 JDK버전을 선택하면, 아래와 같은 곳에서 동의를 누르고, MacOS용으로 다운로드를 받는다.

이 포스팅에서는 .dmg로 받았다.

![](/assets/img/java/20200107_1.png)

설치파일을 다운받고나면, 실행을 시키고 계속 설치->계속을 누른다.

설치과정은 매우 간단하다.

## 설치 확인
JDK가 제대로 설치되었는지 확인을 해보자.

터미널을 열어, 아래와 같이 입력을 하고 자바 버전이 나오는지 확인하자.

![](/assets/img/java/20200107_2.png)

위와 같이 자바의 버전이 출력된다면 정상적으로 설치된 것을 알 수 있다.

실행이 만약 안된다면, 환경변수 설정을 해줘야하는데, 포스팅 아래부분에서 다루겠다.

## 설치 파일 위치
설치한 파일은 아래와 같은 경로에 존재한다.

직접 들어가서 확인해보자.

![](/assets/img/java/20200107_3.png)

## 환경변수 설정
환경변수 설정을 위해서는 /etc/profile 파일에 아래와 같은 환경변수를 추가해준다.

![](/assets/img/java/20200107_4.png)

이와 같이 입력하고, 터미널을 껐다키면 java --version이 정상적으로 동작하는 것을 알 수 있다.

## 자바코드 작성해보기

vi Test.java 명령어로 자바 파일을 생성해보자.

그 후, 아래와 같은 소스코드를 작성한다.

```java
public class Test { 
    public static void main(String[] args) {
         System.out.println("Hello Java!"); 
    } 
}
```

컴파일은 javac [파일명].java 로 컴파일한다.
실행은 java [파일명] 명령어로 한다.(.class는 붙히지 않는다.)

![](/assets/img/java/20200107_5.png)

이렇게 실행이 되는 것을 알 수 있다!!


