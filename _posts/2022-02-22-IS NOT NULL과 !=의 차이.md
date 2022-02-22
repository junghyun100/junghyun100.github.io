---
layout: post
title: "NOT NULL과 !=의 차이"
tags: [개발 TIP, MSSQL]
comments: true

---

프로젝트를 가끔 혼동오는 것을 정리하기 위해 작성한다.

NOT NULL과 !=(또는 <>)와의 차이

---

## 상황 설명

![테이블 설명](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/57082af548cdaf2fc5b6bc27ac81afcdc70e9f5c/images/22%EB%85%84/0222/Table.png)

간단한 예를 들어 테이블은 위와 같이 구성한다.

`IsMembershipWithDrawn`은 NULL이 가능하도록 설정한다.

## 같지 않다?

<b>IS NOT</b> 과 <b>!=(또는 <>)</b> 이 것들의 의미는

<b>"같지 않다"</b> 라는 점이다.

그렇지만 결과적으로 나타나는 값은 다르다.

확인해보자.

![SQL문](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/57082af548cdaf2fc5b6bc27ac81afcdc70e9f5c/images/22%EB%85%84/0222/%ED%95%A8%EC%A0%95.png)

사용하고자 하는 SQL문에서 동일한 조건으로 실행해보았을 때

다음과 같은 결과값이 나타난다.

![Result](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/57082af548cdaf2fc5b6bc27ac81afcdc70e9f5c/images/22%EB%85%84/0222/Select.png)

    1. 맨위는 테이블 내에 있는 모든 데이터
    2. IS NOT NULL 조건 사용
    3. != NULL 조건 사용
    4. <> NULL 조건 사용

의미상으로는 "같지 않다"를 나타내지만 차이점은 다음과 같다.

<b>IS NOT NULL</b>은 검사 중인 개체/레코드가 

실제 `null 값인지 여부(데이터 없음)`를 결정한다다.

!= 와 <> 연산자는 원시 타입(Primitive type)에 대해서만 동작하고 

IS NOT NULL 은 모든 타입에 대해 동작한다.

### 참고링크 : 

<a href="https://stackoverflow.com/questions/5658457/not-equal-operator-on-null">스택오버플로우 관련링크</a> 

<a href="https://jhleed.tistory.com/156">jhleed 블로그</a>

---
