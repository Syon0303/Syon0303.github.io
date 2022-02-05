---
layout: post
title:  "BOJ_4673, 셀프 넘버"
date:   2022-02-05 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 백준 문제 중 하나인 4673번, 셀프 넘버를 풀어보고자 한다.


<br>
[문제 바로가기](https://www.acmicpc.net/problem/4673)

문제가 시키는대로 구현하면 된다.<br><br>

```java

public class Main {

   public static void main(String[] args) {
      // boolean[] 은 초기화 시 false로 초기화된다.
      boolean[] check = new boolean[10001];

      for(int i=1; i<10001; i++) {
         // 1부터 원래 내 수에 각 자릿수를 더한 값을 n이라는 변수를 만들어 넣는다.
         int n = myFunc(i);
         
         // 만약 n이 10000보다 작거나 같은 셀프 넘버라면, check[]를 true로 변경한다.
         if(n < 10001) {
            check[n] = true;
         }
      }
      
      for(int i=1; i< 10001; i++) {
         // 만약 check[]이 false 라면 (1만보다 작거나 같은 셀프 넘버가 아니라면)
         if(!check[i]) {
            System.out.println(i);
         }
      }
   }

   private static int myFunc(int myNum) {
      // 원래 내 수에 각 자릿수를 더한 값
      int sum = myNum;

      while(myNum != 0) {
         sum += myNum % 10;
         myNum /= 10;
      }
      return sum;
   }
}

```
