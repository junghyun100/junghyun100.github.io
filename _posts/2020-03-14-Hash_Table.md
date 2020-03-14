---
layout: post
title: "Hash Table이란 무엇인가요?"
tags: [개념]
comments: true
---
---
### Hashtable

 
일반적으로 동기화가 필요 없다면 HashMap을, 동기화 보장이 필요하다면 Hashtable을 사용하면된다.

#### 1. HashMap은 비동기, HashTable은 동기이다

* 단일 Thread에서는 HashMap이, 멀티 Thread에서는 HashTable을 사용

* 단일 Thread에서 HashTable보다 HashMap의 성능이 더 좋음 : HashTable은 동기화를 위한 비용이 포함되기 때문

#### 2. HashMap은 하나의 null key와 다수의 null value가 허용되지만, HashTable에서는 null을 불허

* HashTable은 객체를 저장하거나 불러올때 key가 hashCode 메소드와 equals 메소드를 사용하기 때문에 null을 불허

* 사용법도 똑같아, put() 메서드로 데이터를 삽입하고, get() 메서드로 추출하면된다.


#### Hashtable의 특징

Vector는 데이터(Data)에 해당하는 객체만을 이용했지만,

Hashtable은 Map 인터페이스를 구현한 클래스이기 때문에 검색을 위한 키와 값을 함께 넣어주어야 한다.

> Hashtable의 사용 예

```java
Hashtable h = new Hashtable(); //객체생성
h.put("name", new String("홍길동")); //데이터 삽입
String name = (String)h.get("Name"); //키를 이용한 데이터 추출(다운캐스팅 필요)
```
 > 공부하는 중이니 다운캐스팅에 대해서는 다음에 조사해봐야겠습니다.

만약 Vector의 경우에 데이터를 찾고자 한다면 IndexOf()나 elementAt()으로 전체 데이터를 검색하면서 비교해야 한다.

하지만 Hashtable은 키만을 이용해서 간단하게 검색할 수 있다.

출처: https://firedev.tistory.com/entry/Java-HashMap과-Hashtable-의-차이점 [개발노트]

```java
import java.util.Enumeration;
import java.util.Hashtable;

public class testHashtable
  {
    public static void main(String args[])
      {
        Hashtable<String, Integer> ht = new Hashtable<String, Integer>();

        // 해시 테이블에 키와 값 집어 넣기
        ht.put("abc", 1);
        ht.put("abc1", 2);
        ht.put("abc2", 3);
       
        // 해시 테이블에 있는 값 꺼내오기
        Enumeration en = ht.keys();

        while(en.hasMoreElements())
        {
          String key = en.nextElement().toString();
          System.out.println(key + " : "+ht.get(key));
         }
       }
    }
```
