---
layout: post
title:  "동적 바인딩과 정적 바인딩"
date:   2022-01-24 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 동적 바인딩과 정적 바인딩에 대해 알아보고자 한다.


<br>
<h2>바인딩 이란 무엇인가</h2>
* 각종 값들이 확정되어 더 이상 변경할 수 없는 구속(bind)상태가 되는 것
* 즉, 컴파일 시 각각의 코드가 메모리 어딘가에 저장되고 함수를 호출하는 부분에는 그 함수가 저장된 메모리의 주소 값이 저장되며 프로그래머가 값을 변경할 수 없는 상ㅇ태가 된다.
* 여기서 함수를 호출하는 부분에 함수가 위치한 메모리 번지로 연결시킨 것을 바인딩이라고 한다.


<br>
<h2>정적(Static) 바인딩</h2>
* 실행 이전에 값이 확정되면 정적 바인딩
* 컴파일 타임에 호출될 함수가 결정되는 것으로, 함수는 기본적으로 정적 바인딩이다.
* 컴파일러는 선언된 자료형을 보고 바인딩을 하기 때문에 실제로 가리키는 객체가 무엇이든 포인터의 자료형일 기반으로 호출의 대상을 결정한다.


<br>
<h2>동적(Dynamic) 바인딩</h2>
* 실행 이후에 값이 확정되면 동적 바인딩이라고 한다.
* 런타임에 호출될 함수가 결정되는 것으로, virtual 키워드를 통해 동적 바인딩하는 함수를 가상 함수라고 한다.
* 함수가 가상 함수로 선언이 되면, 포인터 변수가 실제로 가리키는 객체에 따라 호출의 대상이 결정된다.
* 실행 파일을 만들 때 바인딩 되지 않고 보류 상태로 둔다.
* 실행 시간에 실제로 사용된 객체의 클래스형에 의해 호출될 함수가 결정된다.
* 점프할 메모리 번지를 저잫아기 위한 메모리 공간을 가지고 있다가 런타임에 결정한다.


<br>
<h2>예시</h2>

```java
public class PolymorphismTest{
    public static void main(String[] args){
        SuperClass superClass = new SuperClass();
        superClass.methodA();
        superClass.methodB();

        SuperClass subClass = new SubClass();
        subClass.methodA();
        subClass.methodB();
    }
}

class SuperClass{
    void methodA(){
        System.out.println("SuperClass A");
    }

    static void methodB(){
        System.out.println("SuperClass B");
    }
}

class SubClass extends SuperClass{
    @Override
    void methodA(){
        System.out.println("SubClass A");
    }
    
    static void methodB(){
        System.out.println("SubClass B");
    }
}

```
필자가 예상하기엔, superClass 인스턴스의 methodA(), B() 함수르 불렀으니 첫 번째와 두 번째 줄에는 SuperClassA, SuperClassB가 각각 나올 것이고, subClass 인스턴스의 methodA(), B() 함수를 불렀으니 SubClass A, SubClass B가 나올 것이라고 예상했으나, 이는 정답이 아니다. 실제 결과는 SuperClassA, SuperClassB, SubClass A, SuperClass B가 나오게 되는 이유를 지금부터 알아보자.


* SubClass는 SuperClass의 methodA()를 상속받아 Overriding 했다.
* methodA() 가 어 떤 클래스의 메서드인지, Runtime, 즉 클래스 파일이 실행되는 시점에 결정된다.
* __다시 말해 동적 바인딩은 Runtime 시점에 해당 메서드를 ㄱ현하고 있는 실제 객체 타입을 기준으로 찾아가서 실행될 함수를 호출한다.__
* subClass 참조 변수로 접근 가능한 것은 부모 클래스의 멤버이지만, 자식 클래스에서 메서드를 오버라이딩 했으므로 자식 클래스의 메서드를 호출한다.
* subClass 참조 변수는 런타임 시에 SubClass의 methodA()를 호출한다.
* subClass 참조 변수는 컴파일 시에 SuperClass의 static 메서드인 methodB()를 호출한다.


<br>
<h3>정리</h3>
* 모든 인스턴스 메서드는 Runtime 시에 결정된다.
* 클래스(static) 메서드와 인스턴스 변수는 Compile 시에 결정된다.
* 따라서 Instance 인지, static 인지에 따라 달라진다.


<br>
[설명 출처][설명]<br>
[설명 출처2][설명2]<br>

[설명]: https://woovictory.github.io/2020/07/05/Java-binding/
[설명2]: https://todayscoding.tistory.com/16
