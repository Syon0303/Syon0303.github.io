---
layout: post
title:  "BOJ_1935, 후위 표기식 2"
date:   2022-02-08 19:17:36 +0900
categories: algorithm
---
이번 포스팅에선 BOJ 문제 중 하나인 1935번, 후위 표기식2를 풀어보고자 한다.


<br>
[문제 바로가기](https://www.acmicpc.net/problem/1935)

보통 학부 2학년때 풀어보는 stack 계산기의 열화판. 구현은 쉬웠으나 secNum이랑 firstNum을 바꾸는데 30분이나 걸렸다..<br><br>

```java
import java.util.Scanner;
import java.util.Stack;

public class BOJ_1935 {

	// 피연산자를 담는 수를 저장하는 스택을 생성
	// 연산자가 나오면, 스택에 있는 2개의 수를 꺼냄
	// 그 두 수를 연산자에 맞게 계산 후 다시 스택에 넣음
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		// 5
		int N = sc.nextInt();

		// {'A', 'B', 'C', '*', '+', 'D', 'E', '/', '-'}
		char[] strArr = sc.next().toCharArray();
		
		double[] numArr = new double[N];
		Stack<Double> stack = new Stack<Double>();
	
		// {1, 2, 3, 4, 5}
		for(int i=0; i<N; i++) numArr[i] = sc.nextInt();
		
		
		for(int i=0; i<strArr.length; i++) {
			// i번째 char
			char temp = strArr[i];
			
			// 만약 temp가 연산자 라면
			if(temp == '+' || temp == '-' || temp == '*' || temp == '/'){
				// 끝에서 두개 꺼낸다
				double secNum = stack.pop();
				double firstNum = stack.pop();
				if(temp == '+') stack.push(firstNum + secNum);
				else if(temp == '-') stack.push(firstNum - secNum);
				else if(temp == '*') stack.push(firstNum * secNum);
				else if(temp == '/') stack.push(firstNum / secNum);
			}
			else {
				stack.push(numArr[temp-'A']);
			}
		}
		System.out.printf("%.2f", stack.peek());
	}
}

```
