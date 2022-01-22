---
layout: post
title:  "IS-A, HAS-A"
date:   2022-01-20 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA 에서의 IS-A와 HAS-A에 대해 알아보고자 한다.

<h2>IS - A</h2>

<br>
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcuH6k7%2FbtqFa7GmJK9%2FlcJYhIMQ9EFzeIbKE7fmW0%2Fimg.png" width="50%" height="50%" title="IS-A관계"/>
</p>

* ~ 이다.
* 상속은 IS-A 관계에서 사용하는 것이 효율적이다. IS-A 관계(is a relationship, inheritance)는 일반적 개념과 구체적 개념의 관계이다.
* 사람은 동물이며, 소는 동물이고, 새는 동물이다. 이것과 같은 관계이다. 즉, 일반 클래스는 구체화 하는 상황에서 상속을 사용한다.
* 상속을 사용하면 많은 장점이 있지만, 하위 클래스가 상위 클래스에 종속되기 때문에 이질적인 클래스 간에는 상속을 사용하면 안된다.
* 단순히 코드를 재사용할 목적으로 서로 관련이 없는 개념의 클래스를 상속 관계로 사용하는 것은 추천하지 않는다.


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