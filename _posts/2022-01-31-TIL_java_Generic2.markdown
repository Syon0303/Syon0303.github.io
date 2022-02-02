---
layout: post
title:  "Generic 2"
date:   2022-01-31 19:17:36 +0900
categories: java
---
이번 포스팅에선 저번 포스팅에 이어 JAVA에서의 Generic에 대해 알아보고자 한다.


<br>
<h4>제한된 Generic과 와일드카드</h4>
어제 포스팅에선 가장 일반적인 예시를 보여주었다. 타입을 T라고 하고 외부 클래스에서 Integer를 파라미터로 보내면 T는 Integer가 되고, String을 보내면 T는 String이 된다. 만약 Student라는 클래스를 만들었을 때 T 파라미터를 Student로 보내면 T는 Student가 된다. 즉, 제네릭은 참조 타입 모두 될 수 있다.<br><br>
근데 만약 특정 범위 내로 좁혀서 젷나하고 싶다면 어떻게 해야할까?<br><br>
이때 필요한 것이 바로 extends와 super, 그리고 물음표다. 물음표는 와일드카드라고해서 쉽게 말해 '알 수 없는 타입'이라는 의미이다.<br>
일단 먼저 예시를 보면서 말해보자면 이용할 때 크게 세 가지 방식이 있다. 바로 super 키워드와 extends 키워드, 마지막으로 ? 하나만 오는 경우이다. 코드로 보자면 다음과 같다.

```java
<K extends T> // T와 T의 자손 타입만 가능 (K는 들어오는 타입으로 지정됨)
<K super T> // T와 T의 부모 타입만 가능 (K는 들어오는 타입으로 지정됨)

<? extends T> // T와 T의 자손 타입만 가능
<? super ?> // T와 T의 부모 타입만 가능
<?> // 모든 타입 가능. <? extends Object>랑 같은 의미이다.
```
보통 이해하기 쉽게 다음과 같이 부른다.<br>
__extends T : 상한 경계__ <br>
__? super T : 하한 경계__ <br><br>
__<?> 와일드 카드__ <br><br>


이때 주의해야 할 게 있다. K extends T와 ? extends T는 비슷한 구조이지만 차이점이 있다.<br>
'유형 경계를 지정' 하는 것은 같으나, 경계가 지정되고 __K는 특정 타입으로 지정되지만, ?는 타입이 지정되지 않는다는 의미이다.__<br>
쉬운 예시를 보자.

```java
// Number와 이를 상속하는 Integer, Short, Double, Long 등의 타입이 지정될 수 있으며 객체 혹은 메서드를 호출하는 경우 K는 지정된 타입으로 변환된다.
<K extends Number>

// Number와 이를 상속하는 Integer, Short, Double, Long 등의 타입이 지정될 수 있으며, 객체 혹은 메서드를 호출하는 경우 지정 되는 타입이 없어 타입 참조가 불가능하다.
<? extends T> // T와 T의 자손 타입만 가능.
```
위와 같은 차이가 있다. 그렇기 때문에 특정 타입의 데이터를 조작하고자 할 경우에는 K 같이 특정 제네릭 인수로 지정해줘야 한다. 일단 위 설명은 잠깐 뒤로하고 extends와 super부터 한 번 하나하나 예를 들어보자.
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbpS4Bp%2FbtqLcTploFp%2FcTEkLdHVPZW5lnKOvtbS91%2Fimg.png" width="50%" height="50%" title="제네릭"/>
</p><br>
다음과 같이 서로 다른 클래스들이 상속관계를 갖고 있다고 가정해보자.<br>


<h4>< K extendsT >, < ? extends T ></h4>
이 것은 T 타입을 포함한 자식 타입만 가능하다는 의미이다. 즉, 다음과 같은 경우들이 있다.

```java
<T extends B> // B와 C타입만 올 수 있음.
<T extends E> // E타입만 올 수 있음.
<T extends A> // A, B, C, D, E 타입 모두 올 수 있음.

<? extends B> // B와 C타입만 올 수 있음.
<? extends E> // E타입만 올 수 있음.
<? extends A> //A, B, C, D, E 타입 모두 올 수 있음.
```
보다시피, 상한 한계. 즉, extends 뒤에 오는 타입이 최상위 타입으로 한계가 정해지는 것이다.<br>
대표적인 예로는 제네릭 클래스에서 수를 표현하는 클래스만 받고 싶은 경우가 있다. 대표적인 Integer, Long, Byte, Double, Float, Short같은 Wrapper 클래스들은 Number클래스를 상속받는다.<br>
즉, Integer, Long, Byte, Double, Float, Short 같은 수를 표현하는 Wrapper 클래스만으로 제한하고 싶은 경우 다음과 같이 쓸 수 있다.

```java
public class ClassName<K extends Number>{ ... }
```
이렇게 특정 타입 및 그 하위 타입만 제한하고 싶은 경우 사용하면 된다. 좀 더 구체적으로 예로 들자면, 다음과 같다. Integer는 Number 클래스를 상속받는 클래스라 가능하겠지만, String은 Number 클래스와는 완전 별개의 클래스이기 때문에 에러를 띄운다.

```java
public class ClassName<K extends Number>{ ... }

public class Main{
	public static void mian(String[] args){
		ClassName<Double> a1 = new ClassName<Double>(); // OK
		ClassName<String> a2 = new ClassName<String>(); // error
	}
}
```


<h4><K super T>, <? super T></h4>
이 것은 T 타입의 부모 타입만 가능하다는 의미이다. 즉, 다음과 같은 경우들이 있다.

```java
<K super B>	// B와 A타입만 올 수 있음
<K super E>	// E, D, A타입만 올 수 있음
<K super A>	// A타입만 올 수 있음
 
<? super B>	// B와 A타입만 올 수 있음
<? super E>	// E, D, A타입만 올 수 있음
<? super A>	// A타입만 올 수 있음
```
이는 하한 한계. 즉, super 뒤에 오는 타입이 최하위 타입으로 한계가 정해지는 것이다.<br>
대표적으로는 해당 객체가 UpCasting 될 필요가 있을 때 사용한다. <br>
예를 들어 과일 클래스가 있고 이 클래스를 상속받는 '사과'클래스와 '딸기'클래스가 있다고 가정해보자.<br><br>
이때 사과와 딸기는 종류가 다르지만, 둘 다 '과일'로 보고 자료를 조작해야 할 수도 있다. (뭐 과일 목록을 뽑는다거나 등..) 그럴때 '사과'를 '과일'로 캐스팅해야 하는데, 과일이 상위 타입이므로 업캐스팅을 해야한다. 이럴때 쓸 수 있는 것이 바로 super이다.<br><br>
조금 더 현실성 있는 예제라면 제네릭 타입에 대한 객체비교가 있다.

```java
public class ClassName <E extends Comarable<? super E>> { ... }
```
한 번쯤 봤을만한 코드이다. 특히 PriorityQueue(우선순위 큐), TreeSet, TreeMap 같이 값을 정렬하는 클래스에서 봤을것이다. 만약 특정 제네릭에 대한 자기 참조 비교를 하고싶은 경우 대부분 공통적으로 위와 같은 형식을 취한다.<br>


E extends Comarable부터 한번 보자.<br>
extends는 앞서 말했듯 extends 뒤에 오는 타입이 최상위 타입이 되고, 해당 타입과 그에 대한 하위 타입이라고 했다. 그럼 역으로 생각해보면, __E 객체는 반드시 Comparable을 구현해야 한다는 의미__ 아니겠는가?

```java
public class SoltClass <E extends Comparable<E>> { ... }

public class Student implements Comparable<Student> {
	@Override
	public int compareTo(Person o){ ... };
}

public class Main{
	public static void main(String[] args){
		SoltClass<Student> a = new SoltClass<Student>();
	}
}
```
이렇게만 쓴다면 E extends Comparable < E > 까지만 써도 무방하다. 즉, SoltClass의 E는 Student가 되어야 하는데, Comparable < Person >의 하위 타입이어야 하므로 거꾸로 말해서 Comparable을 구현해야 한다는 의미인 것이다.<br><br>
그러면 왜 Comparable<E> 가 아닌 <? super E> 일까?<br>
잠깐 설명했지만, super E는 E를 포함한 상위 타입 객체들이 올 수 있다고 했다. 만약 위의 예제에서 학생보다 더 큰 범주의 클래스인 Person 클래스를 둔다면 어떻게 될까? 한마디로 아래와 같다면?

```java
public class SoltClass <E extends Comparable<E>> { ... } // Error 가능성 있음
public class SoltClass <E extends Comparable<? super E>> { ... } // 안전성이 높음

public class Person { ... }

public class Student extends Person implements Comparable<Person>{
	@Override
	public int compareTo(Person o) { ... };
}

public class Main{
	public static void main(String[] args){
		SoltClass<Student> a = new SolClass<Student>();
	}
}
```
쉽게 말하면 Person을 상속받고 Comparable 구현부인 compareTo에서 Person 타입으로 업캐스팅(Up-Casting) 한다면 어떻게 될까? <br>
만약 < E extends Comparable < E >> 라면 SoltClass < Student > a 객체가 타입 파라미터로 Student를 주지만, Complarable에서는 그보다 상위 타입인 Person으로 비교하기 때문에 Comparable < E > 의 E인 Student보다 상위 타입 객체이기 때문에 제대로 정렬이 안되거나 에러가 날 수 있다.<br>
그렇기 때문에 E 객체의 상위 타입, 즉 <? super E>을 해줌으로서 위와 같은 불상사를 방지할 수가 있는 것이다.<br><br>
< E extends Comparable < ? super E >>에 대한 설명이 좀 길었는데, 이 긴 내용은 한마디로 이것이다. __"E자기 자신 및 조상 타입과 비교할 수 있는 E"__


<h4>< ? > (와일드 카드, Wild Card)</h4>
이 와일드 카드 < ? > 는 < ? extends Object >와 마찬가지라고 했다. Object는 자바에서의 모든 API 및 사용자 클래스의 최상위 타입이다. 한마디로, < ? > 는 무엇이냐. 어떤 타입이든 상관 없다는 의미이다. 당신이 String을 받던 어떤 타입을 리턴 받던 알바 아니라는 이야기이다. 이는 보통 데이터가 아닌 '기능'의 사용에만 관심이 있는 경우에 < ? > 로 사용할 수 있다.


[설명 출처][설명]<br>

[설명]: https://st-lab.tistory.com/153