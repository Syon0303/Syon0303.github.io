---
layout: post
title:  "Overriding과 this, 그리고 super"
date:   2022-01-21 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA 에서의 this와 super에 대해 알아보고자 한다.

<h2>Overriding</h2>
* 부모 클래스로부터 상속받은 메서드를 자식 클래스에서 재정의하여 사용하는 것이다.
* 부모 클래스에서 정의된 메서드가 자식 클래스에서 다르게 정의가 필요할 때 사용된다. 
* 일반 클래스의 상속 관계에서는 많이 사용되는 개념은 아니지만, 추상 클래스나 인터페이스를 상속받을 때에는 반드시 사용되는 개념이다.


<h3>Overriding의 조건 및 방법</h3>
* 자식 클래스에서 부모 클래스의 메서드를 재정의 하기 위해서는 조건을 지켜야한다.
* __부모 메서드의 이름, 반환 타입, 매개변수의 수, 자료형, 순서를 동일하게__ 하여 자식 클래스에서 작성해야 한다. 
* 접근 제어자는 주로 부모 클래스와 동일하게 사용하지만, 접근 범위를 넓게 지정할 순 있다.(default -> public과 같이)
* 아래는 Overriding의 예시 코드이다.

```java
class SuperClass{
    public void check(){
        System.out.println("Parent method");
    }

    public void sum(int x, int y){
        int sum = 0;
        for (int i = x; i <= y; i++){
            sum += i;
        }
        System.out.println("sum : " + sum);
    }
}

class SubClass extend SuperClass{
    
    // method overriding
    public void check(){ 
        System.out.println("Child method");
    }

    // method overriding
    public void sum(intx, int y){
        int sum = 0;
        int odd = 0;
        int even = 0;

        for (int i = x; i <= y; i++){
            sum += i;
            if(i % 2 == 0){
                even += i;
            }
            else {
                odd += i;
            }
        }
        System.out.println("Sum : " + sum);
        System.out.println("even sum : " + even + "/ odd sum : " + odd);
        // super.sum(x, y); // 부모 메서드 호출
    }
}

public class OverridingEx01{
    public static void main(String[] args){
        SubClass sub = new SubClass();
        sub.check();
        sub.sum(0, 10);
    }
}
```

* 부모 클래스에서 2개의 메서드를 정의하였고, 자식 클래스에서 부모 클래스를 상속받아 메서드를 재정의하였다.
* main 메서드에서 자식 클래스로 인스턴스를 생성하고 메서드를 각각 호출한 결과 자식 클래스의 메서드가 데이터로 출력되는 것을 확인하였다.
* __부모의 메서드는 은닉되고 자식 클래스의 재정의된 메서드만 호출__ 된다.
* 이때 부모의 메서드를 호출하고 싶다면 super.sum(int, int)로 호출이 가능하고 위 소스 코드의 주석을 해제하면 부모 메서드가 호출된다.


<h3>Overriding 요약</h3>
* Overriding이란 __상속받은 메서드를 자식 클래스에서 재정의해서 사용하는 것__ 이다.
* 자식 클래스에서 __부모의 메서드를 수정해야 할 때__ 사용된다. 일반 클래스의 상속 관계에서는 많이 사용되지는 않고 __추상 클래스나 인터페이스에서 필수적으로 사용되는 개념__ 이다.
* 자식 클래스에서 부모 클래스의 메서드와 __동일한 시그니쳐(메서드 이름, 리턴 타입, 매개변수의 수, 자료형, 순서)__ 를 적용해야 한다.
* Overriding 결과 __부모 메서드는 은닉__ 되고, __자식 클래스에서 재졍의된 메서드만 기본적으로 호출__ 된다. __필요 時 super.으로 부모 메서드를 호출__ 할 수 있다.

<hr/>

<br>
<h2>this</h2>
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


<br>
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
<br>
<hr/>


<br>
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


<br>
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
    
    public Child(String daughter, String son){
        //super();
        super("child.mother", "child.father"); // 여기서 Parent의 생성자를 호출한다.
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
* this()에 의해 Child(String, String) 생성자 호출
* super()에 의해 Parent(String, String) 생성자 호출
* Child 객체(ch) 생성됨


<br>

[Overriding 출처][재정의]<br>
[this & super 출처][설명]<br>
[this & super 출처2][설명2]<br>

[재정의]: https://blog.naver.com/PostView.naver?blogId=heartflow89&logNo=220961515893&redirect=Dlog&widgetTypeCall=true&directAccess=false
[설명]: https://blog.naver.com/PostView.naver?blogId=heartflow89&logNo=220961515893&redirect=Dlog&widgetTypeCall=true&directAccess=false
[설명2]: https://ithub.tistory.com/66
