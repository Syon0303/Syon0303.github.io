---
layout: post
title:  "SEA_9229, 한빈이와 SpotMart"
date:   2022-02-09 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 SEA 문제 중 하나인 9229번, 한빈이와 SpotMart를 풀어보고자 한다.


<br>
[문제 바로가기](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AW8Wj7cqbY0DFAXN&categoryId=AW8Wj7cqbY0DFAXN&categoryType=CODE&problemTitle=9229&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1)

보자마자 아~ 완전탐색으로 풀면 되겠군 했는데.. 재귀를 아직 잘 못다뤄서 그런지 삽질이 좀 길었다.<br><br>

```java
import java.util.Scanner;

public class SEA_9229 {

	static int N;
	static int M;
	static int[] map;
	static boolean[] v;
	static int res;

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();

		for(int test_case=1; test_case<=T; test_case++) {
			// 과자 봉지의 수
			N = sc.nextInt();
			
			// 견딜 수 있는 무게
			M = sc.nextInt();
			
			// 들린 곳 초기화
			v = new boolean[N];
			
			// 과자 봉지의 무게를 저장하는 배열
			map = new int[N];
			for(int i=0; i<N; i++) map[i] = sc.nextInt();

			res = -1;
			
			// 과자 무조건 2개 뽑아야됨
			dfs(0, 2);
			
			System.out.println("#" + test_case + " " + res);
		}
	}
 
   static void dfs(int cnt, int r) {
		int sum = 0;
		
		if(r == 0) {
			for(int i=0; i<N; i++) {
				if(v[i]) sum += map[i];
			}
			if(sum <= M) res = Math.max(res, sum);
			
			
			return;
		}
		
		if(cnt == N) return;
		
		v[cnt] = true;
		dfs(cnt + 1, r-1);
		
		v[cnt] = false;
		dfs(cnt + 1, r);
	}
}
```
