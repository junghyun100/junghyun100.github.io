---
layout: post
title: "Dictionary/HashMap 기반 조회와 LINQ/Stream 기반 필터링 성능 비교"
tags: [성능 개선]
comments: true
---
 
해당 Post는 실무에서 대용량 데이터를 다루는 애플리케이션에서 성능을 올리기 위해 고민하다가 알게된 점을 작성합니다.

예를 들어, 조회해야하는 상품 혹은 SKU의 정보가 100만 건 이상의 데이터를 기반으로 어떤 데이터를 추출해야하는 상황이 있을 수 있습니다.

C#의 LINQ와 Java의 Stream은 가독성과 편리함을 제공하지만, 100만 건 이상의 데이터에서는 성능 병목을 초래할 수 있습니다.

이번 포스트에서는 Dictionary/HashMap 기반 조회와 LINQ/Stream 기반 필터링의 성능을 비교하고,

대규모 데이터 처리에서 어떤 방식을 선택했는지 정리해봅니다. 

---

### 시나리오: 어떤 문제를 해결할까?

다음 상황을 가정합니다.

1. List<객체 A>와 List<객체 B>는 각각 100만 건의 데이터를 포함하고 있습니다.

2. 객체 A에서 객체 B에 포함된 항목만 추출해 새로운 리스트 생성하고자 합니다.

3. 객체 A와 B는 예시이므로 간단한 구조를 가지고, ID를 기준으로 매핑한다고 가정합니다.
 
```java
// C# 객체 예시
public class ObjectA { 
    public int Id { get; set; } 
    public string Name { get; set; } 
}
public class ObjectB { 
    public int Id { get; set; } 
    public string Category { get; set; } 
}
```
```java
// Java 객체 예시
@Getter
@Setter
public class ObjectA {
    private int id;
    private String name;
}
@Getter
@Setter
public class ObjectB {
    private int id;
    private String category;
}
```

## 두 가지 로직을 비교합니다.

Dictionary/HashMap 방식: 객체 B의 리스트를 Dictionary(C#) 또는 HashMap(Java)으로 변환 후, 객체 A를 반복하며 조회

LINQ/Stream 방식: 객체 A에서 LINQ의 Where(C#) 또는 Stream의 filter(Java)를 사용해 객체 B에 포함된 항목 필터링

## 성능 분석: 이론적으로 어떤 방식이 빠를까?

1. Dictionary/HashMap 방식

* 작동 원리:
  * 객체 B의 리스트를 Dictionary/HashMap으로 변환 (키: ID, 값: 객체 B)
  * 객체 A를 순회하며 Dictionary/HashMap에서 ID를 조회해 매칭된 항목을 새 리스트에 추가


* 시간복잡도:
  * Dictionary(C#)와 HashMap(Java)은 해시 테이블 기반, 평균 조회 시간복잡도 O(1)
  * 객체 B를 Dictionary/HashMap으로 변환: O(m) (m: 객체 B 크기, 100만)
  * 객체 A를 순회하며 조회: O(n) (n: 객체 A 크기, 100만)
  * 총 시간복잡도: O(n + m), 즉 약 200만 연산


* 메모리: 해시 테이블로 추가 메모리 사용 (100만 건 기준 약 2~3배)

2. LINQ/Stream 방식

* 작동 원리:
   * 객체 A의 리스트에서 LINQ의 Where 또는 Stream의 filter를 사용
   * 각 객체 A의 ID가 객체 B 리스트에 있는지 확인 (Contains 또는 anyMatch의 방식)


* 시간복잡도:
   * 객체 B의 Contains/anyMatch는 O(m), 즉 100만 연산
   * 객체 A의 각 원소(100만)마다 호출하므로 총 시간복잡도: O(n * m), 즉 약 1조 연산
   * 100만 건 데이터에서는 심각한 성능 저하 발생


* 메모리: 추가 자료구조 없이 리스트만 사용, 하지만 **속도가 문제**이다.


---
