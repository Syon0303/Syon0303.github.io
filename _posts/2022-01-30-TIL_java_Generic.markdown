---
layout: post
title:  "Generic"
date:   2022-01-30 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 Generic에 대해 알아보고자 한다.


<br>
<h2>Generic</h2>
* 데이터 형식에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있도록 하게 하는 방법
* 흔히 쓰는 ArrayList, LinkedList는 아래와 같이 사용한다.

```java
ArrayList<Integer> list1 = new ArrayList<Integer>();
ArrayList<String> list2 = new ArrayList<Integer>();

LinkedList<Double> list3 = new ArrayList<Double>();
LinkedList<Character> list4 = new LinkedList<Character>();
```
이와 같이 < > 안에 들어가는 타입을 지정해준다. 그렇다면 생각해보자. 만약 어떤 자료구조를 우리가 배포하려고 한다. 그런데 String 타입도 지원하고싶고 Integer 타입도 지원하고 싶고 많은 타입을 지원하고 싶다. 그러면 String 에 대한 클래스, Integer에 대한 클래스 등 하나하나 타입에 따라 만들 것인가? 그건 너무 비효율적이고, 이러한 문제를 해결하기 위해 제네릭이라는 것을 사용한다.<br>


이렇듯, __제네릭은 클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것을 의미__ 한다. 한마디로, 특정 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정될 수 있도록 하는 일반(Generic)타입이라는 것이다.


<h4>Generic의 장점</h4>
* 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
* 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다. 즉, 관리하기가 편하다.
* 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.


<br>
<h4>Generic 사용방법</h4>
<table><thead><tr><th>&nbsp;&nbsp;&nbsp;&nbsp;타입&nbsp;&nbsp;&nbsp;&nbsp;</th><th>설명</th></tr></thead><tbody><tr><td>&lt;T&gt;</td><td>Type</td></tr><tr><td>&lt;E&gt;</td><td>Element</td></tr><tr><td>&lt;K&gt;</td><td>Key</td></tr><tr><td>&lt;V&gt;</td><td>Value</td></tr><tr><td>&lt;N&gt;</td><td>Number</td></tr></tbody></table>
물론 반드시 한 글자일 필요는 없고, 설명과 반드시 일치해야 할 필요도 없다. 예를들어 <Ele>라고 해도 무방하지만, 대중적으로 통하는 통상적인 선언이 가장 편하기에 위와같은 암묵적인 규칙을 따른다.


<h4>클래스 및 인터페이스 선언</h4>

```java
public class ClassName <T> { ... }
public Interface InterfaceName <T> { ... }
```
기본적으로 제네릭 타입의 클래스나 인터페이스의 경우 위와 같이 선언한다.<br>
T 타입은 해당 블럭 { ... } 안에서까지 유효하다.<br>
또한 여기서 나아가 제네릭 타입을 두개로 둘 수도 있다. (대표적으로 타입 인자로 두개를 받는 대표 컬렉션인 HashMap을 생각해보자.)

```java
public class ClassName <T, K> { ... }
public Interface InterfaceName <T, K> { ... }

// HashMap의 경우 아래와 같이 선언되어있을 것이다.
public class HashMap <K, V> { ... }
```
이렇듯 데이터 타입을 외부로부터 지정할 수 있게 한다. <br>
그럼 이렇게 생성된 제네릭 클래스를 사용하고 싶을 것이다. 즉, 객체를 생성해야 하는데 이때 구체적인 타입을 명시해주어야 하는 것이다.

```java
public class ClassName <T, K> { ... }
public class Main {
	public static void main(String[] args){
		CLassName<String, Integer> a = new ClassName<String, Integer>();
	}
}
```
위 예시대로라면, T 는 String이 되고, K는 Integer가 된다. 이때 주의해야 할 점으로는 타입 파라미터로 명시할 수 있는 것은 참조 타입밖에 올 수 없다. 즉, int, double, char같은 primitive type은 올 수 없다는 것이다. 그래서 int, Double 등 primitive Type의 경우 Integer, Double 같은 Wrapper Type으로 쓰는 이유가 바로 위와같은 이유이다. <br>
또한 바꿔 말하면 참조 타입이 올 수 있다는 것은 사용자가 정의한 클래스도 타입으로 올 수 있다는 것이다.

```java
public class ClassName <T> { ... }
public class Student { ... }

public class Main{
	public static void main(String[] args){
		ClassName<Student> a = new ClassName<Student>();
	}
}
```

<h4>제네릭 클래스</h4>
클래스 및 인터페이스를 제네릭으로 받는 방법을 알아봤으니 이제 활용해보자.

```java
class ClassName<E> {
	private E element;
	
	void set(E element){
		this.element = element;
	}

	E get(){
		return element;
	}
}

class Main{
	public static void main(String[] args){
		ClassName<String> a = new ClassName<String>();
		ClassName<Integer> b = new ClassName<Integer>();

		a.set("10");
		b.set(10);

		System.out.println("a data : " + a.get());
		System.out.println("a E Type : " + a.get().getClass().getName());

		System.out.println();
		System.out.println("b data : " + b.get());
		System.out.println("b E Type : " + b.get().getClass().getName());

	}
}
```
ClassName이라는 객체를 생성할 때 <> 안에 타입 파라미터를 지정한다.<br>
그러면 a객체의 ClassName의 E 제네릭 타입은 String으로 모두 변환되고 반대로 b객체의 ClassName의 E 제네릭 타입은 Integer로 모두 변환된다. 실제로 위 코드를 실행시키면 아래와 같이 출력된다.
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbwZf3n%2FbtqLeqtwL2Z%2FjuSEIt48ifvKRIPZ6PALL1%2Fimg.png" width="30%" height="30%" title="제네릭"/>
</p><br>
만약 제네릭을 두 개 사용하고 싶다면 이렇게 할 수도 있다.

```java
class ClassName<K, V> {	
	private K first;	// K 타입(제네릭)
	private V second;	// V 타입(제네릭) 
	
	void set(K first, V second) {
		this.first = first;
		this.second = second;
	}
	
	K getFirst() {
		return first;
	}
	
	V getSecond() {
		return second;
	}
}
 
// 메인 클래스 
class Main {
	public static void main(String[] args) {
		
		ClassName<String, Integer> a = new ClassName<String, Integer>();
		
		a.set("10", 10);
 
 
		System.out.println("  fisrt data : " + a.getFirst());
		// 반환된 변수의 타입 출력 
		System.out.println("  K Type : " + a.getFirst().getClass().getName());
		
		System.out.println("  second data : " + a.getSecond());
		// 반환된 변수의 타입 출력 
		System.out.println("  V Type : " + a.getSecond().getClass().getName());
	}
}
```
결과는 다음과 같다.
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcOmFzN%2FbtqLcU2HSy0%2FBa7cK2T5szTxVrLzGKnKeK%2Fimg.png" width="30%" height="30%" title="제네릭"/>
</p><br>
이렇게 외부 클래스에서 제네릭 클래스를 생성할 때 <> 괄호 안에 타입을 파라미터로 보내 제네릭 타입을 정해주는 것. 이게 바로 제네릭 프로그래밍이다.


<h4>제네릭 메서드</h4>
위 과정까지는 클래스 이름 옆에 예를들어 < E > 라는 제네릭 타입을 붙여 해당 클래스 내에서 사용할 수 있는 E타입으로 일반화를 했다. 그러나 그 외에 별도로 메서드에 한정한 제네릭도 사용할 수 있으며, 일반적으로 선언 방법은 다음과 같다.

```java
public <T> T genericMethod(T o){ ... }

// [접근 제어자] <제네릭타입> [반환타입] [메서드명] (제네릭타입 파라미터)
```
클래스와 다르게 반환타입 이전에 <> 제네릭 타입을 선언한다. 위에서 다룬 제네릭 클래스에서 활용해보자.

```java
class ClassName<E> {
	private E element;

	void set(E element){
		this.element = element;
	}

	E get() {
		return element;
	}

	<T> T genericMethod(T o) {
		return 0;
	}
}

public class Main {
	public static void main(String[] args) {
		
		ClassName<String> a = new ClassName<String>();
		ClassName<Integer> b = new ClassName<Integer>();
		
		a.set("10");
		b.set(10);
	
		System.out.println("a data : " + a.get());
		// 반환된 변수의 타입 출력 
		System.out.println("a E Type : " + a.get().getClass().getName());
		
		System.out.println();
		System.out.println("b data : " + b.get());
		// 반환된 변수의 타입 출력 
		System.out.println("b E Type : " + b.get().getClass().getName());
		System.out.println();
		
		// 제네릭 메소드 Integer
		System.out.println("<T> returnType : " + a.genericMethod(3).getClass().getName());
		
		// 제네릭 메소드 String
		System.out.println("<T> returnType : " + a.genericMethod("ABCD").getClass().getName());
		
		// 제네릭 메소드 ClassName b
		System.out.println("<T> returnType : " + a.genericMethod(b).getClass().getName());
	}
}
```
보면 ClassName이란 객체를 생성할 때 <> 안에 타입 파라미터를 지정한다. 그러면 a객체의 ClassName의 E 제네릭 타입은 String으로 모두 변환된다.<br>
반대로 b객체의 ClassName의 E 제네릭 타입은 Intger로 모두 변환된다. genericMethod()는 파라미터 타입에 따라 T 타입이 결정된다.<br>
실제로 위 코드를 실행시키면 다음과 같이 출력된다.

<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fczs31x%2Fbtq8zxKbZ05%2FftKMjz295wRPDhCSa2UZtK%2Fimg.png" width="30%" height="30%" title="제네릭"/>
</p><br>
즉, 클래스에서 지정한 제네릭유형과 별도로 메서드에서 독립적으로 제네릭 유형을 선언하여 쓸 수 있다. 그럼 위와같은 방식은 왜 필요한가? 바로 '정적 메서드로 선언할 때 필요' 하기 때문이다.<br>
생각해보자. 앞서 제네릭은 유형을 외부에서 지정해준다고 했다. 즉, 해당 클래스 객체가 인스턴스화 했을 때 지정이 된다는 뜻이다.<br>
그러나 static은 무엇인가? 정적이라는 뜻이다. static 변수, static 함수 등 static이 붙은 것들은 기본적으로 프로그램 실행시에 이미 메모리에 올라가있다.<br><br>
이 말인즉슨, 객체 생성을 통해 접근할 필요 없이 이미 메모리에 올라가 있기 떄문에 클래스 이름을 통해 바로 사용할 수 있다는 것이다.<br>
근데 거꾸로 생각해보면 static 메서드는 객체가 생성되기 전에 이미 메모리에 올라가있는데 타입을 어디서 얻어오는가?

```java
class ClassName<E>{
	// 클래스와 같은 E 타입이더라도 static method는 객체가 생성되기 이전 시점에 메모리에 먼저 올라가기 때문에 E유형을 클래스로부터 얻어올 방법이 없다.
	static E genericMethod(E o){ // error
		return o;
	}
}

class Main{
	public static void main(String[] args){
		// ClassName객체가 생성되기 전에 접근할 수 있으나 유형을 지정할 방법이 없어 에러
		ClassName.genericMethod(3);
	}
}
```
위 내용을 보면 이해가 갈 것이다. 그렇기 때문에 __제네릭이 사용되는 메서드를 정적 메서드로 두고 싶은 경우 제네릭 클래스와 별도로 독립적인 제네릭이 사용되어야 한다__ 는 것이다.

```java
class ClassName<E> {
	private E element;

	void set(E element){
		this.element = element;
	}

	E get() {
		return element;
	}

	// 아래 메서드의 E 타입은 제네릭 클래스의 E 타입과 다른 독립적인 타입이다.
	static <E> E genericMethod1(E o){ // 제네릭 메서드
		return o;
	}

	static <T> T genericMethod2(T o) { // 제네릭 메서드
		return o;
	}
}

public class Main{
	public static void main(String[] args){
		ClassName<String> a = new ClassName<String>();
		ClassName<Integer> b = new ClassName<Integer>();
 
		a.set("10");
		b.set(10);
 
		System.out.println("a data : " + a.get());
		// 반환된 변수의 타입 출력
		System.out.println("a E Type : " + a.get().getClass().getName());
 
		System.out.println();
		System.out.println("b data : " + b.get());
		// 반환된 변수의 타입 출력
		System.out.println("b E Type : " + b.get().getClass().getName());
		System.out.println();
 
		// 제네릭 메소드1 Integer
		System.out.println("<E> returnType : " + ClassName.genericMethod1(3).getClass().getName());
 
		// 제네릭 메소드1 String
		System.out.println("<E> returnType : " + ClassName.genericMethod1("ABCD").getClass().getName());
 
		// 제네릭 메소드2 ClassName a
		System.out.println("<T> returnType : " + ClassName.genericMethod1(a).getClass().getName());
 
		// 제네릭 메소드2 Double
		System.out.println("<T> returnType : " + ClassName.genericMethod1(3.0).getClass().getName());
	}
}
```
보다시피 제네릭 메서드는 제네릭 클래스 타입과 별도로 지정된다는 것을 볼 수 있다. <> 괄호 안에 타입을 파라미터로 보내 제네릭 타입을 지정해주는 것. 이것이 제네릭 프로그래밍이다.<br>
그럼 특정 범위만 허용하고 나머지 타입은 제한할 수는 없는건가? 라는 의문이 들기 마련이다. 이 의문은 내일 포스팅할 예정이다.


[설명 출처][설명]<br>

[설명]: https://st-lab.tistory.com/153