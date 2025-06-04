---
layout: post
title: "Spring Data JPA"
tags: [JPA]
comments: true

---

JPA를 공부하면서 정리를 한 내용들 입니다.

-참고 김영한 개발자님 : [토크ON세미나] JPA 프로그래밍 기본기 다지기 - Spring Data JPA | T아카데미

---

# 8강. Spring Data JPA

## 반복되는 CRUD

JPA를 써서 많은 것을 자동화 했음에도 불구하고

여전히 비슷한 기존의 방식과 비슷한게 많았습니다.

Spring Data JPA는 지루하게 반복되는 CRUD 문제를 세련된 방법으로 해결해줍니다.

개발자는 인터페이스만 작성해도 됩니다.

스프링 데이터 JPA가 구현 객체를 동적으로 생성해서 주입시켜줍니다.

### Spring Data JPA 적용 전

```java
public class MemberRepository {
  
  public void save(Member member) {...}
  public Member findOne(Long id) {...}
  public List<Member> findAll() {...} 
  
  public Member findByName(String name) {...} 
}

public class ItemRepository {

  public void save(Member member) {...}
  public Member findOne(Long id) {...}
  public List<Member> findAll() {...}
}
```

공통된 내용이 Repository 내에서 보입니다.

그래서 이러한 부분을 개선하고자 해서 Spring Data JPA가 사용되었습니다.

### Spring Data JPA 적용 후

Spring Data JPA가 JpaRepository 인터페이스를 제공합니다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    Member findByName(String name);
}

public interface ItemRepository extends JpaRepository<Item, Long> {
    // 비어있음
}
```

공통화 할 수 없었던 Member객체의 findByName()을 제외하고는 인터페이스에서 제공됩니다.

로딩 시점에 MemberRepository와 ItemRepository의 구현체를 생성해줍니다.

### 공통 인터페이스 기능
JpaRepository 인터페이스는 공통 CRUD를 제공해주고,

제네릭은 <엔티티, 식별자>로 설정합니다.

### 메서드 이름만으로 JPQL 쿼리 생성

```java
public interface MemberRepository extends JpaRepository<Member, Long> {
List<Member> findByName(String name); 
}
```
이렇게만 작성하면 JPQL를 자동으로 작성해줍니다.

<strong>예제 1</strong>

```java
List<Member> member = memberRepository.findByName("hello")
```
위 코드를 작성하면 실제 실행된 SQL은 
```sql
SELECT*FROM MEMBER M WHERE M.NAME = 'hello'
```
<strong>예제 2</strong>

이름으로 검색+정렬 하는 경우
```java
pulbic interface MemberRepository extends JpaRepository<Member, Long> {
    List<Member> findByName(String name, Sort sort); 
}
```
위 코드를 작성하면 실제 실행된 SQL은 
```sql
SELECT * FROM MEMBER M WHERE M.NAME = 'hello' ORDER BY AGE DESC
```

<strong>예제 3</strong>

이름으로 검색+정렬+페이징 하는 경우
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    Page<Member> findByName(String name, Pageable pageable);
}
```
Pageable을 파라미터로 추가하면 됩니다.

그리고 실제 사용할 때, 

```java
@RequestMapping("/search")
Page<Member> search(@RequestParam("name") String name, Pageable pageable) {
  PageRequest pageRequest = PageRequest.of(1, 10); 
  return repository.findByName(name, pageRequest);
}
```
위 처럼 페이지의 번호와 페이지 크기를 정해서 넣어주면 됩니다.

### @Query, JPQL 정의

@Query를 사용해서 직접 JPQL 지정가능 합니다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {
  
@Query("select m from Member m where m.name = ?1")
Member findByName(String name, Pageable pageable);
}
```

###  반환 타입 지정

반환 타입도 정할 수 있습니다.

```java
List<Member> findByName(String name); // 컬렉션
Member findByEmail(String email); // 단건
```
하나를 반환하면 타입을 그대로 명시, 컬렉션이면 List를 사용합니다.

### Web 페이징과 정렬 기능

* 컨트롤러에서 페이징 처리 객체를 바로 받을 수 있습니다.

	* page: 현재 페이지
	* size : 한 페이지에 노출할 데이터 건수
	* sort : 정렬 조건
```
/members?page=0&size=20&sort=name,desc
```
```java
@RequestMapping(vlaue = "/members", method = RequestMethod.GET)
String list(Pageable pageable, Model model) {...}
```
### Web 도메인 클래스 컨버터 기능

컨트롤러에서 식별자로 도메인 클래스 찾는 기능도 있습니다.
```
/members/100
```
```java
@RequestMapping("/members/{memberId}")
Member member(@PathVariable("memberId") Member member) {
    return member;
}
```
---
