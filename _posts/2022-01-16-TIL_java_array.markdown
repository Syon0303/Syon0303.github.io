---
layout: post
title:  "Java-array"
date:   2022-01-16 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 배열에 대해 알아보고자 한다.

<h1>ARRAY란 무엇인가</h1>
* 동일한 자료형(Data Type)의 데이터를 연속된 공간에 저장하기 위한 자료구조이다. 즉, 연관된 데이터를 그룹화하여 묶어준다고 생각하면 된다.


<br>
<h2>배열의 장점</h2>
* __연관된 데이터를 저장하기 위한 변수의 선언을 줄여주며, 반복문 등을 이용하여 계산과 같은 과정을 쉽게 처리할 수 있다.__


<br>
<h2>배열의 선언과 사용</h2>
배열을 정의하는 방법은 크게 2가지가 있다.
<br>

<p align="center">
    <b>자료형 [] 변수 = {데이터1, 데이터2, 데이터3, ...};</b>
</p>
<br>
 첫 번째 방법은, __데이터들의 값을 알고 있을 때 사용하면 편리하다.__ 예제를 보기 전에, 이해를 돕기 위해 비유를 들어보고자 한다. 우리가 자주 가는 대형마트를 생각해보면, 과자 코너, 소스 코너, 라면 코너 등 고객들과 직원들이 찾기 편하게 섹션 별로 분류되어 진열되어있다. 따라서 우리는 사고자 하는 상품이 있는 코너로 이동하게 된다. 맥주를 파는 코너로 이동하였다고 생각했을 때, 또 맥주별로 이름이 붙어있고 품목별로 분류되어 진열되어있다. 이러한 점에서 배열과 비슷하다고 할 수 있으며, 위의 비유한 내용으로 배열을 생성하는 예제를 살펴보자.

 
 <br>
```java
public class ArrayEx01 {
    public static void main(String[] args){
        String[] beer = {"Kloud", "Cass", "Asahi", "Guinness", "Heineken"};
        System.out.println(beer[0]);
        System.out.println(beer[1]);
        System.out.println(beer[2]);
        System.out.println(beer[3]);
        System.out.println(beer[4]);
    }
}

```
<br>
beer라는 String[] 변수에 데이터를 저장하였다. 저장한 데이터들을 출력하기 위해서는 인덱스 번호라는 것에 대해 알아야한다. __인덱스 번호는 데이터를 저장한 순서대로 0부터 시작하여 1씩 증가되어 만들어진다.__ 인덱스 번호는 맥주를 품목별로 구분하기 위한 구분자라고 생각하면 되며, beer라는 배열에 0~4까지의 index 번호를 가진 5개의 __공간(length)__ 에 데이터들이 저장되어있다. 이것을 그림으로 표현하면 아래와 같다.

<br>
<p align="center">
    <img src="https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDVfMTc5/MDAxNDg4Njg0ODg1NDg5.CiQ-3B3laHTAPgbHfoPToeSoEnl0CAhH_of9LxGUsVAg.FlTJq2Sleti5XSqYbhfVxly_7cpM2hcZLSJJktu00lYg.PNG.heartflow89/image.png?type=w800" width="50%" height="50%" title="맥주 예시"/>
</p>

<p align="center">
    <i>맥주 예시</i>
</p>
<br>

위에서 언급했던 반복문과 배열을 함께 사용하는 방법에 대해 예제를 통해 알아보자.

```java
public class ArrayEx02{
    public static void main(String[] args){
        int[] score = {93, 75, 95, 76, 70};
        int sum = 0;
        for(int i = 0; i < score.length; i++){
            sum += score[i];
        }
        double avg = (double) sum / score.length;
        System.out.println("점수 합계 : " + sum);
        System.out.println("점수 평균 : " + avg);
    }
}

```
<br>
score라는 int 형 배열을 만들고 계산과 형변환을 통해 총합과 평균을 구했다. 배열의 인덱스 번호를 이용해서 반복문을 사용하게 되면 쉽게 처리가 가능하다. 여기서 반복문의 조건식의 length 라는 속성을 사용하였는데, 이는 상술한 것처럼 배열의 크기를 의미한다. score에 값을 추가하거나 제거한다고 했을때 score.length는 수정할 필요가 없다. 또한 배열의 크기를 모를 때에도 length를 사용하여 반복문을 작동시킬 수 있는 장점이 있다.


<br>
배열을 정의하는 두 번째 방법은 아래와 같다.
<p align="center">
    <b>자료형[] 변수 = new 자료형[배열의 크기];<br>
    변수[0] = 데이터 값;<br>
    변수[1] = 데이터 값;</b>
</p>
<br>
위 방법은 배열의 값은 모르지만 향후 값을 저장하기 위한 배열을 생성하고 싶을 경우 주로 사용한다. new라는 연산자로 배열을 생성하는데 이는 메모리 영역(heap)이 들어가 있어 조금 복잡하다. 지금은 new를 이용해서 배열을 정의한다고만 알아두자.

<br>
두 번째 방법으로 배열을 정의하는 예제를 살펴보자.

<br>
```java
public class ArrayEx03{
    public static void main(String[] args){
        int[] num = new int[3];
        num[0] = 10;
        num[1] = 15;
        num[2] = 13;
        for(int i = 0; i < num.length; i++){
            System.out.println(num[i]);
        }
    }
}

```
<br>
이렇게 보면 첫 번째 방법으로 작성했던 것보다 어렵고 불편하다는 생각이 들 수 있다. 모르는 값을 배열에 저장하고 사용하는 예제를 확인해 보자.

```java
import java.util.Scanner;
public class ArrayEx04 {
	public static void main(String[] args) {
		int[] num = new int[5];
		int max, min;
		Scanner sc = new Scanner(System.in);
		System.out.println("5개의 정수를 입력하시오.");
		for (int i = 0; i < num.length; i++) {
			num[i] = sc.nextInt();
		}
		max = num[0];
		min = num[0];
		for (int i = 0; i < num.length; i++) {
			if (max < num[i]) {
				max = num[i];
			}
			if (min > num[i]) {
				min = num[i];
			}
		}
		System.out.println("최대값 : " + max);
		System.out.println("최소값 : " + min);
	}
}

```
<br>
위의 예제처럼 배열의 값을 모를 때 배열은 정의만 하고 값을 입력받아 데이터를 저장한다. 데이터의 값을 모를 경우가 더 많기 때문에 위와 같은 방법을 더 자주 사용하게 된다.


마무리하기 전에, 향상된 for문(for-each)에 대해서도 설명하고자 한다. 지금까지 사용하던 반복문을 사용해도 되지만, 배열을 사용하여 반복시킬 때 조금 더 편리한 방법이 있는데, 그것이 for-each 문이다. 정의하는 방법은 아래와 같다.

<br>
```java
for (자료형 변수 : 배열) {
    반복 실행할 문장;
}

```
<br>
위에서 살펴봤던 예제와 비슷한 내용으로 for-each문을 작성해보면 다음과 같다.

<br>
```java
public class ForEachEx01{
    public static void main(String[] args){
        int[] score = {78,70,65,98,58};
        int sum = 0;
        for (int i : score){
            sum += i;
        }
    }
    System.out.println("점수 합계 : " + sum);
}

```
<br>
반복문을 작성하는 구간이 문법적으로 간결해졌다. __종료 조건과 초기값, 증감식을 숨기고 배열과 변수를 이용하여 문장을 반복시키는 방법이다.__ 그러나 배열을 사용한다고 반드시 이 방법을 사용할 필요는 없다. 본인이 편한 반복문을 사용하면 되고 이런 것도 있다는 것을 알아두면 되지만.. 필자는 배열에 할당된 값을 변경하는 것이 아니라면, 웬만하면 향상된 For문을 추천한다. (코드가 간결해지기 때문)


이번 포스팅은 배열에 대해 알아보았다. __배열을 사용하게 되면 작성해야 할 코드의 양을 많이 줄일 수 있고 수정하기가 매우 간결하다.__ 자주 접하면서 익숙해져야 할 필수 문법 중 하나이기 때문이다.


<br>
[설명 출처][설명]<br>

[설명]: https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=heartflow89&logNo=220950491600
