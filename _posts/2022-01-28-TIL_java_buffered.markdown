---
layout: post
title:  "BufferedReader & BufferedWriter"
date:   2022-01-28 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 bufferedReader와 bufferedWriter에 대해 알아보고자 한다.


<br>
<h2>BufferedReader & Writer</h2>
* 이름처럼 버퍼를 이용하여 읽고 쓰는 IO Class이다.
* 이는 버퍼를 이용하기 때문에 이 클래스를 이용하면 입력된 데이터가 바로 전달되지 않고 중간에 버퍼링이 된 후 전달된다.
* 출력도 마찬가지로 버퍼를 거쳐 간접적으로 출력장치에 전달되므로 시스템의 데이터 처리 효율성을 높여준다.
* BufferStream을 InputStreamReader & OutputStreamReader를 같이 사용하여 버퍼링을 하게되면 입출력 스트림으로부터 미리 버퍼에 데이터를 갖다 놓기 때문에 보다 효율적인 입출력이 가능해진다.


<br>
<h4>BufferedReader</h4>
* 무언가를 입력받을때는 보통 Scanner를 사용한다. 이를 통해 입력받을 경우 Space & Enter를 경계로 인식하기에 입력받은 데이터를 가공하기 편하다.
* 그러나 BufferedReader의 경우 Enter만 경계로 인식하고 받은 데이터가 String으로 고정되기에 입력받은 데이터를 가공해야만 하는 경우가 많다.
* Scanner에 비해 사용하기 불편하나, 많은 양의 데이터를 입력받는 경우 BufferedReader를 통해 입력받는 것이 효율면에서 훨씬 낫다.


<h4>Buffer가 Scanner보다 빠른 이유</h4>
<p align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcd5i1j%2FbtqBpQikC4p%2FayNp6PzkvFekkqgeDuHgq0%2Fimg.png" width="65%" height="65%" title="버퍼"/>
</p><br>
* 속도가 빠른 것은 버퍼를 이용하기 떄문이다.
* 입력도 사용자가 키보드에 누를 때마다 전달하는 것이 아니라 버퍼에 용량만큼 모았다가 전달하는 것이다.
* 잘 생각해보면, 내가 어떤 물건들을 옮길 때 하나씩 옮기는 것보다 한번에 최대로 옮길 수 있는 만큼 옮겨야 덜 힘들고 빠를 것이다.
* 출력도 마찬가지로 버퍼를 거쳐 간접적으로 출력장치로 전달되기에 시스템데이터 처리 효율성을 높여주며, 버퍼스트림을 InputerStreamReader & OuptutStreamReader를 같이 사용하여 버퍼링하게되면 입출력 스트림으로부터 미리 버퍼에 데이터를 갖다 놓기 때문에 보다 효율적인 입출력이 가능하다.
* 공식 문서에는 <i>Reads text from a character-input stream, buffering characters so as to provide for the efficient reading of characters, arrays, and lines. The buffer size may be specified, or the default size may be used. </i> 라고 되어있는데, 이는 입력 스트림에서 문자를 읽는 함수이고, 문자나 배열, 라인들을 효율적으로 읽기 위해서 문자들을 버퍼에 저장하고 읽는 방법을 취한다고 한다. 버퍼 사이즈는 우리가 정할 수도 있지만, 지정하지 않을 경우에는 기조 디폴트 사이즈가 사용된다고 적혀있다.


```java
public static void main(String[] args) throws IOException{
    BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
    String s = bf.readLine();
    int i = Integer.parseInt(s);
}

```
* readLine() 메서드를 사용하면 데이터를 라인 단위로 읽을 수 있다. Scanner에서 nextLine()과 같다.
* readLine() 메서드의 리턴 값은 String으로 고정되기 때문에 String이 아닌 다른 타입으로 입력을 받으려면 형변환을 꼭 해주어야 한다.
* 예외처리 또한 꼭 해줘야 한다. readLine()을 할 때마다 try & catch를 활용해도 되지만 대개 throws IOEXception을 통해 작업한다.


그렇다면, 알고리즘을 풀다보면 한 줄에 입력을 여러개 받는 상황이 오는데, 이런 상황에는 어떻게 할까?<br>
이런 상황에는 StringTokenizer 클래스 또는 String 클래스의 메서드 중 하나인 Split()을 사용하면 된다.

```java
public static void main(String[] args) throws IOException{
    BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
    String s = bf.readLine();
    StringTokenizer st = new StringTokenizer(s);
    int a = Integer.parseInt(st.nextToken());
    int b = Integer.parseInt(st.nextToken());

    String p = "abcdef";
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    bw.write(p + "\n");

    // write한다고 해서 바로 출력되지 않는다.
    // 직접 출력 Stream에 반영되는 것이 아니라 성능을 위해 Buffer에 담고 있다가 BufferedWriter가 Flush되거나 close되었을 때 한번에 출력 Stream에 반영한다.
    bw.flush();
    bw.write("3");

    // close는 Stream을 닫아버리기 때문에 계속 출력하고자 한다면 flush를 사용한다.
    // bw.close()

    // 출력 내용에 줄바꿈이 필요하다면 newLine 함수를 사용한다.
    bw.write(String.valueOf(b)); // write는 String형만 출력이 가능하므로 정수는 String으로 변환해야 한다.

    bw.flush();
    bw.close();
}

```
여기서 write() 메서드는 String형으로만 출력이 가능하기 때문에 정수를 출력하려면 String.valueOf() 메서드를 이용하여 String으로 변환을 한 후에 출력해야 한다.<br><br>
BufferedWriter의 경우 버퍼를 잡아 놓았기 떄문에 반드시 flush() & close() 메서드를 호출해주어 뒤처리를 해줘야 한다. 그리고 bw.write에는 System.out.println()고 같은 개행기능이 없기 때문에 개행을 해주는 경우 \n을 사용하여 따로 처리해야 한다.


<h4>주요 메서드</h4>
<table><thead><tr><th>메서드 명</th><th>기능</th></tr></thead><tbody><tr><td>BufferedReader(Reader rd)</td><td>rd에 연결되는 문자 입력 버퍼 스트림 생성</td></tr><tr><td>BufferedWriter(Writer wt)</td><td>wt에 연결되는 문자 출력 버퍼 스트림 생성</td></tr><tr><td>Int read()</td><td>스트림으로부터 한 문자를 읽어서 int 형으로 반환</td></tr><tr><td>int read(char[] buf)</td><td>문자 배열 buf의 크기만큼 문자를 읽어들이고 읽어들인 문자 수를 반환</td></tr><tr><td>int read(char[] buf, int offset, int length)</td><td>buf의 offset 위치부터 length 만큼 문자를 스트림으로부터 읽어들임</td></tr><tr><td>String readLine()</td><td>스트림으로부터 한 줄을 읽어 문자열로 반환</td></tr><tr><td>void mark()</td><td>현재 위치를 마킹하고 추 후 Reset() 을 이용하여 마킹 위치부터 시작</td></tr><tr><td>void reset()</td><td>마킹이 있으면 그 위치에서부터 다시 시작하고, 그렇지 않으면 처음부터 다시 시작</td></tr><tr><td>long skip(int n)</td><td>n 개의 문자를 건너 뜀</td></tr><tr><td>void close()</td><td>스트림 닫음</td></tr><tr><td>void write(int c)</td><td>int 형으로 문자 데이터를 출력 문자 스트림으로 출력</td></tr><tr><td>void write(String s, int offset, int length)</td><td>문자열 s를 offset 위치부터 length 길이만큼 출력 스트림으로 출력</td></tr><tr><td>void write(char[] buf, int offset, int length)</td><td>문자 배열 buf의 offset 위치부터 length 길이만큼 출력 스트림으로 출력</td></tr><tr><td>void newLine()</td><td>줄바꿈 문자열 출력</td></tr><tr><td>void flush()</td><td>남아있는 데이터를 모두 출력시킴</td></tr></tbody></table><br><br>


[설명 출처][설명]<br>
[설명 출처2][설명2]<br>

[설명]: https://devlog-wjdrbs96.tistory.com/96
[설명2]: https://coding-factory.tistory.com/251