---
layout: post
title:  "순열 조합 부분집합"
date:   2022-02-10 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 자바 알고리즘인 순열, 조합 그리고 부분집합에 대해 알아보고자 한다.


<br>
<h2>순열, Permutation</h2>
<i>n개의 원소에서 순서를 생각하며 r개의 원소를 선택하는 방법</i>이다.<br>
순서를 생각하며 뽑기 떄문에 뽑은 원소의 구성이 같더라도 순서를 다르게해서 뽑혔으면 다른 경우의 수가 된다.<br>
입력 [1, 2, 3] 으로 들어오면, 출력은 [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 2, 1], [3, 1, 2]가 된다.

```java
import java.util.Scanner;

public class MyPermutation {

	static int N, R;
	static int[] input, output;
	static boolean[] v;

	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		N = sc.nextInt();
		R = sc.nextInt();

		input = new int[N];
		for(int i=0; i<N; i++) input[i] = sc.nextInt();


		output = new int[R];
		v = new boolean[R];

		dfs(0);
	}

	static void dfs(int cnt){
		if(cnt == R){
			System.out.println(Arrays.toString(output));
			return;
		}

		for(int i=0; i<N; i++){
			// 만약 들리지 않은 원소라면
			if(!v[i]) {
				// 해당 원소를 사용한다고 표시하고
				v[i] = true;
				// 출력 값의 cnt번에 input[i]를 넣는다.
				output[cnt] = input[i];

				// 그리고 그 다음 수를 검사하러 출발
				dfs(cnt+1);
				// 내 뒤로 다 검사했으면, 나를 사용하지 않는다고 표시한다.
				v[i] = false;
			}
		}
	}
}

```
<br>
<h2>조합, Combination</h2>
순열과 같이 n개의 원소에서 r개를 선택하지만, 순서를 고려하지 않는다.

```java
public class MyCombination{
	static int N, R;
	static int[] input, output;

	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		N = sc.nextInt();
		R = sc.nextInt();
		for(int i=0; i<N; i++) input[i] = sc.nextInt();
		comb(0, 0);
	}
	static void comb(int cnt, int start){
		if(cnt==R){
			System.out.println(Arrays.toString(output));
			return;
		}

		for(int i=start; i<N; i++){
			output[cnt] = input[i];
			comb(cnt+1, i+1);
		}
	}
}

```

<br>
<h2>부분 집합</h2>
부분 집합은 집합의 원소 일부로 이루어진 집합을 의미하며, 자기 자신도 부분집합이 될 수 있으며 원소가 전혀 없는 공집합도 부분집합이다.<br>
각각의 원소를 선택하는 경우와 선택하지 않는 경우의 수로 나뉘며, 전체 시간복잡도는 2^n이 된다.<br>
조합과 다른 점은, 조합은 r개를 선택하는 경우의 수 이지만, 부분 집합은 선택하는 수가 0~n까지 다양하기 떄문에 원본 배열을 모두 돌아보면 고른것들을 그대로 출력한다.<br>

```java
public class SubSetTest{
	static int N;
	static int[] input;
	static boolean[] v;

	public static void main(String[] args){
		Scanner sc = new Scanner(System.in;
		N = sc.nextInt();
		input = new int[N];
		v = new boolean[N];

		for(int i=0; i<N; i++) input[i] = sc.nextInt();
		generateSubset(0);
	}

	static void generateSubset(int cnt){
		if(cnt == N){
			for(int i=0; i<N; i++){
				if(v[i]) System.out.print(input[i] + " ");
				else System.out.print("X ");
			}
			System.out.println();
			return;
		}

		// 현재 원소를 선택하기
		v[cnt] = true;
		generateSubset(cnt+1);

		// 현재 원소를 선택하지 않기
		v[cnt] = false;
		generateSubset(cnt+1);
	}
}
```