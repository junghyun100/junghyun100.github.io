---
layout: post
title: "Oracle과 MySQL의 차이점"
tags: [SQL]
comments: true
---
 
해당 Post는 Oracle과 MySQL의 차이점에 대해 정리한 파일입니다.

---

결론을 먼저 말하자면 Oracle과 MySQL은 서로 다른 시장을 구축하고 있기 때문에 "직접적인 비교는 옳지 않다"라고 할 수 있습니다.

오라클은 충분한 큰 예산과 복잡한 비즈니스 요구와 기업 고객을 위해 설계되었습니다.

그에 반해, MySQL은 가장 일반적으로 데이터베이스 기반 웹 사이트 및 Non-Critical 애플리케이션에 사용되는 저가의 데이터베이스입니다.


## Orcle 특징

오라클의 특징이자 장점은 크게 5가지로 구분하고 있습니다.

1. Oracle Management Server

 - 중앙 집중 방식으로 Administration monitoring이 가능하고, Multiple databases를 튜닝 가능합니다.

 - 다른 Admin User들과 공유가 가능합니다.

2. Oracle Change Manager

 - 변경 Plan을 작성하고 실제 구현하기 전에 변경 사항의 효과를 볼 수 있습니다.

 - 생산 시스템을 방해하지 않습니다.

3. Administrative Alerts

 - 오류가 발생하면 오라클은 이메일이나 설정되어 있는 계정으로 연락을 줄수 있습니다.

 - 경고는 예정된 가동 정지 시간 동안 차단 될 수 있습니다. 

4. Capacity Planning

 - 업그레이드 관리자의 계획을 돕기 위해 사용 패턴을 추적할 수 있습니다.

 - 병목 현상을 쉽게 파악 할 수 있습니다.

5. Query Optimizer

 - 쿼리 최적화 프로그램으로 오라클은 SQL문을 실행하는 가장 효율적인 방법을 선택합니다.

 - Cost 비용을 최소화하기 위해 테이블과 인덱스를 분석 합니다. (Oracle 10g 이상부터는 Cost_Base)

## MySQL 특징

1. 제일 큰 특징으로는 사용하기가 타 DBMS보다 쉽습니다.

2. PHPMyAdmin 같은 비용이 무료인 GUI 툴이 많습니다.

3. 매우 적은 오버헤드를 사용합니다.

 - MySQL은노트북에 단지 1Mb의 RAM만 사용합니다.

 - 오라클 9i를 설치하는 경우 128Mb를 사용합니다.

4. 고급기능을 지원하기 시작하였습니다.

 - Stored Procedures, Triggers, View, Sub-Queries, Transactional Table, Cascading Update & Delete

 - 타사 InnoDB Storage Engine을 사용할 수 있습니다.

##  예시를 통한 다른 점

### 첫째, 컬럼값에 NULL이면 다른값으로 표시해주는 함수사용법이 다릅니다. 

ORACLE에서는 NVL함수를 사용하지만 MYSQL에서는 IFNULL을 사용합니다.

> ex) (ORACLE) SELECT NVL(USER_ID,'') FROM KGON

>ex) (MYSQL ) SELECT IFNULL(USER_ID,'') FROM KGON



### 두번째로 현재날짜시간 확인하는 방법이 다릅니다.

ORACLE에서는 SYSDATE를 사용하지만 MYSQL에서는 NOW()함수를 사용합니다.

> ex) (ORACLE) SELECT SYSDATE FROM DUAL;

> ex) (MYSQL ) SELECT NOW() FROM DUAL;



### 세번째로 날짜포멧 변환방법이 다릅니다.

ORACLE에서는 날짜를 STRING으로 변경시 TO_CHAR()함수를 사용하지만 MYSQL에서는 DATE_FORMAT()함수를 사용합니다.

> ex) (ORACLE) SELECT TO_CHAR(REG_DATE, 'YYYYMMDD HH24MISS') FROM DUAL;

> ex) (MYSQL ) SELECT DATE_FORMAT(REG_DATE, '%Y%m%d%H%i%s') FROM DUAL;

[형식에 쓰는 영문자는 대소문자에 따라 다른값이 나올 수 있습니다.]

[%Y는 4자리년도(2017) , %y는 2자리년도(17)]



### 네번째로 요일변환의 숫자범위가 다릅니다.

ORACLE은 일,월,화,수,목,금,토를 1,2,3,4,5,6,7로 인식합니다. 

MYSQL은 일,월,화,수,목,금,토를 0,1,2,3,4,5,6으로 인식합니다. 

그렇기 때문에 요일 계산하는 경우 조심해야합니다. 

오늘이 수요일인경우 ORACLE에서는 4가 반환되고 MYSQL에서는 3이 반환되기 떄문에 요일변환 사용하는 부분을 잘 확인해야합니다.

[보통 JAVASCRIPT에서 일,월,화,수,목,금,토를 0,1,2,3,4,5,6으로 쓰기 때문에 ORACLE인경우 결과값을 -1해서 반환하는 경우가 많습니다.]

> ex) (ORACLE) SELECT TO_CHAR(SYSDATE, 'D') FROM DUAL; [결과값: 오늘이 수요일인경우 4를 반환]

> ex) (MYSQL ) SELECT DATE_FORMAT(NOW(), '%w') FROM DUAL; [결과값: 오늘이 수요일인경우 3을 반환]



### 다섯번째로 문자와 문자 합치는 방법이 다릅니다.

ORACLE에서는 문자와 문자를 합칠때 '||'을 사용합니다.

MYSQL에서는 문자와 문자를 합칠때 CONCAT()함수를 사용합니다.

> ex) (ORACLE) SELECT USER_ID FROM KGON WHERE USER_ID LIKE '%' || 'kgon' || '%'

> ex) (MYSQL ) SELECT USER_ID FROM KGON WHERE USER_ID LIKE CONCAT('%','kgon','%')



### 여섯번째로 형변환방법이 다릅니다.

ORACLE에서는 TO_CHAR, TO_NUMBER을 사용하여 형을 변환하지만 MYSQL에서는 CAST를 사용하여 형을 변환합니다.

> ex) (ORACLE) SELECT TO_CHAR(632) FROM DUAL

> ex) (MYSQL ) SELECT CAST(1234 AS CHAR) FROM DUAL



### 일곱번째로 페이징처리가 다릅니다. 

ORACLE은 ROWNUM을 이용하여 WHERE에서 BETWEEN으로 1~10번째자료를 나타냅니다.

MYSQL은 LIMIT를 사용하여 1~10번째자료를 나타냅니다.

> ex) (ORACLE) SELECT * FROM ( SELECT ROWNUM , A.* FROM (SELECT * FROM KGON) A )WHERE ROWNUM BETWEEN 0 AND 10

> ex) (MYSQL ) SELECT * FROM KGON LIMIT 0, 10



### 여덟번째로 시퀀스사용시 다음번호 불러오는 방법이 다릅니다.

ORACLE은 시퀀스명.NEXTVAL을 사용하지만 MYSQL은 시퀀스명.CURRVAL를 사용합니다.


참고 : https://ora-sysdba.tistory.com/entry/Oracle-인가-MySQL-인가 [Welcome To Ora-SYSDBA]

---
