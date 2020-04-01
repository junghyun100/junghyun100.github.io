---
layout: post
title: "SQL고득점 - SELECT"
tags: [SQL]
comments: true

---

위 문제는 프로그래머스 사이트의 SELECT 문제에 관한 설명입니다.<br>

---

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다.

ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME,

SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 모든 레코드 조회하기


동물 보호소에 들어온 모든 동물의 정보를 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요.

```SQL
SELECT ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE 
from ANIMAL_INS

```

또는 

```SQL
SELECT * 
from ANIMAL_INS
```

### 역순 정렬하기

동물 보호소에 들어온 모든 동물의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요.

이때 결과는 ANIMAL_ID 역순으로 보여주세요.

```SQL
SELECT NAME, DATETIME 
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID DESC;
```

### 아픈 동물 찾기

동물 보호소에 들어온 동물 중 아픈 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요.

이때 결과는 아이디 순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME 
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION = "Sick" 
ORDER BY ANIMAL_ID;
```

### 어린 동물 찾기

동물 보호소에 들어온 동물 중 젊은 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요.

이때 결과는 아이디 순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME 
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION != "AGED" 
ORDER BY ANIMAL_ID;
```

### 동물의 아이디와 이름

동물 보호소에 들어온 모든 동물의 아이디와 이름을 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요.

```SQL
SELECT ANIMAL_ID, NAME 
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID;
```

### 여러 기준으로 정렬하기

동물 보호소에 들어온 모든 동물의 아이디와 이름, 보호 시작일을 이름 순으로 조회하는 SQL문을 작성해주세요.

단, 이름이 같은 동물 중에서는 보호를 나중에 시작한 동물을 먼저 보여줘야 합니다.

```SQL
SELECT ANIMAL_ID, NAME,	DATETIME 
FROM ANIMAL_INS 
ORDER BY NAME ASC, DATETIME DESC;
```

### 상위 n개 레코드

동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT NAME  
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1;
```
<a href = "https://programmers.co.kr/learn/courses/30/parts/17042">프로그래머스 - SELECT</a>

---
