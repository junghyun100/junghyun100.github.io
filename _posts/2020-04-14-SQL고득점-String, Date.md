---
layout: post
title: "SQL고득점 - String, Date"
tags: [SQL]
comments: true

---

위 문제는 프로그래머스 사이트의 String, Date 문제에 관한 설명입니다.<br>

---


### 문제

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다.

ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME,

SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다.

ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 

각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다.

ANIMAL_OUTS 테이블의 ANIMAL_ID는 ANIMAL_INS의 ANIMAL_ID의 외래 키입니다.

### 루시와 엘라 찾기

동물 보호소에 들어온 동물 중 이름이 Lucy, Ella, Pickle, Rogan, Sabrina, Mitty인

동물의 아이디와 이름, 성별을 조회하는 SQL 문을 작성해주세요.

```SQL
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE //동물의 아이디와 이름 성별을 선택
FROM ANIMAL_INS//동물 보호소에 들어온 동물 중
WHERE NAME IN ('Lucy','Ella','Pickle','Rogan','Sabrina','Mitty') //조건 ( 이름 안에 ~~가 포함된)
ORDER BY ANIMAL_ID;//ID순으로 정렬
```

### 이름에 el이 들어가는 동물 찾기

보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다.

이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다.

동물 보호소에 들어온 동물 이름 중, 이름에 EL이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요.

이때 결과는 이름 순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME //동물의 ID와 이름을 선택
FROM ANIMAL_INS//동물 보호소에 들어온 동물 중
WHERE NAME LIKE '%EL%' AND ANIMAL_TYPE = "Dog" //조건 (종이 강아지이면서, 이름에 %EL%이 들어간 동물)
ORDER BY NAME;//이름순으로 정렬
```

### 중성화 여부 파악하기

보호소의 동물이 중성화되었는지 아닌지 파악하려 합니다.

중성화된 동물은 SEX_UPON_INTAKE 컬럼에 'Neutered' 또는 'Spayed'라는 단어가 들어있습니다.

동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요.

이때 중성화가 되어있다면 'O', 아니라면 'X'라고 표시해주세요.

```SQL
SELECT ANIMAL_ID, NAME, //동물의 ID와 이름을 선택 
CASE //CASE 선정
WHEN SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%' // 동물의 성별에 Neutered 또는 Spayed가 들어있다면?
THEN 'O'//조건을 만족하면 O를 출력
ELSE 'X'//아니라면 X 출력
END AS '중성화'//중성화로 출력
FROM ANIMAL_INS//동물 보호소에 있는 자료들 중
ORDER BY ANIMAL_ID;//ID순으로 정렬
```

### 오랜 기간 보호한 동물(2)

입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요.

이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.

```SQL
SELECT A.ANIMAL_ID, A.NAME // 동물의 ID와 이름을 선택
FROM ANIMAL_INS A, ANIMAL_OUTS B // A(동물보호소에 들어온 동물들) B(동물보호소를 나간 동물들)로부터
WHERE A.ANIMAL_ID = B.ANIMAL_ID//조건(A와 B중 ID가 같은동물) 
ORDER BY B.DATETIME-A.DATETIME DESC// 동물보호소에 있었던 기간(B의 시간- A의 시간)순으로 정렬
LIMIT 2//2마리만 출력
```

### DATETIME에서 DATE로 형 변환
ANIMAL_INS 테이블에 등록된 모든 레코드에 대해, 각 동물의 아이디와 이름,

들어온 날짜1를 조회하는 SQL문을 작성해주세요.

이때 결과는 아이디 순으로 조회해야 합니다.

```SQL
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') as '날짜' //동물의ID와 이름 날짜(년도,월,일)을 선택
FROM ANIMAL_INS // 동물보호소에 들어온 자료들로부터
ORDER BY ANIMAL_ID // ID순으로 정렬
```

<a href = "https://programmers.co.kr/learn/courses/30/parts/17047">프로그래머스 - String, Date</a>

---
