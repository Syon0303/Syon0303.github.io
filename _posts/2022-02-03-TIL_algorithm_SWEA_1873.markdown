---
layout: post
title:  "SWEA_1873, 상호의 배틀필드"
date:   2022-02-03 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 SWEA 문제 중 하나인 1873번, 상호의 배틀필드를 풀어보고자 한다.


<br>
[문제 바로가기](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5LyE7KD2ADFAXc&)

문제가 시키는대로 구현하면 된다.<br><br>
그러나 시키는 일이 너무 많아서 문제였다.. 설명은 코드 내 주석 참조

```java
import java.util.Scanner;

public class SWEA_1873 {

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int T = sc.nextInt();

      for(int test_case = 1; test_case <= T; test_case++) {
         int h = sc.nextInt();
         int w = sc.nextInt();

         char[][] map = new char[h][w];

         // 맵 입력
         for(int i=0; i<h; i++) {
            char[] temp = sc.next().toCharArray();

            for(int j=0; j<w; j++) {
               map[i][j] = temp[j];
            }
         }

         int n = sc.nextInt();
         String myAction = sc.next();
         char[] myCharArr = myAction.toCharArray();

         // 내가 바라보는 방향을 나타내는 변수. 0=상, 1=하, 2=좌, 3=우
         int idx = 0;

         // 현재 내 위치
         int dx = 0;
         int dy = 0;

         // 맨 처음 전차의 위치(dx, dy)를 찾고, 현재 바라보고 있는 방향(idx) 저장
         for(int i=0; i<h; i++) {
            for(int j=0; j<w; j++) {
               if(map[i][j] == '^') {
                  idx = 0;
                  dx = i;
                  dy = j;
                  break;
               }
               else if(map[i][j] == 'v') {
                  idx = 1;
                  dx = i;
                  dy = j;
                  break;
               }
               else if(map[i][j] == '<') {
                  idx = 2;
                  dx = i;
                  dy = j;
                  break;
               }
               else if(map[i][j] == '>') {
                  idx = 3;
                  dx = i;
                  dy = j;
                  break;
               }
            }
         }

         for(int i=0; i<myAction.length(); i++) {
            // 문자가 'S' (Shoot)라면
            if (myCharArr[i] == 'S') {

               // 전차가 위를 보고 있다면
               if(dx-1 >= 0 && idx == 0) {
                  // 현재 전차 위치의 한칸 위부터 전차 열의 벽까지
                  for(int j=dx; j>=0; j--) {
                     // 전차의 위가 벽돌로 이루어진 벽이라면
                     if(map[j][dy] == '*') {
                        // 전차의 위를 평지로 바꾼다.
                        map[j][dy] = '.';
                        break;
                     }
                     // 전차의 위가 강철로 이루어진 벽이면 아무것도 하지 않는다.
                     else if(map[j][dy] == '#') {
                        break;
                     }
                     // 전차의 위가 물이거나 평지라면 아무것도 하지 않는다.(for문 계속)
                     else if(map[j][dy] == '-' || map[j][dy] == '.') {
                        continue;
                     }
                  }
               }
               // 전차가 아래를 보고 있다면
               else if(dx+1 <= h && idx == 1) {
                  // 현재 전차 위치의 한칸 아래부터 전차 열의 벽까지
                  for(int j=dx; j<h; j++) {                            
                     // 전차의 아래가 벽돌로 이루어진 벽이라면
                     if(map[j][dy] == '*') {
                        // 전차의 아래를 평지로 바꾼다.
                        map[j][dy] = '.';
                        break;
                     }
                     // 전차의 아래가 강철로 이루어진 벽이라면 아무것도 하지 않는다.
                     else if(map[j][dy] == '#') {
                        break;
                     }
                     // 전차의 아래가 물이라면 아무것도 하지 않는다(for문 계속)
                     else if(map[j][dy] == '-' || map[j][dy] == '.') {
                        continue;
                     }
                  }
               }
               // 전차가 왼쪽를 보고 있다면
               else if(dy-1 >= 0 && idx == 2) {
                  // 현재 전차 위치의 한칸 왼쪽부터 전차 행의 벽까지
                  for(int j=dy; j>=0; j--) {
                     // 전차의 왼쪽이 벽돌로 이루어진 벽이라면
                     if(map[dx][j] == '*') {
                        // 전차의 왼쪽을 평지로 바꾼다.
                        map[dx][j] = '.';
                        break;
                     }
                     // 전차의 왼쪽이 강철로 이루어진 벽이라면 아무것도 하지 않는다.
                     else if(map[dx][j] == '#') {
                        break;
                     }
                     // 전차의 왼쪽이 물이라면 아무것도 하지 않는다(for문 계속)
                     else if(map[dx][j] == '-' || map[dx][j] == '.') {
                        continue;
                     }
                  }
               }
               // 전차가 오른쪽을 보고 있다면
               else if(dy+1 <= w && idx == 3) {
                  // 현재 전차 위치의 한칸 오른쪽부터 전차 열의 벽까지
                  for(int j=dy; j<w; j++) {
                     // 전차의 오른쪽이 벽돌로 이루어진 벽이라면
                     if(map[dx][j] == '*') {
                        // 전차의 오른쪽을 평지로 바꾼다.
                        map[dx][j] = '.';
                        break;
                     }
                     // 전차의 오른쪽이 강철로 이루어진 벽이라면 아무것도 하지 않는다.
                     else if(map[dx][j] == '#') {
                        break;
                     }
                     // 전차의 오른쪽이 물이라면 아무것도 하지 않는다(for문 계속)
                     else if(map[dx][j] == '-' || map[dx][j] == '.') {
                        continue;
                     }
                  }
               }
            }
            // 문자가 'U' (Up)라면
            else if (myCharArr[i] == 'U') {
               // 먼저 바라보는 방향을 위로 바꾸고
               idx = 0;
               // 일단 전차의 머리를 돌리고
               map[dx][dy] = '^';
               // 만약 한 칸 위의 칸이 평지라면
               if(dx != 0 && map[dx-1][dy] == '.') {
                  // 현재 위치를 평지로 바꾸고
                  map[dx][dy] = '.';
                  // 나의 위치를 위로 한칸 이동한다.
                  dx--;
                  // 변경된 나의 위치에 전차를 놓는다.
                  map[dx][dy] = '^';
               }
            }
            else if (myCharArr[i] == 'D') {
               // 먼저 바라보는 방향을 아래로 바꾸고
               idx = 1;
               // 일단 전차의 머리를 돌리고
               map[dx][dy] = 'v';
               // 만약 한 칸 아래의 칸이 평지라면
               if(dx != h-1 && map[dx+1][dy] == '.') {
                  // 현재 위치를 평지로 바꾸고
                  map[dx][dy] = '.';
                  // 나의 위치를 아래로 한칸 이동한다.
                  dx++;
                  // 변경된 나의 위치에 전차를 놓는다.
                  map[dx][dy] = 'v';
               }
            }
            else if (myCharArr[i] == 'L') {
               // 먼저 바라보는 방향을 왼쪽으로 바꾸고
               idx = 2;
               // 일단 전차의 머리를 돌리고
               map[dx][dy] = '<';
               // 만약 한 칸 왼쪽의 칸이 평지라면
               if(dy != 0 && map[dx][dy-1] == '.') {
                  // 현재 위치를 평지로 바꾸고
                  map[dx][dy] = '.';
                  // 나의 위치를 왼쪽으로 한칸 이동한다.
                  dy--;
                  // 변경된 나의 위치에 전차를 놓는다.
                  map[dx][dy] = '<';
               }
            }
            else if (myCharArr[i] == 'R') {
               // 먼저 바라보는 방향을 오른쪽으로 바꾸고
               idx = 3;
               // 일단 전차의 머리를 돌리고
               map[dx][dy] = '>';
               // 만약 한 칸 오른쪽의 칸이 평지라면
               if(dy != w-1 && map[dx][dy+1] == '.') {
                  // 현재 위치를 평지로 바꾸고
                  map[dx][dy] = '.';
                  // 나의 위치를 오른쪽으로 한칸 이동한다.
                  dy++;
                  // 변경된 나의 위치에 전차를 놓는다.
                  map[dx][dy] = '>';
               }
            }



         }
         System.out.print("#" + test_case + " ");
         for(char[] temp : map) {
            for(char temp2 : temp) {
               System.out.print(temp2);
            }
            System.out.println();
         }
      }
   }
}

```
