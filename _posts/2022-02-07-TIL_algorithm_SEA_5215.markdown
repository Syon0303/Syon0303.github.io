---
layout: post
title:  "SEA_5215, 햄버거 다이어트"
date:   2022-02-07 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 SEA 문제 중 하나인 5215번, 햄버거 다이어트를 풀어보고자 한다.


<br>
[문제 바로가기](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWT-lPB6dHUDFAVT&categoryId=AWT-lPB6dHUDFAVT&categoryType=CODE)

햄버거로 다이어트를 하겠다는 괴상한 문제. 출제자가 다이어트 중인데 햄버거가 먹고싶었던 것 같다.<br>
이 문제의 경우 재귀를 얼마나 잘 다루는지에 대한 문제였던 것 같은데, 오후에 공부한 재귀 코드를 그냥 2차원 배열로 바꾼 뒤 만져보니까 풀렸다. 그나마 좀 헤매던 부분은 res 변수(sumN)를 처음에 초기화하지 않아 15분정도 날린 것 같다.<br>

```java
package day3;

import java.util.Scanner;

public class SWEA_5215 {

   static int N;
   static int L;
   static int[][] map;
   static boolean[] v;
   static int res;

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int TC = sc.nextInt();


      for(int test_case=1; test_case<=TC; test_case++) {


         N = sc.nextInt(); // 재료의 수
         L = sc.nextInt(); // 제한 칼로리
         map = new int[N][2];
         v = new boolean[N];

         // map 생성
         for(int i=0; i<N; i++) {
            for(int j=0; j<2; j++) {
               map[i][j] = sc.nextInt();
            }
         }

         // 입력 확인
//         for(int[] arr : map) {
//            for(int temp : arr) {
//               System.out.print(temp + " ");
//            }
//            System.out.println();
//         }

         // res 초기화 필수
         res = 0;
         dfs(0);
         System.out.println("#" + test_case + " " + res);

      }
   }

   static void dfs(int cnt) {
      // 종료 조건
      if(cnt == N) {
         // 선택된 내용들의 합을 구하여 그 합이 L 이하이면서 가장 높은지 판단
         int sumN = 0;
         int sumL = 0;

         // 만약 간 곳이라면, N과 L을 각각 더해라
         for(int i=0; i<N; i++) {
            if(v[i]) {
               sumN += map[i][0];
               sumL += map[i][1];
            }
         }
         // 만약 지정된 L보다 sumL이 작거나 같다면
         if(sumL <= L) {
            res = Math.max(sumN, res);
         }
         return;
      }

      //실행 및 재귀
      v[cnt] = true;
      dfs(cnt + 1);

      v[cnt] = false;
      dfs(cnt + 1);
   }
}
```
