---
layout: post
title: "Spring Data JPA(2)"
tags: [JPA]
comments: true

---

JPA를 공부하면서 정리를 한 내용들 입니다.

-참고 김영한 개발자님 : [토크ON세미나] JPA 프로그래밍 기본기 다지기 - Spring Data JPA(2) | T아카데미

---

# 8강. Spring Data JPA

## QueryDSL

* SQL과 JPQL을 코드로 작성할 수 있도록 도와주는 빌더 API
* JPA criteria에 비해서 편리하고 실용적이다.
* 오픈소스

## SQL, JPQL의 문제점
* SQL, JPQL은 문자, Type-check 불가능
* 해당 로직 실행 전까지 작동여부 확인 불가(컴파일 시점에 알 수 없다.)

## QueryDSL 장점
* 문자가 아닌 코드로 작성
* 컴파일 시점에 문법 오류 발견
* 코드 자동완성(IDE 도움)
* 단순하고 쉬움: 코드 모양이 JPQL과 거의 비슷
* 동적 쿼리

## QueryDSL 사용
* JPQL
	* select m from Member m where m.age > 18
* QueryDSL
```java
  JPAFactoryQuery query = new JPAQueryFactory(em);
  QMember m = QMember.member;
  
  List<Member> list = query.selectFrom(m)
                           .where(m.age.gt(18))
                           .orderBy(m.name.desc())
                           .fetch();
```

## QueryDSL - 조인
```java
JPAQueryFactory query = new JPAQueryFactory(em);
QMember m = QMember.member;
QTeam t = QTeam.team;

List<Member> list = query.selectFrom(m)
                         .join(m.team, t)
                         .where(t.name.eq("teamA"))
                         .fetch();
```

## QueryDSL - 페이징 API

```java
JPAQueryFactory query = new JPAQueryFactory(em);
QMember m = QMember.member;

List<Member> list = query.selectFrom(m)
                         .orderBy(m.age.desc())
                         .offset(10)
                         .limit(20)
                         .fetch();
```

## QueryDSL - 동적 쿼리

QueryDSL을 쓰는 가장 큰 이유는 동적 쿼리 때문

특히 DTO로 바로 조회해야 하는 경우 QueryDSL이 특히 유용하다.

```java
String name = "memebr";
int age = 9;

QMember m = QMember.member;

BooleanBuildere builder = new BooleanBuilder();
if (name != null) {
  builder.and(m.name.contains(name));
}
if (age != 0) {
  builder.and(m.age.gt(age);
}

List<Member> list = query.selectFrom(m)
                         .where(builder)
                         .fetch();
```
## QueryDSL - 이것은 자바다!
```java
return query.selectFrom(coupon)
            .where(
                coupon.type.eq(typeParam),
                coupon.status.eq("LIVE"), //서비스 필수 제약조건
                marketing.viewCount.lt(marketing.maxCount) //서비스 필수 제약조건
             )
             .fetch();
```
위와 같은 상황에서 다음과 같이 제약조건을 조립할 수 있다. 
- (가독성과 재사용성)

```java
return query.selectFrom(coupon)
            .where(
                coupon.type.eq(typeParam),
                isServiceable()
            )
            .fetch();

private BooleanExpression isServiceable() {
    return coupon.status.eq("LIST") //서비스 필수 제약조건
           .and(marketing.viewCount.lt(marketing.maxCount)); //서비스 필수 제약조건
}
```

---
