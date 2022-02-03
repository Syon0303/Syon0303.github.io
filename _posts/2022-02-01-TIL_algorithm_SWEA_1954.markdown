---
layout: post
title:  "SWEA_1954, 달팽이 숫자"
date:   2022-02-01 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 SWEA 문제 중 하나인 1954번, 달팽이 숫자를 풀어보고자 한다.


<br>
[문제 바로가기](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PobmqAPoDFAUq&categoryId=AV5PobmqAPoDFAUq&categoryType=CODE&problemTitle=1954&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1&&&&&&&&&)

이 문제는 항상 우 - 하 - 좌 - 상 순으로 꺾이는 형태를 가지고 있다.<br><br>
(0, 0)부터 우측 방향으로 시작하여 내가 가려고 하는 방향이 테두리이거나 숫자가 채워져 있다면 방향을 전환해야한다.

```java
import java.util.Scanner;

public class Main {

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int T;
      T = sc.nextInt();

      for(int test_case = 1; test_case<=T; test_case++) {
         int n = sc.nextInt();
         
         // 내 위치
         int dx = 0;
         int dy = 0;
         
         // 전체 배열
         int[][] myArr = new int[n][n];
         
         // 우 0, 하 1, 좌 2, 상 3
         int idx = 0;
         
         // 우 하 좌 상 순서임.
         int[] x = {0, 1, 0, -1};
         int[] y = {1, 0, -1, 0};
         
         for(int i=1; i<=n*n; i++) {
            // 일단 i 값을 myArr[dx][dy]에 넣는다.
            myArr[dx][dy] = i;
            
            // dx와 dy를 원래 가던 방향대로 이동시킨다.
            dx = dx + x[idx];
            dy = dy + y[idx];
            
            // 만약 내 위치가 테두리를 넘었거나, 온 곳이 0이 아니면(int 배열로 초기화해서 default 값 0임)
            if(dx>=n || dy>=n || dx <0 || dy<0 || myArr[dx][dy] != 0) {
               // 잘못 간 만큼 다시 빼준다.
               dx = dx - x[idx];
               dy = dy - y[idx];
               
               // 인덱스에 1을 더해준다. (만약 오른쪽으로 가고있었다면, 아래로 내려갈 수 있도록)
               idx = (idx+1) % 4;
               
               // 제대로 갔으니 좌표 값을 다시 더해 원하는 방향으로 나아갈 수 있도록 한다.
               dx = dx + x[idx];
               dy = dy + y[idx];
            }
         }
         
		 System.out.println("#" + test_case);

         for(int temp[] : myArr) {
            for(int t : temp) {
               System.out.print(t + " ");
            }
            System.out.println();
         }      
      }
   }
}

```
