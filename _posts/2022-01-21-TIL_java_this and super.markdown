---
layout: post
title:  "this, super"
date:   2022-01-21 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA 에서의 this와 super에 대해 알아보고자 한다.

<h2>this</h2>
<br>
* 현재 클래스의 Instance를 의미한다.
* 즉, 현재 Class의 멤버변수를 지정할 때 사용한다.

아래 예제를 확인해보자.
```java
public class Paraent{
    
    // 멤버 변수
    private String mother;
    private String father;

    public Parent(){
        this.mother = "mother";
        this.father = "father";
    }

    public Parent(String mother, String father){
        this.mother = mother;
        this.father = father;
    }

    public String toString(){
        return this.father + "/" + this.mother;
    }
}
```

<h3>그렇다면, this()란 무엇일까?</h3>
* 현재 클래스에 정의된 생성자를 부를때 사용한다.
* 아래는 Praent Class의 Constructor가 2개 있을 경우, Constructor Value가 들어오지 않는 경우에 this() 메서드를 사용하여 두 번째 Constructor를 불러 초기화하는 코드이다.

```java
public class Parent{
    private String mother;
    private String father;

    public Parent(){
        // this.mother = "mother";
        // this.father = "father";
        this("mother", "father");
    }

    public Parent(String mother, String father){
        this.mother = mother;
        this.father = father;
    }

    public String toString(){
        return this.father + "/" + this.mother;
    }
}
```


<h2>super</h2>
* 자식 클래스에서 상속받은 부모 클래스의 멤버변수를 참조할 때 사용한다.
* 아래는 super를 사용하는 예제이다.

```java
public class Child extends Parent{
    private String daughter;
    private String son;

    public Child(){
        this("daughter", "son");
    }

    public Child(String daughter, String son){
        this.daughter = daughter;
        this.son = son;
    }

    // Parent Class에 getter와 setter가 구현되있을 경우
    public String toString(){
        return super.getFather() + "/" + super.getMother()+ "/" + this.daughter + "/" + this.son;
    }
}
```


<h3>super()</h3>
* 자식 클래스가 자신을 생성할 때 부모 클래스의 Constructor를 불러 초기화할 때 사용한다.
* 기본적으로는 자식 클래스의 Constructor에 추가된다.
* 아래는 super()를 사용하는 예제이다.

```java
public class Child extends Parent{
    private String daughter;
    private String son;

    public Child(){
        this("daughter", "son");
    }
    
    public ChildClass(String daughter, String son){
        //super();
        super("child.mother", "child.father");
        this.daughter = daughter;
        this.son = son;
    }

    public String toString(){
        return super.getFather() + "/" + super.getMother() + "/" + this.daughter+ "/" + this.son;
    }
}

public class familyIO{
    public static void main(String[] args){
        Parent pa = new Parent();
        System.out.println(pa.getMother());
        System.out.println(pa.getFather());
        System.out.println(pa.toString());

        System.out.println();
        System.out.println();

        Child ch = new Child();
        System.out.println(ch.getDaughter());
        System.out.println(ch.getSon());
        System.out.println(ch.toString());
    }
}
```
<br>
위 코드의 실행 결과로는,<br>
mother<br>
father<br>
father/mother<br>
<br>
<br>
daughter<br>
son<br>
child.father/child.mother/daughter/son
<br>
이렇게 된다.

<b4>Child 객체 생성 時 (Child ch = new Ch)호출 순서</b4>
* Child() 생성자 호출
* this()에 의해 


<br>
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fxy28c%2FbtqFa77tFXV%2FBEOrJ7TikSk3fAfpnEhUJK%2Fimg.png" width="50%" height="50%" title="IS-A관계"/>
</p>

<h2>HAS - A</h2>
* ~ 를 가진다.
* HAS-A 관계에서는 상속을 사용하지 않는다.
* HAS-A 관계(Has a relationship, association)는 일반적인 포함 개념의 관계이다.
* HAS-A 관계는 다른 클래스의 기능(변수 혹은 메서드)을 받아들여 사용한다.


<br>
<h2>상속의 잘못된 생각</h2>
* __상속을 코드 재사용의 개념으로 이해하면 안된다.__
* 코드를 재사용할 수 있다고 마구잡이로 잘못 사용하는 경우가 있는데, 상속을 사용하면 클래스 間 결합도가 높아져 상위 클래스를 수정해야할 때 하위클래스에 미치는 영향이 매우 크다. 
* 결국, 상속은 IS-A 관계에서 사용해야 한다고 볼 수 있다.


<br>
<h2>나의 생각 및 정리</h2>
* 결국 HAS-A 관계는 멤버변수화라고 생각한다. (Computer Class에 CPU와 RAM, MainBoard Class가 존재하듯)
* 상속은 형제는 따지지 않으며 위아래만 따진다.

HAS-A 관계의 예를 들어보자.
```java
public Class Cpu{
    String productCompany = intel;
    String series = coreI;
    int generation = 12;
}

public Class Ram{
    String productCompany = samsung;
    String classification = ddr4;
    int capacity = 8;
    boolean isDesktop = true;
    int bandwidth = 2666;
}

public Class Computer {
    CPU cpu;
    RAM ram;
}

```
위 코드에서, Computer와 Cpu(또는 Ram) Class 사이의 관계가 상속을 받아야 된다고 생각하는가? 아니다. IS-A 관계를 생각해보자. 
* 컴퓨터는 CPU 이다. (X)
* CPU는 컴퓨터 이다. (X)


둘 다 뭔가 이상하다. 그렇다면 ~ 를 가진다를 대입해보자.
* 컴퓨터는 CPU를 가진다. (O)
* CPU는 컴퓨터를 가진다. (X)


<br>
결국, HAS-A 관계는 하나의 객체가 다른 객체를 "(부분으로서) 갖거나" 하는 경우에 사용하는 것을 알 수 있다.
<br>
만약, IS-A 관계가 확실한지에 대해 의문을 가진다면, HAS-A 관계를 한번 생각해보자. 


<br>
[설명 출처][설명]<br>
[설명 출처2][설명2]<br>

[설명]: https://zangzangs.tistory.com/44
[설명2]: https://minusi.tistory.com/entry/%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%A0%81-%EA%B4%80%EC%A0%90%EC%97%90%EC%84%9C%EC%9D%98-has-a%EC%99%80-is-a-%EC%B0%A8%EC%9D%B4%EC%A0%90