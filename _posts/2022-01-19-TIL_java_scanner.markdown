---
layout: post
title:  "Scanner"
date:   2022-01-19 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA 에서의 Scanner에 대해 알아보고자 한다.

<h1>Scanner Class</h1>
* 읽은 바이트를 문자, 정수, 실수, boolean, 문자열 등 __다양한 타입으로 변환하여 return하는 클래스이다.__ (단, char는 없음)
* java.util.scanner
* Scanner는 입력되는 키 값을 공백으로 구분되는 토큰 단위로 읽는다.
* 공백 문자로는 '\t', '\f', '\r', '', '\n' 등이 있다.


<br>
<h2>Scanner 기본 사용 방법</h2>

```java
import java.util.Scanner;

public class ScannerEx01{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in); // Scanner 객체인 sc 생성
    }
}

```
* System.in을 사용하여 키보드로 입력 값을 읽고 원하는 타입으로 변환하여 리턴한다.


<br>
<h2>System.in이란?</h2>
* 키보드와 연결된 __자바의 표준 입력 스트림__ 이다.
* 입력되는 키를 byte로 리턴하는 저수준 스트림이다.
* System.in을 직접 사용하면 byte를 문자나 숫자로 변환하는 많은 어려움이 있다.

<br>
<h2>Scanner를 사용한 간단한 예시</h2>

```java
import java.util.Scanner;

public class ScannerEx02{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        String name = sc.next(); // "Syon"
        String city = sc.next(); // "Gumi"
        int age = scan.nextInt(); // 28..?..
    }
}

```

<br>
<h2>Scanner의 주요 Method</h2>
<table><thead><tr><th>메소드</th><th>설명</th></tr></thead><tbody><tr><td>String next()</td><td>다음 토큰을 문자열로 리턴</td></tr><tr><td>byte nextByte()</td><td>다음 토큰을 byte 타입으로 리턴</td></tr><tr><td>short nextShort()</td><td>다음 토큰을 short 타입으로 리턴</td></tr><tr><td>int nextInt()</td><td>다음 토큰을 int 타입으로 리턴</td></tr><tr><td>long nextLong()</td><td>다음 토큰을 Long 타입으로 리턴</td></tr><tr><td>float nextFloat()</td><td>다음 토큰을 float 타입으로 리턴</td></tr><tr><td>double nextDouble()</td><td>다음 토큰을 double 타입으로 리턴</td></tr><tr><td>String nextLine()</td><td>'\n'을 포함하는 한 라인을 읽고, '\n'을 버린 나머지만 리턴</td></tr><tr><td>void close()</td><td>Scanner의 사용 종료</td></tr><tr><td>boolean hasNext()</td><td>현재 입력된 토큰이 있으면 true, 아니면 새로운 입력이 들어올 때까지 기다린 후 true 리턴. ctrl+z가 입력되면 false 리턴</td></tr></tbody></table>

<br>
<h3>Example</h3>

```java
import java.util.Scanner;

public class ScannerEx03{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        String name;
        int age;
        
        System.out.println("Enter your age");
        age = sc.nextInt();
        System.out.println("Enter your name");
        name = sc.nextLine();
        
        System.out.printf("Your age is %d.%n", age);
        System.out.printf("Your name is %s.%n", name);

        sc.close();

    }
}

```
* 위와 같은 예제를 작성해서 실제로 25를 입력하게 되면 아래와 같은 결과를 얻을 수 있다.

```java
Enter your age
>> 25
Enter your name
Your age is 25.
Your name is .

```
* nextInt()와 같이 타입을 지정해서 받는 메서드는 'Enter'값을 무시하고 해당 타입만 받아 변환 후 반환하는데, 이때 컴퓨터 내부에서는 'Enter'값이 아직 남아있기 때문에 NextLine()에서 'Enter'값을 받아들이고 그대로 입력되어 종료되는 것이다. 분명 입력할 때에는 25와 엔터만 눌렀을 뿐인데도 말이다.
<br>
<br>
* 한가지 더, "scan을 굳이 close()로 닫아줄 필요는 없지만, 닫아주는 습관을 기르도록 하자" 라고 본문에서는 이야기하고 있지만, 굳이 닫을 필요는 없다. [왜냐하면][설명2], System.in은 외부로부터 입력을 받는데 스트림을 이용해 입력을 받지만, 거의 모든 입력에 스트림 인스턴스 사용 후 실제 닫을 필요가 없다는 공식 사이트 글이 있기 떄문이다. 리소스가 IO 채널(외부 네트워크 또는 파일 등)일 때에만 스트림을 닫아주면 된다고 한다. 아래는 공식 문서에서 관련 내용을 가져온 것이다.
<br>
<br>
Streams have a BaseStream.close() method and implement AutoCloseable, but nearly all stream instances do not actually need to be closed after use. Generally, only streams whose source is an IO channel (such as those returned by Files.lines(Path, Charset)) will require closing. Most streams are backed by collections, arrays, or generating functions, which require no special resource management. (If a stream does require closing, it can be declared as a resource in a try-with-resources statement.)



<br>
[설명 출처][설명]<br>

[설명]: https://mine-it-record.tistory.com/103
[설명2]: https://okky.kr/article/915691?note=2308988