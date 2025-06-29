---
layout: post
title: "spring batch core 배치 테이블이 자동 생성이 안된다면?"
tags: [Spring]
comments: true
---
 
해당 Post는 사이드 프로젝트를 진행하다가 meta 테이블이 없어서 발생한 문제를 팀원들에게 공유하다가 작성해보는 글입니다.

---

## 가정: 어떤 문제를 해결할까?

환경은 다음과 같이 가정합니다.

버젼 : **Spring Boot 3**, **Spring Batch 5**

yml은 다음과 같이 설정해 두었습니다.

```kotlin
  # 스프링 배치 설정
  batch:
    job:
      enabled: true
    jdbc:
      initialize-schema: always
```

위 처럼 설정한 것은 다음과 같은 이유가 있습니다.

1. initialize-schema의 값을 always로 설정한 것은 meta 테이블 정보를 실행 시점에 넣어주고 싶어서. (생성 이후에는 never를 쓰는 것을 권장합니다.)
2. 실행 시 job을 실행함으로써 배치 테이블이 제대로 들어갔는지 확인 합니다.

### 원인1. Spring Boot 3의 자동 구성 변경

Spring Boot 3에서는 Spring Batch의 자동 구성 방식이 변경되었습니다.

특히 @EnableBatchProcessing 어노테이션의 사용이 권장되지 않으며, 이를 추가하면 Spring Batch의 자동 구성이 비활성화될 수 있습니다. 

이로 인해 spring.batch.* 설정(예: batch.jdbc.initialize-schema)이 무시될 수 있습니다.

> @EnableBatchProcessing should not be used with DefaultBatchConfiguration. You should either use the declarative way of configuring Spring Batch through @EnableBatchProcessing, or use the programmatic way of extending DefaultBatchConfiguration, but not both ways at the same time.

참고 근거1 : <a href="https://github.com/spring-projects/spring-batch/issues/4252">spring batch issue 페이지</a>

참고 근거2 : <a href="https://docs.spring.io/spring-batch/docs/5.0.x/reference/html/job.html#configureJob">공식문서</a>

### 원인2. 내장 데이터베이스와 외부 데이터베이스 간 차이

Spring Boot는 기본적으로 내장 데이터베이스(H2, HSQLDB 등)를 사용할 때만 메타데이터 테이블을 자동으로 생성하도록 설정되어 있습니다. 

외부 데이터베이스(MySQL, PostgreSQL 등)를 사용하는 경우, 

spring.batch.jdbc.initialize-schema: ALWAYS를 설정했더라도 테이블이 생성되지 않을 수 있습니다.

참고 근거 : <a href="https://docs.spring.io/spring-boot/docs/3.0.x/reference/html/howto.html#howto.data-initialization">spring boot 3.0 공식문서(9. Database Initialization)</a>


## 2.해결 방안

위에 올려두었던, Spring boot 3.0의 공식문서에서 제공해주었던 방식을 사용한다.

링크에서 Initialize a Database Using Basic SQL Scripts 쪽을 인용 해보면 다음과 같다.

> In addition, Spring Boot processes the optional:classpath*:schema-${platform}.sql and optional:classpath*:data-${platform}.sql files (if present), where ${platform} is the value of spring.sql.init.platform. This allows you to switch to database-specific scripts if necessary. For example, you might choose to set it to the vendor name of the database (hsqldb, h2, oracle, mysql, postgresql, and so on).

classpath를 이용해 schema의 플랫폼에 따라 sql 문을 가져올 수 있다. (spring-batch 의존성이 제대로 적용되어 있다면 사용이 가능)

참고 링크 : <a href="https://github.com/spring-projects/spring-batch/tree/main/spring-batch-core/src/main/resources/org/springframework/batch/core">spring batch core에서 제공하는 SQL들</a>

![spring-batch-core-sql.png](../images/25%EB%85%84/6%EC%9B%94/spring-batch-core-sql.png)

검색 시 All로 설정했을 때 플랫폼 값을 정확히 입력해야 가져와져서, 

Files를 기준으로 검색하는 것을 나는 선호한다.

이번 프로젝트에서는 mysql을 기준으로 진행하고 있기 때문에, schema-mysql을 들어가보면 다음과 같다.

![schema-mysql.png](../images/25%EB%85%84/6%EC%9B%94/schema-mysql.png)

해당 내용들을 실행하면 정상적으로 Spring batch에 meta 테이블이 들어간 것을 확인할 수 있다.

---
