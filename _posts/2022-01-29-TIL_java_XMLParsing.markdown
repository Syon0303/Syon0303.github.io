---
layout: post
title:  "XML Parsing"
date:   2022-01-29 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 XML Parsing에 대해 알아보고자 한다.


<br>
<h2>XML</h2>
* eXtensible Markup Language
* 데이터 저장, 교환, 공유 등에 초점을 맞춘 언어

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


<h4>JAVA XML Parser</h4>
* parsing이란, "일련의 문자열을 의미있는 토큰으로 분해하고, 그것들로 이루어진 파스트리를 만드는 과정"이다.
* java의 xml parser는 SAXParser와 DOMParser가 있다.


<br>

<h3>DOMParser</h3>
* DOM방식은 문서 전체를 메모리에 로드하여 원하는 노드에 바로 접근하여 추가 수정을 할 수 있다.
* XML 문서를 읽으면 모든 element, text, attribute등에 대한 객체를 생성하고, 이를 Document 객체로 반환한다.
* Document 객체는 DOM API에 알맞는 트리 구조의 자바 객체로 표현되어있다.
* XML 문서가 메모리에 모두 올라가 있어서 노드들의 검색, 수정, 구조 변경이 빠르고 용이하다.


<h3>SAXParser</h3>
* java는 SAX방식 api를 제공한다.
* 이벤트 기반으로, 문서를 앞에서부터 순차적으로 읽어가면서 노드가 열리고 닫히는 과정에서 이벤트가 발생한다.
* 각각의 이벤트가 발생할 때마다 수행하고자 하는 기능을 EventHandler를 사용하여 구현한다.
* XML 문서를 메모리에 전부 로딩하고 파싱하는 것이 아니기에 메모리 사용량이 적고 가볍다.
* 단점으로는 특정 노드를 무작위로 접근하는 random access가 어렵다는 점이다.


<h4>method</h4>
* startElement() : 태그를 처음 만나면 발생하는 이벤트
* endElement() : 닫힌 태그를 만나면 발생하는 이벤트
* characters() : 태그와 태그 사이의 text를 처리하기 위한 이벤트

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
위 코드는 간단한 xml 파일을 불러온 것이다. 이 xml 파일을 SAXParser를 통해 파싱해보고자 한다.<br>
먼저 xml을 파싱하여 저장할 person class를 간단히 setter와 getter를 통해 작성한다.

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
SAXParser를 사용하려면 먼저 DefaultHandler를 상속받는 Handler클래스를 작성해야 한다.

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
위에서 언급한 세 가지 메서드에 대해 이해하기 쉬운 간단한 코드를 작성해보았다.

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
file 대신 api 키로 만들어진 url 주소를 넣어도 똑같이 동작한다. <br> 
출력을 통해 xml파일을 원하는 대로 파싱하여 출력한 결과를 확인할 수 있다.

<p align="center">
    <img src="https://sangwoo0727.github.io/assets/img/java/20200328_1.png" width="65%" height="65%" title="버퍼"/>
</p><br>

[설명 출처][설명]<br>
[설명 출처2][설명2]<br>

[설명]: https://sangwoo0727.github.io/java/JAVA-29_SAXParser/
[설명2]: https://sangwoo0727.github.io/java/JAVA-27_xmljson/