<<<<<<< HEAD
---
title: "[Java] Timer, TimerTask 클래스를 이용한 스케쥴링"
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

JDK 1.3에 추가되었던 java.util.Timer 클래스와 java.util.TimerTask 클래스를 이용하면 백그라운드에서 특정한 시간 또는 일정 시간을 주기로 반복적으로 특정 작업을 실행할 수 있도록 해준다.

먼저, Timer 클래스에 대해 알아보자.

## java.util.Timer 클래스

Timer 클래스는 아래와 같이 3가지의 메소드를 제공한다.

* schedule
* scheduleAtFixedRate
* cancel

이 중, schedule 메소드는 4가지 형태로 오버로딩 되어있다.

__void schedule(TimerTask task, Date time)__ - 설정한 time 시간에, 설정한 task 작업을 수행한다.

__void schedule(TimerTask task, Date firstTime, long period)__ - firstTime 부터 period 간격으로 task 작업을 수행한다.

__void schedule(TimerTask task, long delay)__ - delay 시간이 지난 후에, task를 수행한다.

__void schedule(TimerTask task, long delay, long period)__ - dealy 시간이 지난 후에, period 간격으로 task를 수행한다.

이 중에서, 설정한 시간에 task를 수행하는 위의 두 메소드는 만약 지정한 시간이 현재 시간보다 이전일 경우 바로 task 작업을 수행하게 된다.

나머지 두개의 메소드는 fixed-delay 방식으로 작업을 진행하게 되는데, fixed-delay 방식은 선행 작업이 지연될 경우, 다음에 수행되는 작업 역시 그 시간만큼 지연되는 방식을 말한다.

scheduleAtFixedRate 메소드는 fixed-delay 방식이 아니므로, 정확학게 일정 시간 간격으로 작업을 실행해야 할 때 적합하다.

즉, scheduleAtFixedRate() 메소드는 fixed-rate 방식을 사용하는데, 선행 작업의 지연 유무에 상관없이 지정된 시간에 작업을 실행한다.

두가지 형태로 오버로딩 되어있다.

__void scheduleAtFixedRate(TimerTask task, Date firstTime, long period)__ - 지정한 시간부터 일정한 간격으로 task를 수행한다.

__void scheduleAtFixedRate(TimerTask task, long delay, long period)__ - 일정한 시간(delay)가 지난 후 일정한 간격으로 task를 수행한다.

cancel() 메소드는 Timer를 중지시키며, 실행 중인 task 작업을 제외한 예정된 작업들은 모두 취소한다.

## java.util.TimerTask 클래스

TimerTask 클래스는 Timer 클래스가 수행할 작업을 나타낸다.

TimerTask는 Runnable 인터페이스를 구현하고 있다.

TimerTask가 제공하는 메소드를 보자.

__boolean cancel()__ - TimerTask 작업을 취소한다.

__abstract void run()__ - TimerTask가 실행할 작업

__long scheduledExecutionTime()__ : 가장 최근에 작업이 실행된 시간을 리턴한다.



```java
import java.util.Timer;
import java.util.TimerTask;

public class Main {
    public static void main(String[] args) {
        Timer timer = new Timer();
        TimerTask timerTask = new TimerTask() {
            int cnt = 0;
            @Override
            public void run() {
                if(cnt++ < 5){
                    System.out.println("task...");
                }else{
                    timer.cancel();
                }
            }
        };
        timer.schedule(timerTask,1000,1000);
    }
}
```

이 코드는 프로그램이 실행된 후, 1초 뒤 1초마다 5번 만큼 task...라는 문장을 print하는 코드이다.

다만, Timer 클래스의 기본 생성자를 사용하여 Timer 클래스를 생성하면, 데몬 스레드로 실행되지 않는다.

데몬 스레드로 지정하고싶은 경우는 생성자를 생성할 때, boolean 인자 필요로 하는 생성자로 입력받으면 된다.

```java
Timer timer = new Timer(true);
```

## 참조
__https://docs.oracle.com/javase/8/docs/api/java/util/Timer.html__

__https://promobile.tistory.com/105__


=======
---
title: "[Java] Timer, TimerTask 클래스를 이용한 스케쥴링"
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

JDK 1.3에 추가되었던 java.util.Timer 클래스와 java.util.TimerTask 클래스를 이용하면 백그라운드에서 특정한 시간 또는 일정 시간을 주기로 반복적으로 특정 작업을 실행할 수 있도록 해준다.

먼저, Timer 클래스에 대해 알아보자.

## java.util.Timer 클래스

Timer 클래스는 아래와 같이 3가지의 메소드를 제공한다.

* schedule
* scheduleAtFixedRate
* cancel

이 중, schedule 메소드는 4가지 형태로 오버로딩 되어있다.

__void schedule(TimerTask task, Date time)__ - 설정한 time 시간에, 설정한 task 작업을 수행한다.

__void schedule(TimerTask task, Date firstTime, long period)__ - firstTime 부터 period 간격으로 task 작업을 수행한다.

__void schedule(TimerTask task, long delay)__ - delay 시간이 지난 후에, task를 수행한다.

__void schedule(TimerTask task, long delay, long period)__ - dealy 시간이 지난 후에, period 간격으로 task를 수행한다.

이 중에서, 설정한 시간에 task를 수행하는 위의 두 메소드는 만약 지정한 시간이 현재 시간보다 이전일 경우 바로 task 작업을 수행하게 된다.

나머지 두개의 메소드는 fixed-delay 방식으로 작업을 진행하게 되는데, fixed-delay 방식은 선행 작업이 지연될 경우, 다음에 수행되는 작업 역시 그 시간만큼 지연되는 방식을 말한다.

scheduleAtFixedRate 메소드는 fixed-delay 방식이 아니므로, 정확학게 일정 시간 간격으로 작업을 실행해야 할 때 적합하다.

즉, scheduleAtFixedRate() 메소드는 fixed-rate 방식을 사용하는데, 선행 작업의 지연 유무에 상관없이 지정된 시간에 작업을 실행한다.

두가지 형태로 오버로딩 되어있다.

__void scheduleAtFixedRate(TimerTask task, Date firstTime, long period)__ - 지정한 시간부터 일정한 간격으로 task를 수행한다.

__void scheduleAtFixedRate(TimerTask task, long delay, long period)__ - 일정한 시간(delay)가 지난 후 일정한 간격으로 task를 수행한다.

cancel() 메소드는 Timer를 중지시키며, 실행 중인 task 작업을 제외한 예정된 작업들은 모두 취소한다.

## java.util.TimerTask 클래스

TimerTask 클래스는 Timer 클래스가 수행할 작업을 나타낸다.

TimerTask는 Runnable 인터페이스를 구현하고 있다.

TimerTask가 제공하는 메소드를 보자.

__boolean cancel()__ - TimerTask 작업을 취소한다.

__abstract void run()__ - TimerTask가 실행할 작업

__long scheduledExecutionTime()__ : 가장 최근에 작업이 실행된 시간을 리턴한다.



```java
import java.util.Timer;
import java.util.TimerTask;

public class Main {
    public static void main(String[] args) {
        Timer timer = new Timer();
        TimerTask timerTask = new TimerTask() {
            int cnt = 0;
            @Override
            public void run() {
                if(cnt++ < 5){
                    System.out.println("task...");
                }else{
                    timer.cancel();
                }
            }
        };
        timer.schedule(timerTask,1000,1000);
    }
}
```

이 코드는 프로그램이 실행된 후, 1초 뒤 1초마다 5번 만큼 task...라는 문장을 print하는 코드이다.

다만, Timer 클래스의 기본 생성자를 사용하여 Timer 클래스를 생성하면, 데몬 스레드로 실행되지 않는다.

데몬 스레드로 지정하고싶은 경우는 생성자를 생성할 때, boolean 인자 필요로 하는 생성자로 입력받으면 된다.

```java
Timer timer = new Timer(true);
```

## 참조
__https://docs.oracle.com/javase/8/docs/api/java/util/Timer.html__

__https://promobile.tistory.com/105__


>>>>>>> 35fccfb3e8cf637223c060518d7872b8bad6f7d7
