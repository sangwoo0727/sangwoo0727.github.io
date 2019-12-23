---
title: "[네트워크 프로그래밍] 스트림 - 출력스트림"
categories:
  - Network
tags:
  - Network
comments:
  - true
toc: true
toc_sticky: true
---
## [네트워크 프로그래밍] 스트림 - 출력스트림

### 개요
* 입출력(I/O)는 네트워크 프로그램의 가장 큰 비중을 차지한다. __하나의 시스템에서 다른 시스템으로 데이터를 이동하는 일.__
* 자바에서 I/O는 스트림(stream)에 내장되어 있다.
* 입력 스트림은 데이터를 읽고, 출력 스트림은 데이터를 쓴다.
* 대개 스트림을 생성한 이후에는 지금 읽고 쓰는 대상이 정확히 무엇인지 신경 쓰지 않아도 된다.
* 필터 스트림은 입력 스트림이나 출력 스트림에 연결될 수 있다.
* 필터는 읽거나 쓰는 데이터를 수정하는 데 사용된다. ex) __java.io.DataOutputStream 클래스__ 는 하나의 정수를 4바이트로 변환하고, 이 변환된 바이트를 기본 출력 스트림에 쓰는 메소드를 제공한다.
* 프로그램이 바이트 데이터가 아닌 텍스트를 읽고 쓸 수 있도록 __Reader와 Writer 클래스__ 를 연결할 수 있다. 이 두개의 클래스를 적절히 사용하면 다양한 문자 인코딩을 처리할 수 있다.
* 스트림은 __동기(synchronous)__ 로 동작. 즉, 프로그램이 데이터를 읽거나 쓰기 위해 스트림에 요청하면, 스트림은 다른 작업을 수행하기 전에 데이터를 읽거나 쓸 수 있을때까지 기다린다.

### 출력스트림
* 자바의 기본 출력 클래스는 java.io.OutputStream
* public abstract class OutputStream
* 제공 메소드 5개
1. public abstract void write(int b) throws IOException
2. public void write(byte[] data) throws IOException
3. public void write(byte[] data, int offset, int length) throws IOException
4. public void flush() throws IOException
5. public void close() throws IOException
* OutputStream의 서브클래스는 특정 매체에 데이터를 쓰기 위해 이 메소드를 사용하게 된다. 쓰고자 하는 매체의 종류에 관계없이 주로 이 다섯 개의 메소드를 사용할 수 있다.

#### write(int b) 메소드
* OutputStream의 기반 메소드는 write(int b)이다. 이 메소드는 0~255까지의 정수를 인자로 받고 이에 대응하는 바이트를 출력 스트림에 쓴다.
* 서브클래스에서 목적에 맞는 특정 매체를 다루기 위해 변경할 수 있도록 추상 메소드로 선언되어 있다.
* ex) 서브클래스 ByteArrayOutputStream의 write()메소드는 바이트를 배열로 복사하도록 재구현할 수 있으며, 서브클래스 FileOutputStream의 write()메소드는 파일에 데이터를 쓰도록 재구현할 수 있다.
* write(int b)메소드가 int 타입을 인자로 받지만, 실제로 부호 없는 바이트를 쓴다. 자바는 부호 없는(unsigned)바이트 타입을 지원하지 않기 때문에 int 타입이 대신 사용되었다.
* int타입을 써도 결국 8비트만 전송된다. 0~255의 범위를 벗어난 int 타입이 메소드에 전달되면 , int타입의 최하위 바이트가 쓰이고 나머지 3바이트는 무시된다.(캐스팅 효과)
* cf) 종종 서드파티 클래스를 사용할 때, 0~255를 벗어난 값을 쓰면, Illegal ArgumentException 예외가 발생하거나 항상 255가 써지는 버그가 발견될 수 있으니, 0~255를 벗어난 정수값은 쓰지 않는 것이 좋다. 

#### flush() 메소드
* 스트림은 네트워크 하드웨어가 아닌 자바 코드 내에서 직접 버퍼링을 할 수 있다.
* 스트림 아래에 BufferedOutputStream이나 BufferedWriter를 연결하여 버퍼링이 가능해진다. 이러한 버퍼링이 사용되기 때문에 출력 스트림을 사용할 때 플러시(flush)가 중요하다.
* 일반적으로 요청을 보낸 후에는 추가로 다른 요청을 보내기 전에 이미 보낸 요청의 응답을 기다린다.
* 서버로부터 응답을 받기 전에 추가로 쓸 데이터가 없다면, 요청은 버퍼에 담긴 채로 전송되지 않기 때문에 서버로부터의 응답은 오지 않을 것.

![](/assets/img/Network/1910271.jpg)

* 위의 그림이 그에 대한 설명이다.
* flush()메소드는 버퍼가 아직 가득 차지 않은 상황에서 강제로 버퍼의 내용을 전송함으로써 데드락 상태를 해제한다.
* 스트림이 버퍼링되는지를 판단하여 플러시하는 것보다 항상 플러시를 하는 것이 좋다. 특정 스트림의 참조를 획득하는 방법에 따라 해당 스트림의 버퍼링 여부를 판단하기 어려운 상황도 발생하기 때문이다.

#### close()메소드
* 스트림 사용이 끝나면 해당 스트림의 close()메소드를 호출하여 스트림을 닫는다. 
* close() 메소드는 파일 핸들 또는 포트 같은 해당 스트림과 관련된 리소스를 해제한다.
* 해당 스트림이 네트워크 연결 스트림이라면 스트림을 닫을 때 네트워크 연결이 종료된다.
* 출력 스트림을 닫은 후에 스트림에 쓰기를 시도하면 IOException이 발생한다.(일부 몇몇 종류의 스트림은 닫은 후에도 스트림의 객체에 일부 작업이 허용된다.)
* 장시간 실행 중인 프로그램에서 스트림을 닫지 않을 경우, 파일 핸들, 네트워크 포트 또는 다양한 리소스에서 누수가 발생한다.
* 자바 6와 이전 버전에서는 finally 블록 안에서 스트림을 닫는 방법을 사용하는 것이 좋다. 스트림 변수는 유효범위를 고려하여 try 바깥에 선언해야하고, 초기화는 try 안에서 해야한다.
* NullpointerExceptions를 피하려면 스트림 변수를 닫기 전에 널 여부를 검사할 필요가 있다.
* 스트림을 닫을 때 발생하는 예외는 일반적으로 무시하거나 필요하면 로그를 남긴다.
  
```java
//자바 6와 이전 버전에서의 close() 사용법

OutputStream out = null; //스트림변수는 try 바깥에 선언
try{
  out = new FileOutputStream("/tmp/data.txt"); //초기화는 try블록 안에서
  //출력 스트림을 이용한 작업
} catch(IOException ex){ //NullpointerExceptions를 피하기 위해 스트림 변수를 닫기 전에 검사
  System.err.println(ex.getMessage());
} finally{ //스트림을 닫는 방법은 finally 블록 안에서!
  if(out != null){
    try{
      out.close();
    } catch(IOException ex){
      //무시
    }
  }
}
```

* 일반적으로 인스턴스가 가비지 컬렉터에 의해 소거되기 전에 정리하는 데 사용된다.
* 자바 7에서는 인스턴스를 정리하기 위해 좀 더 깔끔한 방법으로 try-with-resources 생성자가 추가되었다.
* 스트림 변수를 try블록 인자 목록안에 선언한다.
* 또한 try 블록 인자 목록에 있는 AutoCloseable 객체의 close()를 자동으로 호출하므로 finally 구문은 더 이상 필요하지 않다.

```java
//자바 7부터
try(OutputStream out = new FileOutputStream("/tmp/data.txt")){
  //출력 스트림을 이용한 작업
} catch(IOException ex){
  System.err.println(ex.getMessage());
}
```

#### 출처 : 자바 네트워크 프로그래밍 , O'REILLY, 제이펍