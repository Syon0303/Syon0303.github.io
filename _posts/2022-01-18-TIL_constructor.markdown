---
layout: post
title:  "생성자"
date:   2022-01-18 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 생성자에 대해 알아보고자 한다.

<h1>생성자란 무엇인가</h1>
* 다차원 배열이란, 2차원 이상의 배열을 의미하며, 배열 요소로 또 다른 배열을 가지는 배열을 의미한다.
* 2차원 배열은 배열 요소로 1차원 배열을 가지는 배열이며, 3차원 배열은 배열 요소로 2차원 배열을 가지는 ...


<br>
<h2>2차원 배열</h2>
* 2차원 배열이란 배열의 요소로 1차원 배열을 가지는 배열이다. 기본적인 배열(1차원)보다야 사용 빈도가 적긴 하지만, 많이 사용되니 알아두면 좋을 것이다.

2차원 배열은 아래와 같이 선언된다.

<br>
<p align="center">
    <img src="https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDVfMjQ3/MDAxNDg4NzIyMTkzMTQ4.9gK3m7XnaMiLivGafCCCZSjuJQMFe3z81fSgEnNSy3Mg.b7GeOEEVrUY7atju5i7vI-FVcdezcZ6pyW6zZRlrOBkg.PNG.heartflow89/image.png?type=w800" width="50%" height="50%" title="2차원 배열 선언 예시"/>
</p>



<br>
2차원 배열도 기본 배열과 마찬가지로 2가지 형식으로 배열을 정의할 수 있다. 첫 번째 방식은 배열의 값을 미리 알고 있을 때 사용하게 된다. 2차원 이상의 배열들은 수학의 행렬과 비슷한 자료구조를 갖는다. 우선 간단한 예제를 통해 알아보자.

```java
public class ArrayEx05{
    public static void main(String[] args){
        int[][] num = { {4, 3, 4},
                        {3, 7, 6},
                        {5, 8, 7},
                        {9, 9, 10} };
        
        for(int i = 0; i < 4; i++){
            for(int j = 0; j < 3; j++){
                System.out.println(i+"행 "+j+"열의 값 : "+num[i][j]);
            }
        }
    }
}

```
<br>
4행 3열짜리 배열을 정의하였고, 배열에 저장된 값 출력을 위해 이중 반복문을 작성하였다.


2차원 배열을 정의하는 두 번째 방법은 아래와 같다.

<br>
<p align="center">
    <img src="https://mblogthumb-phinf.pstatic.net/MjAxNzAzMDVfMjM5/MDAxNDg4NzIyMjYzODA1.kMdpX6fckTj61D3tVEdjvp6rRXx9oWiErr3Nz0TRTz4g.YaAMSUD_1K17RiB5RLaBaQB5YH4A4HnwgtZmBxIqz78g.PNG.heartflow89/image.png?type=w800" width="50%" height="50%" title="2차원 배열 선언 예시"/>
</p>

<br>
위와 같은 방법은 값을 모르는 경우에 주로 사용되며, 우선 첫 번째 방법과 같은 예제를 두 번째 방법으로 정의하는 방법에 대해 알아보자.

```java
public class ArrayEx06{
    public static void main(String[] args){
        int[][] num = new int[4][3];
        num[0][0] = 4; num[0][1] = 3; num[0][2] = 4;
        num[1][0] = 3; num[1][1] = 7; num[1][2] = 6;
        num[2][0] = 5; num[2][1] = 8; num[2][2] = 7;
        num[3][0] = 9; num[3][1] = 9; num[3][2] = 10;
        for(int i = 0; i < 4; i++){
            for(int j = 0; j < 3; j++){
                System.out.println(i+"행 "+j+"열의 값 : "+num[i][j]);
            }
        }
    }
}

```
<br>
첫 번째 방법으로 정의한 배열과 동일한 결과가 출력된다. 이제 2차원 배열과 관련된 응용 예제에 대해 몇 가지 알아보도록 하자.

```java
public class ArrayEx07{
    public static void main(String[] args){
        int[][] score = { {79, 80, 99},
                          {95, 85, 89},
                          {90, 65, 56},
                          {69, 78, 77} };
        int[] student = new int[4];
        int[] subject = new int[3];
        String[] stuName = {"A", "B", "C", "D"};
        String[] subName = {"eng", "math", "sci"};

        for (int i = 0; i < student.length; i++){
            for(int j = 0; j < subject.length; j++){
                student[i] += score[i][j];
            }
            System.out.println(stuName[i] + " 총점 : " + student[i]);
        }
        for(int j = 0; j < subject.length; j++){
            for(int i = 0; i <student.length; i++){
                subject[j] += score[i][j];
            }
            System.out.println(subName[j]+" 총점 : " + subject[j]);
        }
    }
}

```

우선 4명의 학생의 과목별 점수를 2차원 배열로 저장하고, 학생별 총점과 과목별 총점을 위해 배열을 생성하였다. 그리고 학생 및 과목별 총점 계산을 위한 반복문을 정의하고, 저장된 값을 출력시켰다.


<br>
추가적으로 2차원 배열을 이용한 구구단을 출력하는 예제를 알아보자.

```java
public class ArrayEx08{
    public static void main(String[] args){
        int[][] ggd = new int[8][9];
        for(int i = 0; i < 8; i++){
            for(int j = 0; j < 9; j++){
                ggd[i][j] = (i+2) * (j+1);
                System.out.print((i+2)+"*"+(j+1)+"="+ggd[i][j]+"\t");
            }
            System.out.println();
        }
    }
}

```

먼저 값 저장을 위한 배열 ggd을 생성하고, 단수(행)와 곱하기(열)를 반복시키기 위한 반복문을 정의하고 배열에 값을 저장하고 저장된 값을 출력시켰다.



<br>
[설명 출처][설명]<br>

[설명]: https://m.blog.naver.com/PostView.naver?blogId=heartflow89&logNo=220950845259&navType=by
