---
layout: post
title: "Isolation level"
tags: [DB, 면접]
comments: true

---

# Isolation level이란 무엇인가?

트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준

ANSI에서 작성된 SQL-92 표준은 4가지 Transaction Isolation Level 정의

* Read Uncommitted

* Read Committed

* Repeatable Read

*  Serializable

Isolation level 조정은 동시성이 증가되는데 반해 데이터 무결성에 문제가 발생할 수 있고,

데이터의 무결성을 유지하는 데 반해 동시성이 떨어질 수 있다.

---

## Isolation Level 의 필요성

데이터베이스는 ACID 같이 트랜잭션이 원자적이면서도 독립적인 수행을 하도록 한다.

그래서 Locking 이라는 개념이 등장한다.

트랜잭션이 DB를 다루는 동안 다른 트랜잭션이 관여하지 못하게 막는 것

하지만 무조건적인 Locking으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식으로 구현되면 DB의 성능은 떨어지게 된다.

반대로 응답성을 높이기 위해 Locking 범위를 줄인다면 잘못된 값이 처리 될 여지가 있다.

---
### A.    Read Uncommited Isolation Level (레벨 0)

- SELECT 문장을 수행하는 경우 해당 데이터에 shared lock이 걸리지 않는 레벨.

- 트랜잭션에 처리중인 혹은 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용

- 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 <br>

다른 사용자는 B라는 아직 완료되지 않은(Uncommitted 혹은 Dirty) 데이터 B를 읽을 수 있다. 
<br>(Commit이 이루어진 트랜잭션만 조회 가능)

- Dirty read, Non-repeatable read, Phantom read 현상 발생

---

### B.     Read Committed Isolation Level (레벨 1)

- SELECT 문장이 수행되는 동안 해당 데이터에 shared lock이 걸리는 레벨.

- dirty read 방지 : 트랜잭션이 커밋되어 확정된 데이터만을 읽는 것을 허용

- 어떠한 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 해당 데이터에 접근할 수 없다.

- DB2, SQL Server, Sybase의 경우 읽기 공유 lock을 이용해서 구현,

하나의 레코드를 읽을 때 lock을 설정하고 해당 레코드에서 빠지는 순간 lock해제

- Non-repeatable read, Phantom read 현상 발생
---
### C.     Repeatable Read Isolation Level (레벨 2)

- 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 

다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능하다.

- 선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 후행 트랜잭션이 갱신하거나 삭제하는 것을 

불허함으로써 같은 데이터를 두 번 쿼리했을 때 일관성 있는 결과를 리턴함

- DB2, SQL Server의 경우 읽은 데이터에 걸린 공유 lock을 커밋할 때까지 유지하는 방식으로 구현

- Oracle은 이 레벨을 명시적으로 지원하지 않지만 for update절을 이용해 구현 가능

- SELECT col FROM a WHERE col1 BETWEEN 1 AND 10 을 수행하였고 결과는 두 건의 데이터 출력(col1 = 1 ,5). 

다른 사용자가 col1이 1이나 5인 row에 대한 update는 불가능하다. 이를 제외한 나머지 범위에 해당하는 row를 insert하는 것은 가능.

- Phantom read 현상 발생
---
### D.    Serializable Isolation Level (레벨 3)

- SELECT 수행 시 READ LOCK, 즉 shared lock을 사용

- 완벽한 읽기 일관성 모드를 제공

- 데이터의 일관성 및 동시성을 위해 MVCC(Multi Version Concurrency Control)을 사용하지 않음

(MVCC는 다중 사용자 데이터베이스 성능을 위한 기술로 데이터 조회 시 LOCK을 사용하지 않고 

데이터의 버전을 관리해 데이터의 일관성 및 동시성을 높이는 기술)

- 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는

그 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능하다.

 ---

###  낮은 단계의 트랜잭션 고립화 수준 이용시 발생하는 현상 3가지

- Dirty Read (=Uncommitted Dependency)

커밋되지 않은 수정 중인 데이터를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생

어떤 트랜잭션에서 아직 실행이 끝난지 않은 다른 트랜잭션에 의한 변경 사항을 보게 되는 되는 경우

- Non-Repeatable Read (=Inconsistent Analysis)

한 트랜잭션 내에서 같은 쿼리를 두 번 수행할 때 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제함으로써

두 쿼리의 결과가 상이하게 나타나는 비 일관성 발생

- Phantom Read

한 트랜잭션 안에서 일정 범위의 레코드를 두 번 이상 읽을 때, 첫 번째 쿼리에서 없던 레코드가 두 번째 쿼리에서 나타나는 현상.

이는 트랜잭션 도중 새로운 레코드가 삽입되는 것을 허용하기 때문에 나타남.
 
```
