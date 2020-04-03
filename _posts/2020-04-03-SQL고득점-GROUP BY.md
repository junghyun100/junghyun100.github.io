---
layout: post
title: "SQL고득점 - GROUP BY"
tags: [SQL]
comments: true

---

위 문제는 프로그래머스 사이트의 GROUP BY 문제에 관한 설명입니다.<br>

---

ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다.

ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME,

SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 고양이와 개는 몇 마리 있을까

동물 보호소에 들어온 동물 중 고양이와 개가 각각 몇 마리인지 조회하는 SQL문을 작성해주세요.

이때 고양이가 개보다 먼저 조회해주세요.

```SQL
SELECT ANIMAL_TYPE ,COUNT (ANIMAL_TYPE)
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE;

```

### 동명 동물 수 찾기

동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요.

이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.

```SQL
SELECT NAME, COUNT(NAME)
FROM ANIMAL_INS 
GROUP BY NAME 
HAVING COUNT(NAME) >= 2
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

### 입양 시각 구하기(1)

ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다.

ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 

각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다.


보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다.

9시부터 19시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요.

이때 결과는 시간대 순으로 정렬해야 합니다.

```SQL
SELECT HOUR(DATETIME), COUNT(DATETIME)
FROM ANIMAL_OUTS 
WHERE HOUR(DATETIME) >= 9 
AND HOUR(DATETIME) <= 19 
GROUP BY HOUR(DATETIME);
```

### 입양 시각 구하기(2)

보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다.

0시부터 23시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요.

이때 결과는 시간대 순으로 정렬해야 합니다.

#### 처음 풀었을 때

```SQL
SELECT HOUR(DATETIME), COUNT(DATETIME)
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME)<=23
GROUP BY HOUR(DATETIME);
```
이렇게 풀어낸다면 count가 없는 출력이 나오지 않았습니다.

다른 사람들 코드를 봤을 때 훨씬 깔끔하게 푼 내용이 있어서 들고 왔습니다.

```SQL
SET @hour = -1;
SELECT
    (@hour := @hour +1) as HOUR,
    (SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(`datetime`) = @hour) AS `COUNT`
FROM
  ANIMAL_OUTS 
WHERE
  @hour < 23
```
출처 : rudtyz님이 올려주신 코드

> SET @변수명 = '값';<br>
SET @변수명 := '값'; <br>
SET 명령어를 사용한 변수 선언 시 '=' 와 ':=' 2가지 방법은 차이가 없습니다.<br>
'=' : MySQL에서 대입연산자, 비교연산자 2가지로 사용 됨 (SET 명령어에서만 대입 연산자로 인식)<br>
':=' : MySQL에서 대입 연산자로만 사용 됨 <br>
변수 사용 시 명시적으로 대입 연산자의 의미만을 갖는 ':=' 의 사용을 권장합니다. <br>

출처: https://mentha2.tistory.com/98 [행궁동 데이터과학자]

<a href = "https://programmers.co.kr/learn/courses/30/parts/17044">프로그래머스 - GROUP BY</a>

---
