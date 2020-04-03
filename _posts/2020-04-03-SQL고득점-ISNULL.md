---
layout: post
title: "SQL고득점 - IS NULL"
tags: [SQL]
comments: true

---

위 문제는 프로그래머스 사이트의 IS NULL 문제에 관한 설명입니다.<br>

---

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다.

ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME,

SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 이름이 없는 동물의 아이디


동물 보호소에 들어온 동물 중, 이름이 없는 채로 들어온 동물의 ID를 조회하는 SQL 문을 작성해주세요.

단, ID는 오름차순 정렬되어야 합니다.

```SQL
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL 
ORDER BY ANIMAL_ID;
```

### 이름이 있는 동물의 아이디


동물 보호소에 들어온 동물 중, 이름이 있는 동물의 ID를 조회하는 SQL 문을 작성해주세요. 

단, ID는 오름차순 정렬되어야 합니다.

```SQL
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL 
ORDER BY ANIMAL_ID;
```

### NULL 처리하기

입양 게시판에 동물 정보를 게시하려 합니다.

동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요.

이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 No name으로 표시해 주세요.

```SQL
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name") as NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS;
```

<a href = "https://programmers.co.kr/learn/courses/30/parts/17045">프로그래머스 - IS NULL</a>

---
