---
title: "[Java] System.out.println() 동작원리"
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
자바를 시작하며 가장 처음 접하는 메소드 중 System.out.println()이 있다.2

문득 자바 공부를 하면서, 항상 습관적으로 사용하던 이 메소드에 대해 궁금해졌다.

가장 먼저 나오는 System 클래스는 java.lang 패키지에 속하는 클래스이다.

java.lang에 속하기 때문에 우리는 import를 하지 않고 사용할 수 있는 것이다!

![](/assets/img/java/20200528_1.png)

그렇다면 System.out의 out은 무엇일까?

![](/assets/img/java/20200528_2.png)

System 클래스 안에는 static 변수이면서, PrintStream의 객체 out 변수가 존재한다.

static 변수이기 때문에 인스턴스 생성없이 System.out으로 바로 접근할 수 있는 것이다. 

그렇다면 PrintStream은 무엇일까?

![](/assets/img/java/20200528_3.png)

PrintStream 클래스는 FilterOutputStream을 상속받으면서, Appendable, Closeable 인터페이스를 구현하고 있는 클래스이다.

즉, System.out.println()은 결국, System 클래스안에 있는 static 변수 out은 PrintStream클래스의 객체이고, println()은 PrintStream의 메소드라는 것을 유추할 수 있다.

그렇다면 이번에는 println() 메소드를 살펴보자.

![](/assets/img/java/20200528_4.png)

println() 메소드는 PrintStream 클래스 안에서 다양한 매개변수에 따라 오버로딩 되어있는 것을 확인할 수 있었다.

심지어는 매개변수가 없는 println() 메소드도 정의되어 있었다. 우리가 흔히 System.out.println(); 으로만 작성해도 오류가 나지 않은 이유다.

사진에서 보듯이 println(어떤 매개변수) 메소드는 print(어떤 매개변수); 와 newLine(); 으로 이루어져 있는 것을 알 수 있다.

먼저 print(어떤 매개변수) 메소드부터 알아보자.

![](/assets/img/java/20200528_5.png)

print(어떤 매개변수) 메소드는 역시나, 매개변수 타입에 의해 다양하게 오버로딩 되어있었다.

print 메소드는 write 메소드를 호출하는데, 매개변수의 타입에 따라 스트링으로 변환되어 write(문자열)을 호출하고 있었다.

우리가 println()안에 넣는 값은 이렇게 문자열로 바뀌어 write 메소드에 의해 출력되고 있던 것이다.

그리고, println()메소드는 print메소드 아래에 있는 newLine() 메소드에 의해 우리는 개행처리를 하지 않아도, 자동으로 개행이 이루어졌던 것이다.

위에서 잠깐 언급한 오버로딩된 매개변수가 없는 println() 메소드를 보면, newLine()메소드만 호출하고 있는 것을 확인할 수 있었다.

더욱 깊게 알고싶었지만 아직 스트림에 대한 정확한 이해를 하지 못해서, 이정도로 마무리 지으려한다.

추후 스트림 공부를 끝내고 나면 다시 한번 살펴보겠다!

System.out.println() 메소드 하나만으로도 자바의 객체지향에 대해 느낄 수 있었다~




