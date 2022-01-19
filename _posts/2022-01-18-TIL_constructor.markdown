---
layout: post
title:  "생성자"
date:   2022-01-18 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 생성자에 대해 알아보고자 한다.

<h1>생성자란?</h1>
* 생성자는 __new 연산자를 통해 인스턴스를 생성할 때 반드시 호출되고 제일 먼저 실행되는 일종의 메서드(하지만 메서드는 아님)이다.__ 생성자는 인스턴스 변수(필드 값 등)를 초기화 시키는 역할을 한다.


<br>
<h2>생성자의 선언 방법</h2>

```java
public 클래스(매개 변수){
    ...
}

```
* 생성자를 선언하는 방법은 위의 내용과 같다. 클래스라는 부분은 생성자를 정의하는 클래스의 이름과 동일하게 적어줘야 한다. 위의 public에 대한 내용은 추후 다룰 예정이고, 우선은 생성자를 선언할 때에는 public을 적어주면 된다.

<br>
<h2>생성자의 종류 및 사용</h2>
위에서 생성자를 선언하는 방법을 알아보았다. 인스턴스를 생성할 때 반드시 생성자를 호출한다고 했다. 그러나 지금까지는 생성자를 정의하지 않았는데 어떻게 호출이 된 것일까? 그 이유는 클래스를 정의할 때 __생성자를 생략하면 컴파일러가 자동적으로 기본 생성자(Default Constructor)를 생성하여 주기 때문이다.__ 아래 소스 코드를 통해 알아보자.

```java
public ConstructorEx01 {
    // public ConstructorEx01(){} // Default Constructor 자동 생성

    public static void main(String[] args){
        ConstructorEx01 ce = new ConstructorEx01(); // 인스턴스 생성 및 생성자 호출
    }
}

```
* 위의 내용처럼 인스턴스를 생성할 때 생성자를 호출한다. 생성자를 생략하면 주석 처리된 부분이 자동으로 생성되며, 이번에는 기본 생성자를 직접 정의하는 방법을 알아보자.
<br>

```java
public class ConstructorEx01{
    public ConstructorEx01(){
        System.out.println("생성자 호출 성공");
    }

    public static void main(String[] args){
        ConstructorEx01 ce = new ConstructorEx01(); // 인스턴스 생성 및 생성자 호출
    }
}

```
* 처음 예제와 비교하자면 생성자를 직접 정의하였고, 생성자 내부에 소스 코드 한 줄을 추가하였다. 결과는 "생성자 호출 성공"이 출력된다.
* 생성자를 선언할 때 매개변수를 괄호 안에 가질 수 있다고 했고, 그것에 대한 예제를 알아보자.
<br>

```java
public class ConstructorEx02{
    public ConstructorEx02(String a){
        System.out.println(a + "생성자 호출 성공");
    }

    public static void main(String[] args){
        ConstructorEx02 ce = new ConstructorEx02("사용자 정의" ); // 인스턴스 생성 및 생성자 호출
        // ConstructorEx02 ce2 = new ConstructorEx02(); // 컴파일 에러
    }
}

```
* 매개변수를 갖는 생성자를 정의하였고, main 메서드에서 생성자를 호출하였다. 결과는 '사용자 정의 생성자 호출 성공'이 출력된다.
* 주석 처리된 부분은 기본 생성자를 호출하지만, 컴파일 에러가 발생한다. __이는 사용자가 정의한 생성자가 있어서 Default 생성자를 더 이상 자동으로 만들어주지 않기 때문이다.__
<br>

```java
public class ConstructorEx03{
    public ConstructorEx03(){
        System.out.println("생성자 호출 성공");
    }
    public ConstructorEx03(String a){
        System.out.println(a + "생성자 호출 성공");
    }

    public static void main(String[] args){
        ConstructorEx03 ce = new ConstructorEx03("사용자 정의"); // 매개변수를 갖는 생성자
        ConstructorEx03 ce2 = new ConstructorEx03(); // 기본 생성자 호출
    }
}

```
* 기본 생성자를 사용자가 추가하였고, 정상적으로 결과가 출력된다. 참고로 __생성자의 매개변수를 다르게 지정하여 정의하는 것을 생성자 오버로딩__ 이라고 한다.
* 생성자의 역할은 인스턴스를 초기화 시킨다고 했는데, 무엇을 의미하는지는 소스 코드를 통해 알아보자.
<br>

```java
class CalculatorEx{
    int a;
    int b;

    public CalculatorEx(){
        a = 10;
        b = 20;
    }

    public CalculatorEx(int num1, int num2){
        a = num1;
        b = num2;
    }

    public void sum(){
        System.out.println("합계 : " + (a + b));
    }
}

public class ConstructorEx04{
    public static void main(String[] args){
        CalculatorEx cc = new CalculatorEx();
        cc.sum();
        
        CalculatorEx cc2 = new CalculatorEx(0, 10);
        cc2.sum();
    }
}

```
* 별도의 클래스를 하나 정의하였고 필드변수 (int a, int b)를 선언하였다. 기본 생성자와 매개변수를 갖는 생성자를 정의하고 메서드에서 합을 구하는 코드를 추가하였다.
* main 메서드에서 __인스턴스를 생성하면서 생성자를 호출하였고, 호출된 생성자에 의해 필드변수가 초기회되었다.__ 그리고 인스턴스로 메서드를 호출하여 값을 출력한다.


<br>
<h2>생성자의 특징 (메서드와의 차이)</h2>
* 생성자는 반드시 __클래스명과 동일하게 정의__ 해야 한다.
* 생성자 앞에는 __접근 제어자(public 따위와 같은)만 올 수 있다.__ (메서드와 다르게 static 따위의 수식어 불가능)
* __반환 값이 없으므로 void도 필요 없다.__ (메서드는 void 따위와 같은 자료형이 있어야 함)
* 그 외에도 상속이 되지 않거나 하는 차이가 있다.

<br>
[설명 출처][설명]<br>

[설명]: https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=heartflow89&logNo=220955879645
