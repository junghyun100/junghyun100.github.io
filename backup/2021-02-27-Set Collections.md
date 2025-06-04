---
layout: post
title: "Set Collections"
tags: [Java Framework, JAVA, Set]
comments: true

---

Java Framework의 Set에 대해 정리를 한 내용 입니다.

---

# Hierarchy of Collection Framework

<img src="https://static.javatpoint.com/images/java-collection-hierarchy.png">

컬렉션 프레임 워크의 계층 구조는 위와 같습니다.

이 중에서 `Set`에 대해서 알아봅니다.

## Set Collections

`Set 인터페이스`를 구현한 모든 `Set 컬렉션 클래스`는 다음과 같은 특징을 가집니다.

1. 요소의 저장 순서를 유지하지 않습니다.
2. 같은 요소의 중복 저장을 허용하지 않습니다.

> 순서를 보장하지 않는다는 말은 <br>hashCode 값에 따라 순서가 정해져서 나오기 때문입니다.

대표적인 `Set 컬렉션 클래스`에 속하는 클래스는 다음과 같습니다.

1. HashSet
2. LinkedHashSet
3. TreeSet

## List와 Set의 차이점

`Set의 특징`을 잘 보여주는 예시입니다.

예시에서는 `ArrayList`와 `HashSet`을 이용해서 비교합니다.

```java
import java.util.ArrayList;
import java.util.HashSet;

import java.util.Iterator;

class Main {

  public static void main(String[] args) {
        ArrayList<String> al = new ArrayList<String>();
        al.add("one");
        al.add("two");
        al.add("two");
        al.add("three");
        al.add("three");
        al.add("five");
        System.out.println("array");
        Iterator ai = al.iterator();
        while(ai.hasNext()){
            System.out.println(ai.next());
        }

        HashSet<String> hs = new HashSet<String>();
        hs.add("one");
        hs.add("two");
        hs.add("two");
        hs.add("three");
        hs.add("three");
        hs.add("five");
        Iterator hi = hs.iterator();
        System.out.println("\nhashset");
        while(hi.hasNext()){
            System.out.println(hi.next());
        }
    }
}
```

이것의 결과로 볼 수 있는 특징은

<strong>List는 중복을 허용하고, Set은 허용하지 않는다</strong>는 점입니다.

결과값은 아래와 같습니다.

```java
array
one
two
two
three
three
five

hashset
one
two
three
five
```

`two`와 `three`의 값이 `hashSet`에는 중복이 되므로 걸러진 채로

출력이 됨을 확인할 수 있습니다.

### 출력에 사용된 iterator에 대해

메소드 `iterator`의 호출 결과는 인터페이스 `iterator`를 구현한 객체를 리턴합니다.

인터페이스 `iterator`는 아래 3개의 메소드가 있고, 각각의 역할은 아래와 같다.

<img src="/images/2021년/0227/Iterator.PNG">

* hasNext : 반복할 데이터가 더 있으면 true를 반환하고 그렇지 않으면 false를 반환합니다.

* next : hasNext가 true라는 것은 next가 리턴할 데이터가 존재한다는 의미입니다.

* remove : 반복자가 반환 한 마지막 요소를 제거합니다. (1개가 덜 사용됨)

## HashSet 테스트

<img src="/images/2021년/0227/HashSet.PNG">

`HashSet`을 확인해보고자 했습니다.

<img src="/images/2021년/0227/HashSet-R.PNG">

결과는 위 그림 처럼 나왔습니다.

`asdfasdf`,`Vijay`, `Ajay`가 들어가지만 걸러질 2번의 `asdfasdf`가 있었습니다.

그럼에도 불구하고 결과값으로 출력되는 것은 순서가

`Vijay`, `asdfasdf`, `Ajay`의 순서로 출력이 되었습니다.

이것이 <strong>"요소의 저장 순서를 유지하지 않습니다."</strong> 라는 특성으로 이해했습니다.

## LinkedHashSet 테스트

<img src="/images/2021년/0227/LinkedHashSet.PNG">

`LinkedHashSet` 확인해보고자 했습니다.

<img src="/images/2021년/0227/LinkedHashSet-R.PNG">

결과는 위 그림 처럼 나왔습니다.

위에 있던 `HashSet`과 비교해 보았을 때 

<strong>삽입 순서를 유지한다</strong>는 사실을 알 수 있었습니다.

## TreeSet 테스트

<img src="/images/2021년/0227/TreeSet.PNG">

`TreeSet` 확인해보고자 했습니다.

<img src="/images/2021년/0227/TreeSet-R.PNG">

결과는 위 그림 처럼 나왔습니다.

`TreeSet`의 요소는 `오름차순`으로 저장됩니다.

`TreeSet`의 가장 큰 특징은 오름차순으로 정렬되고 Tree 형태로 저장되어,

액세스 및 검색 시간은 매우 빠르다는 점입니다.

## 참고자료

<a href="https://www.javatpoint.com/collections-in-java">javatpoint</a>

<a href="https://opentutorials.org/module/516/6446">생활코딩</a>

---
