---
title: "[Java] XML과 JSON, 그리고 자바를 통한 파싱 방법"
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
## XML이란?
XML의 약자를 먼저 살펴보겠다. XML은 EXtensible Markup Language의 약자이다.

여기서 Mark-up Language는 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어라고 정의할 수 있다.

우리가 흔히 아는 Mark-up Language의 대표적인 것이 HTML이 있다.

HTML과 XML의 가장 큰 차이라고 한다면, HTML은 데이터가 어떻게 보일지(표현)에 초점을 맞춰 데이터를 표시하기 위한 언어라면, XML은 데이터 저장, 교환 공유 등에 초점을 맞춘 언어이다.

사실, XML은 SGML이라는 문서의 구조를 기술할 수 있도록 하는 국제 표준 언어가 너무 무겁고, 복잡하여 이를 해결하는 방안으로 탄생한 작은 SGML이라고 볼 수 있다.

인터넷이 발전하며 거대한 네트워크가 생기고, 여기서 발생하는 다른 기종 간의 시스템 통합 문제, 서로 다른 포맷의 데이터 통합 문제 등의 문제를 해결하기에 XML이 탄생했다고 생각하면 이해하기 쉬울 것이다.

## XML의 특징
XML은 표현으로부터 분리된 언어이고, 데이터의 표현이 아닌 저장, 교환 공유에 초점을 맞춘 언어이다.

또한, html처럼 태그를 사용하되, 미리 정의된 태그가 아닌, 사용자가 직접 태그를 정의해서 사용을 한다. XML은 이러한 태그에 대해 대소문자를 구분한다.

시작 태그와 종료 태그를 쌍으로 사용하며, html과 다르게 White space가 보존된다.

이렇게 XML 주요 문법에 맞게 작성한 XML을 Well Formed XML이라고 부른다.

추가로, XML이 valid하다고 표현하기 위해서는 Well Formed + DTD가 만족해야 한다.

DTD에 대한 설명은 링크를 하나 첨부하겠다.

__[TCPSchool.com - DTD 개요](http://tcpschool.com/xml/xml_dtd_intro)__

XML에 대한 예시를 살펴보며, XML의 구조를 익혀보자.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<People>
	<Person>
		<name>홍길동</name>
		<age>26</age>
		<gender>Male</gender>
	</Person>
	<Person>
		<name>김갑순</name>
		<age>30</age>
		<gender>Female</gender>
	</Person>
</People>
```

## 자바의 XML Parser
먼저 파싱을 프로그램적으로 정의하면 "일련의 문자열을 의미있는 토큰으로 분해하고, 그것들로 이루어진 파스 트리를 만드는 과정"을 말한다.

특정문서가 컴퓨터에 의해 처리되기 위해서는 그 구조를 프로그램에 의해 처리 가능한 데이터 구조나 구조화된 정보 등으로 변환하는 작업이 필요하다.

XML 문서 등을 읽어 들여서, 다른 프로그램이나 서브루틴이 사용할 수 있는 내부 표현 방식으로 변환해 주는 것을 파싱이라고 한다.

파서란, 문서의 내용을 읽어들여 처리하는 파싱을 수행하는 프로세서 또는 프로그램을 말한다.

특히, 자바의 XML 파서는 DOM방식과 SAX방식 두 종류가 있다.

## XML Parser - SAX
자바는 SAX 방식 api를 제공한다.

SAX는 이벤트 기반으로, 문서를 앞에서부터 순차적으로 읽어가면서 노드가 열리고 닫히는 과정에서 이벤트가 발생한다.

각각의 이벤트가 발생될 때마다 수행하고자 하는 기능을 이벤트 핸들러 기술을 이용하여 구현한다.

XML 문서를 메모리에 전부 로딩하고 파싱하는 것이 아니기 때문에 메모리 사용량이 적고, 가볍다.

단점은 특정 노드를 무작위적으로 접근하는 random access가 어렵다는 점이다.

SAX 방식을 통한 파싱에 대한 코드는 이 후 포스팅을 통해 작성하겠다.

## XML Parser - DOM
DOM 방식과 SAX 방식의 가장 큰 차이는 문서에 접근하는 방식이다. DOM 방식은 문서 전체를 메모리에 로드하여 원하는 노드에 바로 접근하여 추가 및 수정할 수 있다.

XML문서를 읽으면 모든 Element, Text, Attribute 등에 대한 객체를 생성하고, 이를 Document 객체로 리턴한다.

Document 객체는 DOM API에 알맞는 트리 구조의 자바 객체로 표현되어 있다.

XML 문서가 메모리에 모두 올라가 있어서 노드들의 검색, 수정, 구조 변경이 빠르고 용이다.

DOM 방식을 통한 파싱에 대한 코드는 이후 포스팅을 통해 작성하겠다.

## JSON이란
XML은 점점 잊혀져가고 있다. 처음 등장하였을 때는 많은 인기를 얻었지만, 많은 태그때문에 문자량이 늘어나고, 여전히 무거워 실행 속도가 느리다는 단점 등에 의해 개발자들 사이에서 인기를 잃어가고 더 유연하고 빠른 형식으로 대체되어가고 있다.

이런 가운데 JSON(JavaScript Object Notation)의 등장은 개발자들에게 큰 인기를 얻게 되었다. JSON은 매우 단순하게 JavaScript의 Object와 array를 이용하여 데이터를 표현한다. 또한 XML에 비해 매우 가볍고, 이해가 쉽다는 점이 있다.

JSON과 XML은 데이터를 저장하고 전달하기 위한 공통점을 가지고 있다. 또한 둘다 계층적인 구조를 가지고 있으며, 다양한 프로그래밍 언어에 의해 파싱될 수 있다.

차이점으로는 JSON은 태그를 사용하지 않으며, XML에 비해 코드가 단순하고, 배열을 사용할 수 있다. 

```json
"person":[
    {"name":"홍길동","age":26,"gender":"Male"},
    {"name":"김갑순","age":30,"gender":"Female"}
]
```

자바를 이용한 JSON 파싱 방법에는 다양한 라이브러리가 제공되어지지만, 이후 포스팅을 통해 파싱코드를 작성할 때는 google에서 만든 gson을 이용하겠다.

## 출처
__[TCPSchool.com](http://tcpschool.com/json/json_intro_xml)__

__[IT 내맘대로 끄적끄적](http://itnovice1.blogspot.com/2019/01/xml-json.html)__

__[GOHGOH](https://gohlab2017.tistory.com/3)__

__[java xml parser비교(DOM,SAX)](https://blog.naver.com/beabeak/50181208186)__






