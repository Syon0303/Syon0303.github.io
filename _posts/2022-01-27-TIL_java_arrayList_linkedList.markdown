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
위의 생성자를 이용하여 ArrayList를 만들게 되면 DEFAULT_CAPACITY = 10으로 정의되어 있다. 한마디로 배열의 크기가 10으로 지정된 것과 같다고 생각하면 된다.


[설명 출처2][설명2]<br>

[설명2]: https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95