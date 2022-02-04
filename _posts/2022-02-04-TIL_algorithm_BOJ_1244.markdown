---
layout: post
title:  "BOJ_1244, 스위치 켜고 끄기"
date:   2022-02-04 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 백준 문제 중 하나인 1244번, 스위치 켜고 끄기를 풀어보고자 한다.


<br>
[문제 바로가기](https://www.acmicpc.net/problem/1244)

문제가 시키는대로 구현하면 된다.<br><br>
그러나.. 문제를 꼭 끝까지 잘 읽자. 왜 안되지 왜 안되지 하면서 계속 제출했었다. __이 문제는 한 줄에 20개씩 출력해야 한다.__

```java
import java.util.Scanner;

public class BJ_1244 {

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int n = sc.nextInt();

      boolean [] arr = new boolean[n];
      for(int i=0; i<n; i++) {
         int temp = sc.nextInt();
         if(temp == 0) {
            arr[i] = false;
         }
         else if (temp == 1){
            arr[i] = true;
         }
      }

      int stuNum = sc.nextInt();

      for(int i=0; i<stuNum; i++) {
         int gender = sc.nextInt();
         int inputNum = sc.nextInt();

         // 만약 (받은 수 - 1)이 스위치 수보다 크다면 그만
         if(inputNum-1 > n) {
            break;
         }
         
         // 남학생이든 여학생이든 받은 수에 알맞은 index의 스위치는 무조건 reverse
         arr[inputNum-1] = !arr[inputNum-1];

         // 남학생 (% 연산을 사용하면 더 깔끔했을 것 같다. 풀고 나니까 보인다.)
         if(gender == 1) {
            int cnt = 2;
            while(true) {
               if(inputNum * cnt <= n) {
                  arr[inputNum*cnt-1] = !arr[inputNum*cnt-1];
                  cnt++;
               }
               else {
                  break;
               }
            }
         }

         // 여학생
         else if(gender == 2) {
            // 받은 수를 2로 나눈 것 만큼 돌아라
            for(int j=1; j<=n/2; j++) {
               // 만약 내 왼쪽이 0보다 크거나 같고, 내 오른쪽이 받은 수보다 작다면
               if(inputNum-1-j >= 0 && inputNum-1+j < n) {
                  // 내 왼쪽과 오른쪽이 같다면 둘 다 reverse
                  if(arr[inputNum-1-j] == arr[inputNum-1+j]) {
                     arr[inputNum-1-j] = !arr[inputNum-1-j];
                     arr[inputNum-1+j] = !arr[inputNum-1+j];
                  }
                  // 내 왼쪽과 오른쪽이 같지 않다면 반복 그만
                  else {
                     break;
                  }
               }
            }
         }

      }
      
      // 결과를 0과 1로 출력하기 위해 boolean[]을 int[]로 변환
      int[] result = new int[n];
      for(int i=0; i<n; i++) {
         if(arr[i] == false) {
            result[i] = 0; 
         }
         else if(arr[i] == true) {
            result[i] = 1;
         }
      }
      
      // 결과 출력
      for(int i=1; i<=result.length; i++) {
         System.out.print(result[i-1] + " ");
         if(i % 20 == 0) {
            System.out.println();
         }
      }
   }
}

```
