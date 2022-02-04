---
layout: post
title:  "SWEA_2805, 농작물 수확하기"
date:   2022-02-02 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 SWEA 문제 중 하나인 2805번, 농작물 수확하기를 풀어보고자 한다.


<br>
[문제 바로가기](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV7GLXqKAWYDFAXB&)

농작물에서 가운데 줄은 무조건 구해야 하므로 가운데 줄을 기준으로 농작물의 절반 지점까지는 구역을 넓히고, 가운데 줄이 되면 점점 줄여가도록 해결함.<br><br>

```java
import java.util.Scanner;

public class SWEA_2805 {

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int T = sc.nextInt();

      for(int test_case=1; test_case<=T; test_case++) {
         int N = sc.nextInt();

         int[][] arr = new int[N][N];

         // map 입력
         for(int i=0; i<N; i++) {
            String temp = "";
            temp = sc.next();
            for(int j=0; j<N; j++) {
               arr[i][j] = temp.toCharArray()[j];
               arr[i][j] = arr[i][j]-'0';
            }
         }

         // 결과를 저장하는 변수
         int result = 0;
         
         // 몇 번째 열인지 체크하는 변수
         int cnt = 0;
         
         // 중앙 열 index 저장 변수
         int mid = N/2;

         // 행마다 반복
         for(int i=0; i<N; i++) {
            
            // 중앙 열에서 얼마나 왼쪽으로 떨어져있는가?
            int start = mid - cnt;
            
            // 중앙 열에서 얼마나 오른쪽으로 떨어져 있는가?
            int end = mid + cnt;

            // end-start번 만큼 반복(무조건 홀수. 1, 3, 5 ... N/2 까지)
            for(int j=start; j<=end; j++) {
               result += arr[i][j];
            }
            // 만약 현재 행이 중앙 행(정사각형이라 행=열)보다 작다면, 더 중앙 쪽으로 갈 수 있도록 ++ (범위를 넓힐 수 있도록)
            if(i < mid) {
               cnt++;
            }
            // 만약 현재 행이 중앙 행보다 크거나 같다면(중앙 선이거나, 넘었다면), 다시 범위를 좁힐 수 있도록 -- 
            else {
               cnt--;
            }

         }
         System.out.println("#" + test_case + " " + result);
      }
   }
}

```
