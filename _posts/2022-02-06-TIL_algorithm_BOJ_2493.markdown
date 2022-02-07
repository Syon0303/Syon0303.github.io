---
layout: post
title:  "BOJ_2493, 탑"
date:   2022-02-06 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 백준 문제 중 하나인 2493번, 탑을 풀어보고자 한다.


<br>
[문제 바로가기](https://www.acmicpc.net/problem/2493)

처음엔 스택은 생각도 못하고 for문으로 30분정도 씨름했다. 그리고 스택을 써야됐구나를 알게되서 스택을 사용했는데, 막상 스택을 사용해보니 너무 오랜만에 사용하는 자료구조라 다시 스택 공부하고, 스택에 배열을 넣을 생각도 못하고 있다가 시간을 날려버렸다. 결국 검색해서 키워드만 좀 보자는 식으로 검색했는데, 스택에 배열도 들어가더라. 그래서 스택에 배열(idx, height)을 넣어 해결했다. 아, Scanner를 사용하니 시간초과가 자꾸 떠서 bufferedReader를 사용했다. 스택 하나에 배열을 넣는 방법으로 풀었지만, Integer형 스택을 그냥 두개 사용해도 될 것 같다.<br><br>

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Stack;
import java.util.StringTokenizer;

public class BOJ_2493 {

   public static void main(String[] args) throws NumberFormatException, IOException {
      // 입력을 위한 br 및 st 선언
      BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

      // br을 통한 탑의 수 입력
      int n = Integer.parseInt(br.readLine());

      // 탑 스택인 map 선언
      Stack<int[]> map = new Stack<int[]>();

      // 탑의 높이를 입력받기 위한 StringTokenizer st 선언
      StringTokenizer st = new StringTokenizer(br.readLine());

      // 결과 값을 저장할 StringBuilder res 선언
      StringBuilder res = new StringBuilder();


      for(int i=0; i<n; i++){
         // 다음 탑의 높이
         int num = Integer.parseInt(st.nextToken());
         if(map.isEmpty()){
            res.append(0 + " ");
         }
         else {
            while(true) {
               // 저장된 스택의 맨 위의 탑의 정보(temp)를 불러와라 
               int[] temp = map.peek();
               // temp 탑의 idx (첫 번째부터 시작)
               int idx = temp[0];
               // temp 탑의 높이
               int height = temp[1];

               // 만약 temp 탑의 높이보다 다음 탑의 높이가 작다면 (레이저가 닿는다면)
               if(height > num) {
                  res.append(idx + " ");
                  break;
               }
               // temp 탑의 높이보다 다음 탑의 높이가 크다면 (레이저가 닿지 않는다면)
               else {
                  // temp를 죽여라
                  map.pop();
               }

               // 만약 map이 비어있다면 (스택에 아무것도 없다면 = 레이저가 닿지않는다면)
               if(map.isEmpty()) {
                  // 0을 결과에 추가
                  res.append(0 + " ");
                  break;
               }
            }
         }
         // 스택에 다음 탑의 정보를 저장
         map.push(new int[] {i+1, num});
      }
      System.out.println(res.toString());
   }
}

```
