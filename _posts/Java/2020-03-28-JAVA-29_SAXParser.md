---
title: "[Java] 자바를 이용한 XML 파싱(SAX parser 활용)"
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
이전 포스팅에서 자바에서 XML을 파싱하는 방법으로 SOX Parser와 DOM Parser가 있다는 것을 설명하였다.

이번 포스팅에서는 SAX Parser에 대해 구체적으로 코드를 통해 적어보려 한다.

SAX 파서에 대해 다시 한 번 알아보자.

## SOX Parser
Simple API for XML Parser의 약어로, 자바 API에서 제공한다.

기본적으로 SAX 파서는 문서를 순회하며 event가 발생하면서 순차적으로 파싱을 하게 된다. 

SAX는 XML 문서를 읽어들여서 어떤 태그를 만나면 그에 따라 이벤트를 생성한다.

SAX에는 기본적으로 세가지 이벤트가 발생하고, 각각의 메소드는 아래와 같다.

__startElement()__ : 태그를 처음 만나면, 발생하는 이벤트

__endElement()__ : 닫힌 태그를 만나면 발생하는 이벤트

__characters()__ : 태그와 태그 사이의 text(내용)을 처리하기 위한 이벤트

예제 코드를 통해 알아보자.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<People>
	<person>
		<age>30</age>
		<name>홍길동</name>
		<gender>Male</gender>
		<role>Java Developer</role>
	</person>
	<person>
		<age>30</age>
		<name>김철수</name>
		<gender>Male</gender>
		<role>Designer</role>
	</person>
	<person>
		<age>21</age>
		<name>김영희</name>
		<gender>Female</gender>
		<role>FrontEnd</role>
	</person>
	<person>
		<age>28</age>
		<name>김영심</name>
		<gender>Female</gender>
		<role>MD</role>
	</person>
</People>
```

간단하게 작성한 xml 코드가 있다. valid하진 않다.

이 xml 파일을 SAX parser를 통해 파싱을 해보겠다.

먼저 xml을 파싱하여 저장할 Person 클래스를 간단히 setter와 getter를 통해 작성하겠다. 접근지시자는 무시하고 작성한다.

```java
public class Person {
	private int age;
	private String name;
	private String gender;
	private String role;
	public Person() {
	};
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public String getRole() {
		return role;
	}
	public void setRole(String role) {
		this.role = role;
	}
	@Override
	public String toString() {
		return "이름:"+name+" 나이:"+age+" 성별:"+gender+" 직책:"+role+"\n";
	}
}
```


SAX를 통해 파싱을 하기 위해서는 먼저 DefaultHandler를 상속받는 Handler 클래스를 작성해야 한다.

```java

import java.util.ArrayList;
import java.util.List;

import org.xml.sax.Attributes;
import org.xml.sax.helpers.DefaultHandler;

public class PeopleSaxHandler extends DefaultHandler{
	
	//파싱한 사람객체를 넣을 리스트
	private List<Person> personList;
	//파싱한 사람 객체
	private Person person;
	//character 메소드에서 저장할 문자열 변수
	private String str;
	
	public PeopleSaxHandler() {
		personList = new ArrayList<>();
	}
	
	public void startElement(String uri, String localName, String name, Attributes att) {
		//시작 태그를 만났을 때 발생하는 이벤트
		if(name.equals("person")) {
			person = new Person();
			personList.add(person);
		}
	}
	public void endElement(String uri, String localName, String name) {
		//끝 태그를 만났을 때,
		if(name.equals("age")) {
			person.setAge(Integer.parseInt(str));
		}else if(name.equals("name")) {
			person.setName(str);
		}else if(name.equals("gender")) {
			person.setGender(str);
		}else if(name.equals("role")) {
			person.setRole(str);
		}
	}
	public void characters(char[] ch, int start, int length) {
		//태그와 태그 사이의 내용을 처리
		str = new String(ch,start,length);
	}
    public List<Person> getPersonList(){
		return personList;
	}
	public void setPersonList(List<Person> personList) {
		this.personList=personList;
	}
}
```

위에서 언급한 세가지 이벤트에 대해 이해하기 쉽게 간단한 코드로 작성해보았다.

이제 테스트를 위한 코드를 작성하겠다.

```java
package xml;

import java.io.File;
import java.util.List;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

public class PersonSaxTest {
	public static void main(String[] args) {
		File file = new File("./src/xml/people.xml");
		SAXParserFactory factory = SAXParserFactory.newInstance();
		
		try {
			SAXParser parser = factory.newSAXParser();
			PeopleSaxHandler handler = new PeopleSaxHandler();
			parser.parse(file, handler);
			
			List<Person> list = handler.getPersonList();
			
			for(Person p:list) {
				System.out.println(p);
			}
		}catch(Exception e) {
			e.printStackTrace();
		}	
	}
}
```

출력을 통해 xml 파일을 원하는 대로 파싱하여, 출력한 결과를 확인 할 수 있다.

![](/assets/img/java/20200328_1.png)


## 출처
[https://jlblog.me](https://jlblog.me/55)

[https://jang8584.tistory.com](https://jang8584.tistory.com/59)

[https://pilgood.tistory.com](https://pilgood.tistory.com/9)