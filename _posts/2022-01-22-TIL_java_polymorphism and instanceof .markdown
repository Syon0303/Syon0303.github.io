---
layout: post
title:  "Polymorphism & Instanceof"
date:   2022-01-22 19:17:36 +0900
categories: java
---
오늘 포스팅에선 JAVA 에서의 Polymorphism(다형성)과 Instanceof에 대해 알아보고자 한다.


<h2>Polymorphism(다형성)</h2>
* 프로그램 언어의 각 요소들(상수, 변수, 식, 오브젝트 등)이 다양한 자료형에 속하는 것이 허가되는 성질.
* 반댓말은 단형성(monomorphism)으로 프로그램 언어의 각 요소가 한 가지 형태만 가지는 성질.
* 쉽게 말하면, __다형성이란 하나의 객체에 여러가지 타입을 대입할 수 있다는 것__ 이다.


<h4>단형성 예시</h4>

```java
// 숫자를 문자열로 바꾸는 경우
String age = stringFromNumber(number);

// 날짜를 문자열로 바꾸는 경우
String today = stringFromDate(date);
```
<br>
위의 코드는 단형성 체계의 언어에서 함수의 이름이 중복될 수 없기 때문에 나오는 함수명이다. 위와 같이 비슷한 기능을 하는 함수의 이름을 자료형에 따라 끝도 없이 나열한다면 코드가 지저분해질 것이다. __다형성 체계의 언어에서는, 함수의 이름을 같게 하여 위의 작업을 간결하게 만들 수 있다.__

<h4>같은 이름의 함수이지만 다른 일을 하는 예시</h4>

```java
// 숫자를 문자열로 바꾸는 경우
String age = stringValue(number);

// 날짜를 문자열로 바꾸는 경우
String today = stringValue(date);
```
<br>

물론, 여러 자료형에 따라 문자열로 바꾸는 작업 자체가 줄어드는 것은 아니지만, 메서드 하나의 이름만 가지고도 기억하기 쉽고 메서드 이름을 절약한다는 장점이 있다.


<br>
<h3>다형성을 구현하는 방법</h3>
다형성을 구현하는 방법은 대표적으로 오버로딩, 오버라이딩, 함수형 인터페이스를 사용하는 방법이 있다. (단, 이번 포스팅에서 함수형 인터페이스는 다루지 않는다.)


<br>
<h4>Overloading</h4>
한 클래스 내에 이미 사용하는 이름의 메서드가 있어도 규칙 내에서 동일한 이름의 메서드를 정의하도록 허용하는 기술
1. 메서드의 이름이 같아야 한다.
2. 매개 변수의 개수 또는 타입이 달라야 한다.
3. 매개 변수는 같고, 리턴 타입이 다를 때는 성립하지 않는다.
4. 오버로딩된 메서드들은 매개 변수로만 구분될 수 있다.
<br>

__주의할 점으로, 요구 사항이 바뀌었을 때 모든 메서드를 수정할수도 있으므로 꼭 필요한 경우에만 사용해야한다.__ 일반적인 메서드보다는 Constructor 오버로딩을 주로 사용한다.

```java
public class Station{
    private Long id;
    private String name;

    public Station(){}

    public Station(String name){
        this(null, name);
    }

    public Station(Long id, String name){
        this.id = id;
        this.name = name;
    }
}

```
위와 같이 Station Class의 필드를 초기화해 주는 조건이 여러 가지가 있을 수 있는데, Constructor 오버로딩을 통해 쉽게 구현할 수 있다. 


<br>
<h4>Overriding</h4>
* 상위 클래스의 메서드를 재정의하는 것을 의미한다.
* 클래스 상속이나 인터페이스 상속을 통해 구현할 수 있다.

아래 예시는 운송 수단에 대한 예시로 운송 수단의 종류인 자동차, 비행기, 기차가 있다고 하자. 모두 움직일 수 있으므로 move() 메서드를 각각 아래와 같이 정의할 수 있다.

```java
public class Car{
    public void move(){
        System.out.println("도로로 달린다.");
    }
}

public class Airplane{
    public void move(){
        System.out.println("하늘을 난다.");
    }
}

public class Train{
    public void move(){
        System.out.println("선로를 주행한다.");
    }
}

```

이후에, 각각의 운송수단을 움직여보자.

```java
public class Main{
    public static void main(String[] args){
        final Car car = new Car();
        final Airplane airplane = new Airplane();
        final Train train = new Train();

        car.move();
        airplane.move();
        train.move();
    }
}

```
<br>
각각의 운송수단을 움직이기 위해 서로 다른 객체를 만들어서 move() 메서드를 실행해 주었다. 그러나 100가지의 운송수단이 생긴다면 move() 메서드를 100번 타이핑해야한다. 이들을 반복문으로 묶어줄 수 없기 때문이다. 이때 Overridng을 사용한다면 위의 문제를 해결할 수 있다.
<br><br>
이번 포스팅에선 인터페이스를 사용하여 구현하자. 우선 Movable(인터페이스는 보통 ~able이다.)이라는 인터페이스를 만들자. 이 인터페이스의 메서드로는 move()가 있고, Car, Train, Airplane에 상속해주자. 그리고 하위 클래스들은 Move() 메서드를 Overridng하자.

```java
public interface Movable{
    void move();
}

public class Car implements Movable{
    @Override
    public void move(){
        System.out.println("도로로 달린다.");
    }
}

public class Airplane implements Movable{
    @Override
    public void move(){
        System.out.println("하늘을 난다.");
    }
}

public class Train implements Movable{
    @Override
    public void move(){
        System.out.println("선로를 주행한다.");
    }
}

```

이제 Movable이라는 객체는 Car가 될 수도 있고, Airplane이 될 수도 있고, Train이 될 수도 있다. 이는 __한 객체에 여러 타입을 대응할 수 있다__ 는 것을 몸소 보여주는 예시이다. 그래서 아래와 같이 다형성을 이용하여 Car, Airplane, Train의 move() 메서드를 호출할 수 있다.

```java
public class Main{
    public static void main(String[] args){
        final List<Movable> movables = Array.asList(new Car(), new Train(), new Airplane());
        for (final Movable movable : moveables){
            movable.move();
        }
    }
}

```

<br>
<hr/>
<br>

<h3>자동 타입 변환(promotion)</h3>
* 자동 타입 변환은 부모 타입의 참조변수에 자식 객체의 주소를 넣는 행위를 뜻한다.

```java
Parent p1 = new Child();
```

* 상속 관계에서 자식클래스는 상속받은 데이터 + __α__ 의 데이터를 갖고 있다. 그러나 자동 타입 변환을 하게되면 __상속받은 데이터__ 만을 사용할 수 있다.
* 이는 타입이 자료형이기 때문인데, 우리가 자료형을 정의하는 목적은 __'범위'__ 를 설정해주기 위해서이다.
* int 형은 대충 -21억에서 21억사이의 정수를 표현할 수 있다. 자료형은 범위를 설정하여 메모리 낭비를 방지한다. 예를 들어 자료형이 8byte인 double 형 밖에 없다고 가정하면, 나는 정수만 사용할거라 4byte면 충분한데도 어쩔수 없이 8byte를 사용해야한다. 이처럼 데이터 별로 적당한 타입을 지정해주지 않으면 지나친 메모리 낭비를 유발한다.

<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBeBLn%2Fbtq7DsP0Bs1%2FISPmM4m3gwT7Kcn4yPJgi1%2Fimg.png" width="45%" height="45%" title="데이터 표현 범위"/>
</p>

참조 타입도 이와 마찬가지다. 참조 타입은 해당 클래스가 지정한 필드와 메서드를 '범위'로 가진다. 그러므로 해당 __참조 타입으로 생성된 객체는 그 범위를 벗어날 수 없다.__

```java
public class Main{
    public static void main(String[] args){
        Child ch1 = new Child();
        Parent ch2 = new Child(); // 자동 타입 변환

        if(ch1 == ch2){
            System.out.println("같은 객체입니다"); // 타입은 달라도 객체는 같다.
        }
    }
}
```
<br>
ch1과 ch2가 가리키는 객체가 같다는 의미는 두 참조변수 모두 Child 클래스의 객체를 참조하고 있다는 의미이다. 하지만 둘은 '타입'이 다르다. 다시말해서, 표현할 수 있는 '범위'가 다르다는 것이다.

```java
// 부모 클래스
public class Parent{
    public void a(){
        System.out.println("나는 부모다.");
    }
}

public class Child extends Parent{
    // 상속 받은 메서드
    public void a(){
        System.out.println("나는 자식이다.");
    }

    // 자식만 가지는 메서드
    public void b(){
        System.out.println("나만 갖고있다");
    }
}

```
<br>
이처럼 서로 같은 객체를 참조하고 있지만, 선언된 참조변수의 타입이 달라 표현 가능한 데이터의 범위가 서로 다르다. ch2는 부모 타입을 참조변수로 갖고 있기 떄문에 부모 클래스에게 상속받은 메서드와 필드만 사용 가능하다. 만약 상속받은 메서드가 오버라이딩되었다면, 오버라이딩 된 메서드를 호출한다.

```java
public class Main{
    public static void main(String[] args){
        Child ch1 = new Child();
        Parent ch2 = ch1;

        ch1.b();
        ch2.b(); //자동형변환타입. 실행되지 않는다.
    }
}

```

```java
public class Main{
    public static void main(String[] args){
        Child ch1 = new Child();
        Parent ch2 = ch1;

        ch1.a();
        ch2.a(); //자동형변환타입.
    }
}

```

<h4>매개변수로 받는 자동 타입 변환</h4>
자동 타입 변환을 매개변수로도 할 수 있다.

```java
// 자식 클래스
public class Child extends Parent{
    // 오버라이딩된 메서드
    public void a(){
        System.out.println("나는 자식");
    }

    public void b(Parent parent){ // 매개변수로 자동 변환됨
        parent.a();
    }
}
```

```java
// 메인 클래스
public class Main{
    public static void main(String[] args){
        Child ch1 = new Child();
        ch1.b(new Child());
    }
}
```

위 코드의 출력 결과로는 <br>
__나는 자식__ <br>
이 출력된다. (매개변수로 자식 객체가 들어갔으니 메서드가 오버라이딩된 메서드로 호출된다.)


<br>
<h4>강제 타입 변환(Casting)</h4>
부모타입으로 선언된 자식 객체는 자신의 메서드나 필드를 사용할 수 없다. 그래서 만약 이를 사용하는 경우 Casting을 해준다.

```java
public class Main{
    public static void main(string[] args){
        Parent parent = new Child();
        Child ch = (Child) parent; // 강제 타입 변환
        ch.b();
    }
}
```

<br>
<hr/>

<h2>Instanceof 란?</h2>
* 참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 Instanceof 연산자를 사용한다.
* 주로 조건문에 사용되며, Instanceof의 왼쪽에는 참조변수를, 오른쪽에는 타입(클래스 명)이 피연산자로 위치한다.
* 연산의 결과로는 boolean 값인 true와 False 둘 중 하나를 반환한다.
* Instanceof를 이용한 연산결과로 true를 얻었다는 것은, 참조변수가 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.
* null인 참조변수에 대해 Instanceof 연산을 수행하면 False를 결과로 얻는다.

```java
public class Main{
    public static void main(String[] args){
        Parent pa1 = new Child();
        Parent pa2 = new Parent();
        Casting casting = new Casting();

        casting.castingObject(pa1);
        casting.castingObject(pa2);
    }
}

public class Casting{
    void castingObject(Parent parent){
        if(parent instanceof Child){
            Child ch = (Child) parent;
            System.out.println("Casting success, 부모 타입의 자식 객체");
        }
        else{
            System.out.println("Casting fail, 부모 타입의 부모 객체");
        }
    }
}
```

위 코드의 결과로는 <br>
Casting success, 부모 타입의 자식 객체 <br>
Casting fail, 부모 타입의 부모 객체 <br>
가 나오게 된다.

__참조변수 instanceof 객체타입__ 의 연산 결과는 상술한대로 true 또는 False로 반환된다. casting을 시도할때는 경우를 구분하지 않으면 ClassCastException 예외가 발생하니 유념하자.

<br>

[다형성 출처][다형성]<br>
[자동 타입 변환과 강제 타입 변환 출처][설명]<br>
[Instanceof 출처][설명2]<br>

[다형성]: https://steady-coding.tistory.com/446
[설명]: https://lordofkangs.tistory.com/20
[설명2]: https://arabiannight.tistory.com/313
