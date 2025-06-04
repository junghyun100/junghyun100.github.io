---
layout: post
title: "Spring JDBC"
tags: [Spring JDBC]
comments: true
---
 
해당 Post는 Spring JDBC에 대해서 정리한 파일입니다.

JDBC를 이용해서 프로그래밍을 하게 되면 반복적인 코드가 많이 발생합니다.

이러한 반복적인 코드는 개발자의 생산성을 떨어트리는 주된 원인이 됩니다.

이러한 문제를 해결하기 위해 등장한 것이 Spring JDBC입니다.

---

## Spring JDBC란?

* JDBC 프로그래밍을 보면 반복되는 개발 요소가 있습니다.
* 이러한 반복적인 요소는 개발자를 지루하게 만듭니다.
* 개발하기 지루한 JDBC의 모든 저수준 세부사항을 스프링 프레임워크가 처리해줍니다.
* 개발자는 필요한 부분만 개발하면 됩니다.

### Spring JDBC - 개발자가 해야 할 일은?

<img src ="https://cphinf.pstatic.net/mooc/20180205_176/1517797019977L6ygq_PNG/2_11_2_springJDBC.PNG">

## Spring JDBC 패키지

* org.springframework.jdbc.core

JdbcTemplate 및 관련 Helper 객체 제공
* org.springframework.jdbc.datasource

DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜젝션매니져 및 다양한 DataSource 구현을 제공
* org.springframework.jdbc.object

RDBMS 조회, 갱신, 저장등을 안전하고 재사용 가능한 객제 제공
* org.springframework.jdbc.support

jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크를 지원


## JDBC Template

* org.springframework.jdbc.core에서 가장 중요한 클래스입니다.
리소스 생성, 해지를 처리해서 연결을 닫는 것을 잊어 발생하는 문제 등을 피할 수 있도록 합니다.

스테이먼트(Statement)의 생성과 실행을 처리합니다.
* SQL 조회, 업데이트, 저장 프로시저 호출, ResultSet 반복호출 등을 실행합니다.

JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환시킵니다.
 
### JdbcTemplate select 예제1

1. 열의 수 구하기
```java
int rowCount = this.jdbcTemplate.queryForInt("select count(*) from t_actor");
```

### JdbcTemplate select 예제2

2. 변수 바인딩 사용하기
```java
int countOfActorsNamedJoe = this.jdbcTemplate.queryForInt("select count(*) from t_actor where first_name = ?", "Joe"); 
```

### JdbcTemplate select 예제3

3. String값으로 결과 받기
```java
String lastName = this.jdbcTemplate.queryForObject("select last_name from t_actor where id = ?", new Object[]{1212L}, String.class); 
```

### JdbcTemplate select 예제4

4. 한 건 조회하기
```java
Actor actor = this.jdbcTemplate.queryForObject(

  "select first_name, last_name from t_actor where id = ?",

  new Object[]{1212L},

  new RowMapper<Actor>() {

    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

      Actor actor = new Actor();

      actor.setFirstName(rs.getString("first_name"));

      actor.setLastName(rs.getString("last_name"));

      return actor;

    }

  });
```

### JdbcTemplate select 예제5

5. 여러 건 조회하기
```java
List<Actor> actors = this.jdbcTemplate.query(

  "select first_name, last_name from t_actor",

  new RowMapper<Actor>() {

    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

      Actor actor = new Actor();

      actor.setFirstName(rs.getString("first_name"));

      actor.setLastName(rs.getString("last_name"));

      return actor;

    }

  });
```

### JdbcTemplate select 예제6

6. 중복 코드 제거 (1건 구하기와 여러 건 구하기가 같은 코드에 있을 경우)
```java
public List<Actor> findAllActors() {

  return this.jdbcTemplate.query( "select first_name, last_name from t_actor", new ActorMapper());

}

private static final class ActorMapper implements RowMapper<Actor> {

  public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

    Actor actor = new Actor();

    actor.setFirstName(rs.getString("first_name"));

    actor.setLastName(rs.getString("last_name"));

    return actor;

  }

}
```

### JdbcTemplate insert 예제

7. INSERT 하기
```java
this.jdbcTemplate.update("insert into t_actor (first_name, last_name) values (?, ?)",  "Leonor", "Watling");
```

### JdbcTemplate update 예제

8. UPDATE 하기
```java
this.jdbcTemplate.update("update t_actor set = ? where id = ?",  "Banjo", 5276L);
```

### JdbcTemplate delete 예제

9. DELETE 하기
```java
this.jdbcTemplate.update("delete from actor where id = ?", Long.valueOf(actorId));
```

## JdbcTemplate외의 접근방법

### NamedParameterJdbcTemplate

JdbcTemplate에서 JDBC statement 인자를 ?를 사용하는 대신 파라미터명을 사용하여 작성하는 것을 지원

### SimpleJdbcTemplate

JdbcTemplate과 NamedParameterJdbcTemplate 합쳐 놓은 템플릿 클래스

이제 JdbcTemplate과 NamedParameterJdbcTemplate에 모든 기능을 제공하기 때문에 삭제 예정될 예정(deprecated)

### SimpleJdbcInsert

테이블에 쉽게 데이터 insert 기능을 제공

> 생각해보기

JDBC 프로그래밍이 불편해서 이를 해결하기 위해서 등장한 기술에는 Spring JDBC 외에도 다양한 기술들이 존재합니다. 

대표적으로 JPA와 MyBatis가 그러한 기술입니다. 문제를 해결하는 방법이 왜 여러 가지가 존재할끼요?

> Spring JPA와 Mybatis는 각각 장단점이 있습니다.

Spring JPA의 경우에는 객체 단위로 CUD가 가능합니다. 

일반적으로 CUD는 단일 테이블에 대하여 작업을 하는 경우가 많은데 Mybatis에서 하게 되면 해당 쿼리를 직접 짜고, 

들어오는 Parameter에 대하여 모두 확인하여 처리해야 하는 경우가 많습니다.

그러나 MyBatis의 장점을 무시할 순 없습니다. 

ER-DB를 사용하는 경우 자기가 원하는 Query를 짜서 직접 데이터를 손쉽게 전달하고 처리할 수 있다는 장점이 있죠. 

특히, Spring JPA에서 활용하는 상당히 복잡한 나름대로의 SQL을 배워야 하고, 

테이블 Join을 하게 될 경우 해당 조건을 객체구조에도 녹여내야 하는 등 배우고 익혀서 적용해야 할 포인트가 많아지게 됩니다.

<a href="http://blog.naver.com/PostView.nhn?blogId=admass&logNo=220870636605">참고</a>

---
