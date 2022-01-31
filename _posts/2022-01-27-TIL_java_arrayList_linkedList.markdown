---
layout: post
title:  "ArrayList & LinkedList"
date:   2022-01-27 19:17:36 +0900
categories: java
---
이번 포스팅에선 JAVA에서의 ArrayList와 LinkedList에 대해 알아보고자 한다.


<br>
<h2>List</h2>
* List는 모든 프로그래밍 언어에서 가장 유용한 자료구조 중 하나이다.
* 배열의 크기는 정해져 있기 때문에 자료형의 크기가 가변하는 상황이라면 List를 사용하는 것이 효율적이다.
* List의 구현체로는 Stack, Vector, ArrayList, LinkedList가 있다.
* 이번 포스팅에선, ArrayList와 LinkedList에 대해 집중적으로 보고자 한다.


<br>
<h4>ArrayList</h4>
* 중복을 허용하고 순서를 유지하며 인덱스로 원소들을 관리한다.
* 배열은 크기가 지정되면 고정되지만 ArrayList는 클래스이기 때문에 배열을 추가, 삭제 할 수 있는 메서드가 존재한다.
* 그러나 추가 시 배열이 동적으로 늘어나는 것이 아니라 용량이 꽉 찬 경우 더 큰 용량으 ㅣ배열을 만들어 옮기는 작업을 한다.


```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable{
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ELEMENTDATA = {};
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    transient Object[] elementData;
    private int size;

    public ArrayList(Int initialCapacity){
        if(initialCapacity > 0){
            this.elementData = new Object[initialCapacity];
        }
        else if(initialCapacity == 0){
            this.elementData = EMPTY_ELEMENTDATA;
        }
        else{
            throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
        }
    }

    public ArrayList(){
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    public ArrayList(Collection<? extends E> c){
        Object[] a = c.toArray();
        if((size = a.length) != 0){
            if(c.getClass() == ArrayList.class){
                elementData = a;
            }
            else{
                elementData = Arrays.copyOf(a, size, Object[].class);
            }
        }
        else{
            elementData = EMPTY_ELEMENTDATA;
        }
    }
}
```
ArrayList는 위와 같이 3개의 생성자가 존재한다.
* 아무 것도 매개변수로 받지 않는 생성자
* 초기 용량을 매개변수로 받는 생성자
* Collection 타입을 매개변수로 받는 생성자

이 중 두번째 생성자에 대해 알아보자.

```java
List<Integer> list = new ArrayList<>();

```
보통 ArrayList의 객체를 만들 때 위와 같이 만들게 된다. 위와 같이 만들면, 아래와 같은 매개변수가 존재하지 않는 생성자가 만들어진다.

```java
private static final int DEFAULT_CAPACITY = 10;
public ArrayList(){
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

```
위의 생성자를 이용하여 ArrayList를 만들게 되면 DEFAULT_CAPACITY = 10으로 정의되어 있다. 한마디로 배열의 크기가 10으로 지정된 것과 같다고 생각하면 된다.<br>
만약, 10보다 더 많은 원소를 넣으면 어떻게 될까?<br>
용량이 초과되었을 때 내부 코드는 어떻게 동작하는지 알아보자.

```java
public class ArrayList<E>{
    public boolean add (E e){
        ensureCapacityInternal(size + 1);
        elementData[size++] = e;
        return true;
    }
}

```
ArrayList 안에는 배열에 값을 추가할 수 있는 add() 메서드가 존재한다. ensureCapacityInternal()이 보이는데, 여기서 아마 배열 용량을 늘리는 작업이 일어날 것 같다.

```java
public class ArrayList<E>{
    private void ensureCapacityInternal(int minCapacity){
        ensureExplictCapacity(calculateCapacity(elementData, minCapacity));
    }

    private void ensureExplicitCapacity(int minCapacity){
        modCount++;

        if(minCapacity - elementData.length > 0)
            grow(minCapacity);
    }

    private void grow(int minCapacity){
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0){
            newCapacity = minCapacity;
        }
        if (newCapacity - MAX_ARRAY_SIZE > 0){
            newCapacity = hugeCapacity(minCapacity);
        }
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
}

```
ArrayList 내부의 메서드인데 코드는 위와 같이 되어있다. 복잡해 보이지만, 중요한 부분은 grow() 메서드 내부에서 Arrays.copyOf(elementData, newCapacity);를 통해서 더 큰 배열에다 기존 배열의 원소들을 복사한다는 점이다. Array.copyOf() 내부 코드를 보면 알 수 있지만, 실제 A 배열을 B 배열로 옮기는 과정은 원소의 수가 얼마 안되면 괜찮겠지만 많다면 상당히 많은 시간이 소요되고 효율적이지 못하다.


<h4>ArrayList 객체를 만들 때 초기 용량을 설정하자</h4>
위와 같이 초기 용량을 설정하지 않으면 DEFAULT_CAPACITY = 10이다. 많은 원소가 추가, 삭제되는 상황이라면, 빈번하게 배열으 ㅣ복사가 일어날 것이다. 물론 초기 용량을 미리 예상하기는 쉽지않겠지만 대략적으로 초기 용량을 생각하고, 그 예상하는 값보다 살짝 더 여유있는 값으로 초기 용량을 설정해주는 것이 좋다.


<h4>add()를 통해서 ArrayList 용량이 꽉 찬다면?</h4>

```java
int newCapacity = oldCapacity + (oldCapacity >> 1);
```
용량을 늘리는 코드에서 위와 같은 코드가 있다. 이는 oldCapacity + oldCapacity / 2로 늘리고 있는 것이다. oldCapacity가 8이라면 8+4 = 12로 늘어난다는 것이다.


<h4>ArrayList API</h4>
* add(E element) : 원소를 마지막에 추가하기. 이는 배열의 마지막에 원소를 추가하는 것이기 때문에 빠르게 추가할 수 있다.
* add(int index, E element) : 원소를 지정된 위치에 추가하기. 이는 삽입한 요소 뒤로 모든 값의 인덱스를 하나씩 미뤄야 하기 때문에 이 과정에서 시간이 오래 걸린다.
* remove(int index) : 원소의 인덱스로 삭제하기. 이는 마지막 원소를 삭제한다면 쉽게 삭제할 수 있지만, 중간이나 처음의 원소를 삭제하게 되면 빈 공간을 다시 채워야 하는 과정이 필요하기 떄문에 효율적이지 못하다.
* get(int index) : 인덱스에 해당하는 원소 찾아오기. 배열은 인덱스에 해당하는 원소를 O(1)만에 찾아올 수 있기 때문에 탐색에는 매우 유리하다.
* 정리하면, ArrayList는 탐색은 빠르게 할 수 있지만, 중간에서 추가 또는 삭제가 빈번하게 일어나면 비효율적이라는 특징을 갖고 있다.


<br>
<h2>LinkedList</h2>
<p align="center">
    <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/1335/2949.png" width="100%" height="100%" title="데이터 표현 범위"/>
</p><br>
LinkedList는 내부적으로 양방향의 연결 리스트로 구성되어 있어서 참조하려는 원소에 따라 처음부터 순방향 또는 역방향으로 순회할 수 있다. 이는 배열의 단점을 보완하기 위해 고안된 자료구조이며, 배열의 단점은 아래와 같다.

* 크기를 변경할 수 없다. 즉, 새로운 배열을 생성해서 복사해야 한다.
* 실행속도를 향상시키기 위해서는 충분히 큰 용량을 미리 정해놔야하는데 이것이 메모리 낭비가 될 수 있다.
* 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.

LinkedList는 바로 API를 보면서 알아보자.

* add(E element) : 원소를 마지막에 추가하기. LinkedList도 ArrayList와 마찬가지로 add()메서드가 존재한다. 그러나 LinkedList는 배열처럼 인덱스를 갖고 있지 않다. 따라서 원소를 추가하기 위해서는 Head에서부터 마지막까지 찾아가야 하기 떄문에 시간이 많이 걸린다.
* add(int index, E element) : 원소를 지정된 위치에 추가하기. 인덱스를 지정해서 추가하는 것도 마찬가지로 해당 위치로 가려면 처음부터 탐색해서 가야하기 때문에 시간이 걸린다.
* remove(int index) : 원소를 삭제하기. 원소를 삭제하려면 배열의 경우 빈 공간을 다시 채워주는(index를 당겨주는) 작업이 필요하지만, LinkedList는 삭제하려는 원소 앞이나 뒤로 가서 가르키는 값을 Null로 변경해주면 그만이다.
* get(int index) : 인덱스에 해당하는 원소 찾아오기. LinkedList는 ArrayList와 다르게 인덱스를 통해 검색을 하는 것이 아니라 처음부터 해당 원소까지 검색해야 하기 떄문에 O(n)만큼의 시간이 걸린다.


<h4>시간 계산해보기</h4>

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class ArrayListLinkedListTest {
    public static void main(String[] args) {
        ArrayList al = new ArrayList(2000000);
        LinkedList ll = new LinkedList();

        System.out.println("= 순차적으로 추가하기 =");
        System.out.println("ArrayList : " + addl(al));
        System.out.println("LinkedList : " + addl(ll));
        System.out.println();
        System.out.println("= 중간에 추가하기 =");
        System.out.println("ArrayList : " + add2(al));
        System.out.println("LinkedList : " + add2(ll));
        System.out.println();
        System.out.println("= 중간에서 삭제하기 =");
        System.out.println("ArrayList : " + remove2(al));
        System.out.println("LinkedList : " + remove2(ll));
        System.out.println();
        System.out.println("= 순차적으로 삭제하기 =");
        System.out.println("ArrayList : " + remove1(al));
        System.out.println("LinkedList : " + remove1(ll));
    }

    public static long addl(List list) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < 1000000; i++) {
            list.add(i+"");
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    public static long add2(List list) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000; i++) {
            list.add(500, "X");
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    public static long remove1(List list) {
        long start = System.currentTimeMillis();

        for (int i = list.size() - 1; i >= 0; i--) {
            list.remove(i);
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    public static long remove2(List list) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000; i++) {
            list.remove(i);
        }

        long end = System.currentTimeMillis();
        return end - start;
    }
}

```
<br>

```
= 순차적으로 추가하기 =
ArrayList : 126
LinkedList : 171

= 중간에 추가하기 =
ArrayList : 1695
LinkedList : 10

= 중간에서 삭제하기 =
ArrayList : 1303
LinkedList : 122

= 순차적으로 삭제하기 =
ArrayList : 8
LinkedList : 23
```
1. 순차적으로 추가하기
* ArrayList : 순차적으로 추가하면 배열 원소의 이동 없이 추가만 하면 되서 금방 한다.
* LinkedList : LinkedList는 순차적으로 추가하면 그 추가하고자 하는 곳으로 계속 탐색해서 가야한다. 그러나 내부적으로 양방향으로 되어있기 떄문에 ArrayList와 큰 차이는 나지 않는다.

2. 중간에 추가하기
* ArrayList : 중간에 추가하게 되면 빈 공간을 만들어야 하기 때문에 원소들의 이동이 필요하고, 그렇기에 상당히 비효율적이다.
* LinkedList : 중간에 추가할 때는 추가하고자 하는 원소 앞 또는 뒤의 노드로 가서 가리키고 있는 주소만 추가해주면 되기 때문에 금방 한다.

3. 중간에서 삭제하기
* ArrayList : 중간에서 삭제하는 것도 중간에 추가하기와 마찬가지로 빈 공간이 생기기 떄문에 채우기 위해서 원소들의 이동이 일어나므로 시간이 오래 걸린다.
* LinkedList : 중간에서 삭제하는 것도 추가하기와 마찬가지의 과정으로 금방 한다.

4. 순차적으로 삭제하기
* ArrayList : 순차적으로 마지막 원소를 삭제할 때는 원소들의 이동이 필요 없기에 시간이 오래 걸리지 않는다.
* LinkedList : 내부적 양방향 연결리스트로 되어있기 떄문에 ArrayList와 큰 차이가 없는 것을 볼 수 있다.


<h4>결론</h4>
<table><thead><tr><th>컬렉션</th><th>읽기(접근 시간)</th><th>추가 및 삭제</th><th>비고</th></tr></thead><tbody><tr><td>ArrayList</td><td>빠르다</td><td>느리다</td><td>순차적인 추가 삭제는 더 빠름<br>비효율적인 메모리 사용</td></tr><tr><td>LinkedList</td><td>느리다</td><td>빠르다</td><td>데이터가 많을수록 접근성이 떨어짐</td></tr></tbody></table>

<br>

__다루고자 하는 데이터의 수가 변하지 않는다면, ArrayList가 최상의 선택이겠지만 데이터 수의 변경이 잦다면 LinkedList를 사용하는 것이 더 나은 선택이 될 수도 있다.__



[설명 출처2][설명2]<br>

[설명2]: https://devlog-wjdrbs96.tistory.com/64