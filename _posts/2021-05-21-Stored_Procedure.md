---
layout: post
title: "Stored Procedure"
tags: [MsSQL, Database]
comments: true
---

해당 포스팅은 Stored Procedure 관련 내용입니다.

---

# Stored Procedure란?

항상 단일 SQL문장으로 모든 쿼리나 데이타 갱신을 수행하면 좋겠지만, 

여러 SQL문장을 수행하여 복잡한 로직을 구성해야하는 경우가 있습니다. 

여러 SQL문장들을 SQL서버에서 하나의 함수처럼 저장해서 

편리하게 호출할 수 있게 만든 것이 저장 프로시져(Stored Proceudre 혹은 간단히 SP라 부른다)입니다. 

Stored Procedure는 그 안에 복잡한 비지니스 로직을 넣을 수도 있고, 

복잡한 데이타 처리를 수행할 수 있습니다.

### 참고로, MSSQL은 SP를 사용하는 것을 정책으로 삼고 있습니다.

## 저장 프로시저의 장점

저장 프로시저를 사용하게되면 여러 가지 장점들이 있습니다.
   
1. 하나의 요청으로 여러 SQL문을 실행 할 수 있습니다.
2. 네트워크 소요 시간을 줄일 수 있습니다.
    * 동일한 쿼리를 여러번 호출 하는 것보다 SP를 호출할 때 한 번만 네트워크를 경유하기 때문에 네트워크 소요시간을 줄이고 성능을 개선할 수 있습니다.
3. 개발 업무를 구분해 개발 할 수 있습니다.
    * 순수한 애플리케이션만 개발하는 조직과 DBMS 관련 코드를 개발하는 조직이 따로 있다면, DBMS 개발하는 조직에서는 SP를 만들어 API처럼 제공하고 애플리케이션 개발자는 SP를 호출해서 사용하는 형식으로 역할을 구분하여 개발이 가능합니다.
    
   
## 저장 프로시저의 단점

저장 프로시저의 장점에 대해 알아보았다면 이제 단점에 대해서도 알아볼 필요가 있습니다.
   
1. 처리 성능이 낮습니다.
    * 문자나 숫자 연산에 저장 프로시저를 사용한다면, C나 JAVA보다 느린 성능을 보여줍니다.
2. 디버깅이 어렵습니다.
3. DB 확장이 매우 힘듭니다.
   * 서비스 사용자가 많아져 서버수를 늘려야할 때, DB 수를 늘리는 것이 더 어렵습니다.
   * 서비스 확장을 위해 서버수를 늘릴경우 DB 수를 늘리는 것보다 WAS의 수를 늘리는 것이 더 효율적이기 때문에 대부분의 개발에서 DB에는 최소의 부담만 주고 대부분의 로직은 WAS에서 처리할 수 있게 합니다.

## Stored Procedure 문법1
   
Stored Procedure를 만들기 위해서는 CREATE PROCEDURE (혹은 줄여서 CREATE PROC) 문을 사용합니다. 

기본적으로 CREATE PROC 뒤에 프로시져명을 써주고, AS 뒤에 저장할 SQL문장들을 적어줍니다. 

다음은 간단하게 SELECT 한 문장으로 된 SimpleSP이라는 예시 입니다.

```SQL   
CREATE PROCEDURE SimpleSP
AS
SELECT Id,Score FROM [Scores]
```

## Stored Procedure 문법2
   
Stored Procedure는 저장 프로시져 안에서 사용할 입력 파라미터를 받아들일 수 있고, 

또한 출력 파라미터도 사용할 수 있습니다. 

파라미터는 프로시져명과 AS 사이에 @로 시작되는 파라미터명과 파라미터 타입을 적습니다. 

출력 파라미터 인 경우는 OUTPUT 키워드를 뒤에 사용합니다. 

출력 파라미터가 있는 경우, 프로시져 내에서 출력 파라미터에 값을 할당해 준다.

```sql
CREATE PROCEDURE sp_ClassAvg
   @class int,
   @minScore int,
   @average int output
AS
 DECLARE @sum int
 DECLARE @cnt int
 
 SELECT @sum=SUM(Score),@cnt=COUNT(Score)
 FROM [Scores]
 WHERE Score >= @minScore

 SET @average = @sum/@cnt

 INSERT INTO RunData
 VALUES (@class,@minScore,@avergage)
GO

```

## Stored Procedure 호출

Stored Procedure 실행하기 위해서는 EXECUTE (줄여서 EXEC)문이나 sp_executesql을 사용합니다. 

Stored Procedure가 만약 문장의 처음에 나올 경우 

EXEC 혹은 sp_executesql를 생략하고 SP명만으로 직접 실행할 수 있습니다. 

파라미터가 있는 경우는 파라미터를 순서대로 적어주거나 

Named Parameter로 [파리미터명=파라미터값]의 형식으로 적어줍니다.

```sql
-- SimpleSP 호출
EXECUTE SimpleSP

-- sp_ClassAvg 호출
-- Ordered Parameters
DECLARE @avg int
EXEC sp_ClassAvg 10, 50, @avg output
SELECT @avg
GO

-- sp_ClassAvg 호출
-- Named Parameters
DECLARE @avg int
EXEC sp_ClassAvg @minScore=50, @class=10, @average=@avg output
SELECT @avg
GO
```
 
 
---
