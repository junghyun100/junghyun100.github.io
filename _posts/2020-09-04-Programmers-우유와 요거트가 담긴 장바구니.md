---
layout: post
title: "Programmers-우유와 요거트가 담긴 장바구니"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 우유와 요거트가 담긴 장바구니 문제에 관한 설명입니다.<br>

Summer/Winter Coding(2019)에 나왔던 문제입니다.

---

## 문제

`CART_PRODUCTS` 테이블은 장바구니에 담긴 상품 정보를 담은 테이블입니다.

`CART_PRODUCTS` 테이블의 구조는 다음과 같으며, 

`ID`, `CART_ID`, `NAME`, `PRICE`는 각각 테이블의 아이디, 장바구니의 아이디, 상품 종류, 가격을 나타냅니다.


|NAME       |   TYPE | 
| :-------: | :----: | 
| "ID"      |   INT  | 
| "CART_ID" |   INT  | 
| "NAME"    |VARCHAR |
| "PRICE    |   INT  |

데이터 분석 팀에서는 우유(Milk)와 요거트(Yogurt)를 동시에 구입한 장바구니가 있는지 알아보려 합니다. 

우유와 요거트를 동시에 구입한 장바구니의 아이디를 조회하는 SQL 문을 작성해주세요.

이때 결과는 장바구니의 아이디 순으로 나와야 합니다.

### 해결 방법

SQL을 직접 실행하면서 해보는게 좋을 것 같아  테이블에 값들을 직접 넣어봤습니다.

```SQL
CREATE SCHEMA CART;

USE CART;

CREATE TABLE CART_PRODUCTS
	(
        ID INT(10),
		CART_ID INT(10),
		NAME VARCHAR(30),
		PRICE INT(10)
	);
  
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (1630,83,'Cereal',3980);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (1631,83,'Multipurpose Supply',3900);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (5491,286,'Yogurt',2980);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (5504,286,'Milk',1880);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (8435,448,'Milk',1880);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (8437,448,'Yogurt',2980);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (8438,448,'Tea',11000);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (20236,1034,'Yogurt',2980);
INSERT INTO CART_PRODUCTS  (ID, CART_ID, NAME,PRICE)
VALUES (20237,1034,'Butter',4890);
``` 입력 끝.

--- SELECT * FROM CART_PRODUCTS; (출력부분)
```

이 문제에서 사용된 방법은 이너 조인입니다.

### INNDER JOIN

<img src = "https://t1.daumcdn.net/cfile/tistory/99219C345BE91A7E32" >

`A, B 테이블` 중에

`A`의 `KEY` 값과 `B`의 `KEY` 값이 같은 결과를 리턴


```SQL
SELECT DISTINCT A.CART_ID
FROM (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Yogurt') A
INNER JOIN (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Milk') B
ON A.CART_ID = B.CART_ID
```

`SELECT` (원하는 쿼리)

`SELECT 'DISTINCT' CART_ID`를 사용한 것은 중복된 값들이 출력되기 때문입니다.

예를 들어 `DISTINCT`가 없다고 생각해봅시다.

|CART_ID|
| :-----:|
|286|
|448|
|448|
|578|
|578|
|977|
|1048|
|1048|

이렇게 중복되는 값들이 SQL문안에 들어갈 수 있습니다. 

FROM (테이블1)

INNER JOIN (테이블2)

테이블 1과 2에 원하는 쿼리문을 넣어주고, 

ON (KEY값)

두개의 테이블에 KEY값이 모두 존재할 때만 

출력하는 INNER JOIN을 활용하여 쿼리문을 작성합니다.

```SQL
FROM (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Yogurt') A
INNER JOIN (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Milk') B
```
테이블 `A`에서 요거트의 이름이 들어간 상품들의 `CART_ID`를 출력하고,

INNER JOIN 을 시켜서 테이블 `B`에서 밀크의 이름이 들어간 상품들의 `CART_ID`를 출력합니다.

```SQL
ON A.CART_ID = B.CART_ID
```
KEY 값은 `A`테이블과 `B`테이블에서 `CART_ID`가 서로 같은 곳.

즉 두가지 상품을 모두 포함하는 `CART_ID`를 출력합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/62284">우유와 요거트가 담긴 장바구니</a>

---
