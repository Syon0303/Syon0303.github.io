---
layout: post
title:  "싱글톤 패턴"
date:   2022-01-23 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 싱글톤 패턴(Singleton pattern)에 대해 알아보고자 한다.


<br>
<h2>Singleton 이란 무엇인가</h2>
* __어떤 클래스가 최초 한번만 메모리를 할당(Static)하고 그 메모리에 객체를 만들어 사용하는 디자인 패턴__
* 생성자의 호출이 반복적으로 이루어져도 실제로 생성되는 객체는 최초 생성된 객체를 반환한다.
* 보통 아래의 코드와 같이 사용한다.

```java
public class ExampleClass{
    // insatance
    private static ExampleClass instance = new ExampleClass();

    //private constructor
    private ExampleClass(){}

    public static ExampleClass getInstance(){
        return instance;
    }
}
```
<br>
위 코드에서는 Instance라는 전역 변수를 선언하였는데 static을 줌으로써 인스턴스화 하지 않고 사용할 수 있게 하였지만, 접근 제한자가 private로 되어 있어 직접적인 접근은 불가능하다.<br>
또한, 생성자도 Private로 되어 있어 new 를 통한 객체 생성도 불가능하다. 결국, getInstance 메서드를 통해 해당 인스턴스를 얻을 수 있게 된다.


<br>
<h2>Singleton 을 사용하는 이유</h2>
* 한 번의 객체 생성으로 재사용이 가능하기 때문에 메모리 낭비를 방지할 수 있음.
* 싱글톤으로 생성된 객체는 무조건 한 번 생성으로 전역성을 띄기에 다른 객체와 공유가 용이함.
* 당연히, 문제점도 존제한다.


<br>
<h2>Singleton 의 문제점</h2>
* 전역성을 띄면서 다른 객체와 공통으로 사용하는 경우와 같은 몇 가지의 케이스만 효율적이며, 그 이외에는 문제점이 생길 수 있다.
* 싱글톤으로 만든 객체의 역할이 간단한 것이 아닌 역할이 복잡한 경우라면 해당 싱글톤 객체를 사용하는 __다른 객체간의 결합도__ 가 높아져 객체 지향 설계 원칙에 어긋나게 된다.(Open-Closed principle, 개방 폐쇄 원칙)
* 해당 싱글톤 객체를 수정하는 경우 싱글톤 객체를 사용하는 곳에서 __Side effect__ 가 발생할 확률이 높아지며, multi-thread 환경에서 동기화 처리 문제 등이 생기게 된다.


<br>
<h2>다양한 Singleton 의 구현</h2>
싱글톤을 구현하는 방법은 몇 가지가 있는데, 아래와 같이 이루어져 있다.


<br>
<h4>static block</h4>

```java
public class ExampleClass{
    // insatance
    private static ExampleClass instance;

    //private constructor
    private ExampleClass(){}

    static{
        try{
            instance = new ExampleClass();
        }
        catch(Exception e){
            throw new RuntimeException(e.getMessage());
        }
    }
    
    public static ExampleClass getInstance(){
        return instace;
    }
}

```
위와 같이 static 블럭을 사용하는 경우 클래스의 특징 중 하나인 클래스가 생성될 떄 단 한 번만 실행하는 특성을 활용한다. 그러나 인스턴스가 사용되는 시점이 아닌 클래스 로딩 시점에 실행이 되는 문제가 있다.


<br>
<h4>lazy initialization</h4>
위 static 방법에서 개선하여 클래스의 로딩 시점이 아닌(생성 시점) 인스턴스가 필요하여 요청할 때 생성되는 형태로 작성하였다.

```java
public class ExampleClass{
    // insatance
    private static ExampleClass instance;

    //private constructor
    private ExampleClass(){}

    public static ExampleClass getInstance(){
        if (instance == null) { 
            instance = new ExampleClass();
        }
        return instance;
    }
}

```
그러나 위의 형태로 구현할 경우 multi-thread 환경에서 취약하다.<br>
특정 Thread가 동시에 getInstance()를 호출한다면 인스턴스가 두 번 생성되는 문제가 발생한다. 


<br>
<h4>Thread safe + Lazy</h4>

```java
public class ExampleClass{
    // insatance
    private static ExampleClass instance;

    //private constructor
    private ExampleClass(){}

    public static synchronized ExampleClass getInstance(){
        if (instance == null) { 
            instance = new ExampleClass();
        }
        return instance;
    }
}

```
Lazy에서 보였던 GetInstance() 메서드에 synchronized 키워드를 붙임으로서 Thread에서 동시 접근에 대한 문제를 해결하였다.<br>
그러나, synchronized 키워드는 성능의 저하를 발생시킨다.


<br>
<h4>Holder</h4>

```java
public class ExampleClass{
    //private constructor
    private ExampleClass(){}

    private static class InnerInstanceClazz(){
        private static final ExampleClass instance = new ExampleClass();
    }

    public static ExampleClass getInstance(){
        return InnerInstanceClaszz.instance;
    }
}

```
JVM의 클래스 로더 매커니즘과 클래스의 로드 시점을 이용하여 내부 클래스를 통해 생성 시킴으로서 Thread 間 동기화 문제를 해결한다<br>
__위 방법은 현재 Java Singleton 생성에서 사용하는 가장 대표적인 방법이다.__


<br>
<h4>정리</h4>
Singleton pattern은 Spring framework에서도 많이 사용되며, 어떤식으로 구현하는지 알아두면 굉장히 많은 도움이 된다.<br>
Java와 Spring에서의 Singleton 차이점이라면, Singleton 객체의 생명주기가 다르긴 하다.<br>
또한, Java에서 공유 범위는 Class loader 기준이지만, Spring에서는 ApplicationContext가 기준이 된다.


<br>
[설명 출처][설명]<br>

[설명]: https://elfinlas.github.io/2019/09/23/java-singleton/
