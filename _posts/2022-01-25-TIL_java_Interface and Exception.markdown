---
layout: post
title:  "Interface & Exception"
date:   2022-01-25 19:17:36 +0900
categories: java
---
오늘 포스팅에선 JAVA 에서의 Interface와 Exception에 대해 알아보고자 한다.


<h2>Interfapce</h2>
* 극단적으로 동일한 목적 하에 동일한 기능을 수행하게끔 강제하는 것
* 자바의 다형성을 극대화하여 개발코드 수정을 줄이고 프로그램 유지보수성을 높이기 위해 사용


```java
public interface 인터페이스명{
    //상수
    타입 상수 명 = 값;

    //추상 메서드
    타입 메서드명(매개변수, ...);

    //디폴트 메서드
    default 타입 메서드명(매개변수, ...)ㅖ
        //구현부
    }

    //정적 메서드
    static 타입 메서드명(매개변수){
        //구현부
    }
}
```
<br>
* 상수 : 인터페이스에서 값을 정해줄테니 함부로 바꾸지 말고 제공해주는 값만 참조해라. (절대적)
* 추상메서드 : 가이드만 줄테니 추상메서드를 Override해서 재구현해라. (강제적)
* 디폴트메서드 : 인터페이스에서 기본적으로 제공해주지만, 맘에 안들면 각자 구현해라. (선택적)
* 정적메서드 : 인터페이스에서 제공해주는 것을 무조건 새용해라. (절대적)


<h4>예시</h4>
대한민국에서 은행 사업을 하려면, 금융결제원에서 정의한 어떠한 가이드를 따라야 한다고 치고, Bank 라는 이름으로 인터페이스를 만든다. 이제 어느 은행에서든 운영하려면 Bank 라는 인터페이스 가이드에 맞게 구현해야한다. 인출메서드, 입금메서드는 각각의 은행에서 오버라이딩을 통해 재구현해야하면, 블록체인 인증 메서드는 무조건 금융결제원에서 제공해주는 메서드를 사용한다. 따라서 정적메서드로 구현하여 오버라이딩을 할 수 없게 만들었다.

```java
public interface Bank{
    // 상수
    public int MAX_INTEGER = 100000000;

    // 추상메서드 (인출하는 메서드)
    void withDraw(int price);

    // 추상메서드 (입금하는 메서드)
    void deposit(int price);

    // 디폴트 메서드 (휴면계좌를 찾아주는 메서드이며 필수 구현은 선택사항)
    default String findDormancyAccount(String cutId){
        System.out.println("**금융개정법안 이후 고객의 휴면계좌 찾아주기 운동**");
        System.out.println("**금융결제원에서 제공하는 로직**");
        return "**은행 123-456-7890-11";
    }

    // 정적 메서드 (블록체인 인증을 요청하는 메서드)
    static void BCAuth(String bankName){
        System.out.println(bankName + " 에서 블록체인 인증을 요청합니다.");
        System.out.println("전 금융사 공통 블록체인 로직 수행");
    }
}
```
<br>

디폴트 메서드에 대해 생각해보자. 이는 대체 무엇일까? 예시를 들어보자. <br><br>
금융결제원에서 이미 인터페이스를 각 은행사에 가이드하였고 정상적으로 서비스가 되고 있는데 갑자기 금융 트렌드가 바뀌면서 고객의 휴면계좌를 찾아주는 서비스를 정부에서 점진적으로 도입하라고 지시를 했다면.. 어떻게 될까 <br><br>
그냥 추상메서드를 추가해서 다시 가이드하면 안되나? 라고 생각했지만.. 각 은행사마다 개발 환경 및 운영 환경이 다르고, 휴면계좌 찾아주기 신규 프로세스를 도입하는데 있어서 은행사마다 개발기간이 모두 상이하기때문에 조금은 러프한 메서드를 추가해주어야한다. __만약 추상메서드를 인터페이스에서 추가한다면, 이를 implements한 모든 클래스에서 강제적으로 추상메서드를 구현해야하고 구현하지 않을 시 전부 에러가 난다.__ <br><br>
그러나 디폴트 메서드를 정의하고 기본 구현부를 제공한 후 만약 맘에 들지 않는다면 각자 오버라이딩하여 재구현하도록 선택적인 메서드로 가이드한다면 시스템 운영 유지보수성이 확보될 것이다.<br><br>
__이미 운영되고 있는 시스템에서 추가 요건으로 인해 불가피하게 반영해야할 때 디폴트메서드를 사용하면 효과적이란 이야기이다.__ <br><br>
이제, KB은행, SH은 규격화된 Bank 인터페이스를 통해 각자에 맞는 스타일대로 은행 인출/입금 서비스를 제공한다.

```java
public class KBBank implements Bank{
    @Override
    public void withDraw(int price){
        System.out.print("KB은행만의 인출 로직");
        if(price < Bank.MAX_INTEGER){
            // price원 만큼 인출
        }
        else{
            // 인출 실패
        }
    }

    @Override
    public void deposit(int price){
        // KB은행만의 입금 로직...
    }
}

```
KB은행 휴면계좌 찾아주기 메서드를 재구현하지 않았다. 이는 금융결제원이 제공해주는 메서드를 사용하겠다는 뜻이거나 혹은 아직 사용하지 않겠다라고 이해하면 된다.

```java
public class SHBank implements Bank{

	@Override
	public void withDraw(int price) {
		System.out.println("SH은행만의 인출 로직...");
		if(price < Bank.MAX_INTEGER){
			System.out.println(price+" 원을 인출한다.");	
		}else{
			System.out.println(price+" 원을 인출실패.");
		}
	}

	@Override
	public void deposit(int price) {
		System.out.println("SH은행만의 입금 로직..."+price+" 원을 입금한다.");
		System.out.println("SH은행은 별도의 후행처리 작업을 따로 한다.");
	
	}
	
	//JAVA8에서 가능한 defualt 메소드(고객의 휴면계좌 찾아주는 메소드)
	@Override
	public String findDormancyAccount(String custId){
		System.out.println("**금융개정법안 00이후 고객의 휴면계좌 찾아주기 운동**");
		System.out.println("*SH은행만의 로직 적용*");
		return "00은행 000-000-0000-00";
	}
}

```
하지만 SH은행에서는 휴면계좌 찾아주기 메서드를 재정의하여 SH은행사만의 휴면계좌 찾아주기 로직을 재구현했다.

```java
public class KakaoBank{

	public void kakaoWithDraw(int price) {
		System.out.print("Kakao은행만의 인출 로직...");
		System.out.println(price+" 원을 인출한다.");	
	}

	public void kakaoDeposit(int price) {
		System.out.println("Kakao은행만의 입금 로직..."+price+" 원을 입금한다.");
	}
	
	public void kakaoFindDormancyAccount(){
		System.out.println("kakao은행만의 휴면계좌 찾아주기 로직");
	}
}

```

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
