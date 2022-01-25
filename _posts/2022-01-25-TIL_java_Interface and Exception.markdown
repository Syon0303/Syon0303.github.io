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
이제, KB은행, SH은행은 규격화된 Bank 인터페이스를 통해 각자에 맞는 스타일대로 은행 인출/입금 서비스를 제공해보자.

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
<br>
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
<br>
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
신규 은행사 카카오뱅크는 인터페이스를 Implements 하지 않은 채 자신만의 메서드를 구현했다. 이는 금융결제원에서 제공해주는 그 어떠한 서비스도 사용할 수 없으며 호환성이 없으며 연동이 불가능할 것이다.<br><br>
아래 메인 소스를 보면 bank = new KakaoBank(); 부분에서 type mismatch 에러가 날 것이다. 또한, 자바의 다형성을 극대화하여 개발 코드 수정을 줄일 수 있는데 어떤 부분에서 가능한 것일까? <br><br>
만약 이 메인함수가 특정 대학교에 등록금 인출, 입금 등의 업무와 관련있는 소스라고 하자. 간혹 등록금 납부 주관 은행을 교체하기도 하는데, 믈론 가상 계좌를 통해 납입하지만 주은행을 변경하게 되면 대학교 등록금 납부 시스템에 기존은행에서 교체할 은행으로 변경해줘야한다.<br><br>
이럴 경우 간단하게 인스턴스만 바꾸면 호환성이 보장된 상태에서 동일한 기능을 수행할 수 있을 것이다.


<h4>정리</h4>

__인터페이스는 추상메서드와 상수를 통해 강력한 강제성을 가지게 하여 인터페이스를 Implements한 클래스가 동일한 동작을 수행하도록 보장한다.__


<br>
<hr/>
<h2>Exception</h2>
* 자바에서의 런타임 오류는 Error와 Exception으로 나누어져 있다.
* 에러는 프로그램이 코드로 복구될 수 없는 오류를 의미하고, 예외는 프로그래머가 직접 예측하여 막을 수 있는 처리가능한 오류이다.
* 예를 들어, 메모리가 부족한 경우 프로그래머의 제어가 불가능하므로 OOM(Out Of Memory)가 발생할 것이며, 함수 호출이 많아 스택이 쌓일 경우에는 StackOverFlowError가 발생할 것이다.

```java
int a, b;
a = 10;
b = 0;

int c = a / b;
System.out.println(c);
```
<br>
위 코드는 에러가 발생한다. 어떤 수를 0으로 나눌 수는 없기 때문에 ArithmeticException 에러가 나올 것이다.<br>
그러나 조건문을 통해서 0으로 나누지 못하게 막을 수 있다. 이처럼 우리가 예측가능한 상황에서 오류를 제어할 수 있는 것이 예외이다.<br>
예외는 Compile시에 발견할 수 있는 예외와 프로그램 실행 시에 발생하는 예외 두 종류가 있다. Compile 시에 발생할 수 있는 예외는 각종 IDE가 막아주기도 한다.<br>
그러나 Compile 시에 발견하지 못하는 에러를 Runtime 에러라고 하는데, 이는 프로그래머가 예측해야 한다. 그리고 그런 예외가 발생했을 때 어떤 동작을 처리해야하는지를 예외 처리라고 한다.


<br>
<h2>예외 처리</h2>

예외가 발생했을 때, try...catch...finally 라는 키워드로 예외를 처리할 수 있거나 메서드를 호출한 곳으로 던질 수 있다. 한 가지 중요한 점은 자바에서 __모든 예외는 Exception이라는 클래스를 상속__ 받는다는 것이다. Exception의 상속 트리를 보자.

<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZDGhx%2Fbtq1I7rwySp%2FRsGRdGQ7TmuayB5jTi5CTK%2Fimg.png" width="100%" height="100%" title="데이터 표현 범위"/>
</p><br>

<h4>try catch finally</h4>
예외를 처리하는 방식은 다음과 같다.

```java
try{
	//예외가 발생될만한 코드
}catch(FileNotFoundException e){	//FileNotFoundException이 발생했다면

}catch(IOException e){ //IOException이 발생했다면

}catch(Exception e){	//Exception이 발생했다면

}finally{	
	///어떤 예외가 발생하던 말던 무조건 실행
}
```
<br>
그러나 아래와 같이 상속관계에 있는 예외 중 부모가 위의 catch, 그리고 그 자식 예외 클래스가 아래의 catch로 놓일 수는 없다.

```java
try{
	//.. 중략 ..//
} catch (Exception e){
	//컴파일 오류 발생
} catch (IOException e){

}
```
<br>
Exception 클래스는 모든 예외의 부모이기때문에 Exception을 IOException보다 위에서 처리할 수 없다. 왜냐하면, IOException의 catch 블록은 도달할 수 없기 떄문이다.


<h4>throws</h4>
아까 예외를 그냥 던질 수 있다고 했는데, 이 의미는 예외를 여기서 처리하지 않을테니 나를 불러다가 쓰는 녀석에게 에러 처리를 전가하겠다는 의미이며 코드를 짜는 사람이 이 선언부를 보고 어떤 예외가 발생할 수 있는지도 알게 해준다. 아래의 코드를 보자.

```java
public static void divide(int a,int b) throws ArithmeticException {
	if(b==0) throw new ArithmeticException("0으로 나눌 수는 없다니까?");
	int c=a/b;
	System.out.println(c);
}
public static void main(String[] ar){
	int a=10;
	int b=0;
		
	divide(a,b);
}
```
<br>
divide() 메서드는 a와 b를 나눈 후에 출력하는 역할을 하는데, 이 나누기 부분에서 우리는 예외가 발생했음을 알 수 있다. 그래서 try, catch를 사용하여 예외 처리를 해야 하지만, devide()를 호출하는 부분에서 처리하기를 원한다. 왜냐하면, __divide()를 호출한 곳에서 예외가 발생한 다음의 처리를 divide() 메서드가 정하지 않기__ 떄문이다. 예를 들어, main 메서드에서는 예외가 발생하면 다시 divide()를 호출하거나, 프로그램을 끝내거나, b의 값을 다시 입력받거나 해야하기 때문이고, divde() 메서드가 그 결정을 할 수 없다는 의미이다. 그래서 throws, ArithmetricException을 divide를 호출한 Main에 던지는(throw) 것이다. 여기서 예외를 던지는 방법은 아래와 같다.

```java
throw 예외객체
ex) throw new Exception("예외 발생!")
```
<br>
예외를 발생시키는 키워드는 throw이고, 이때 main은 그 예외를 처리하기 위해 try, catch 블록을 아래처럼 사용하면 된다.

```java
try {
	divide(a,b);
}catch(ArithmeticException e) {
	e.getMessage();
	e.printStackTrace();
}
```
<br>
만약 throws 키워드로 처리되어야 할 예외가 여러 개가 존재한다면, 쉼표러 끊어서 예외를 넘길 수도 있다. 그 결과는 아래와 같다.

```java
java.lang.ArithmeticException: 0으로 나눌 수는 없다니까?
	at aa.Main.divide(Main.java:8)
	at aa.Main.main(Main.java:17)
```


[Interface 출처][설명]<br>
[Exception 출처][설명2]<br>

[설명]: https://limkydev.tistory.com/197
[설명2]: https://reakwon.tistory.com/155
