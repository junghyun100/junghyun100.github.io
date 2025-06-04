---
layout: post
title: "네트워크 - MAC Protocols without Collision"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

MAC 프로토콜 중에서 <strong>collision이 발생하지 않는 기술</strong>중 하나인,

<strong>Channel Partitioning</strong>에 대해서 먼저 살펴 봅니다.

# Channel Partitioning : FDMA

<img src="/images/2021년/0118/Channel Partitioning_FDMA.PNG">

먼저 채널을 나누는 기술은 처음에 주파수로 나누는 기술을 <strong>FDMA</strong>라고 합니다.

> FDMA(Frequency Division Multiple Access) : 주파수를 나눈다는 뜻.

그림처럼 하나의 케이블에서 전송 될 수 있는 `주파수들을 여러 개로 분할`을 해서

개별 노드 하나하나 한테 주파수 채널을 나누어 줍니다.

각각 개별 노드에는 서로 다른 주파수를 할당 받기 채문에 충돌이 날 일이 없습니다.

충돌이 날 일이 없지만 문제는 2번, 5번과 6번 노드처럼 보낼 데이터가 없을 때는 

채널이 그냥 놀기 때문에 `자원이 낭비가 되는 문제`가 있게 됩니다.

그렇지만 <strong>가장 단순</strong>하고 가장 사람들이 가장 먼저 생각 해 낼 수 있는 방식입니다.

# Channel Partitioning : TDMA

<img src="/images/2021년/0118/Channel Partitioning_TDMA.PNG">

두 번째는 <strong>TDMA, time division multiple access</strong> 입니다.

이것은 전체 채널을 아까처럼 주파수 방식으로 나누는 것이 아니라

어떤 순간에는 `전체 bandwidth를 한 사용자가 다 사용`을 합니다.

여기서 `프레임`이라는 것으로 나누고 이 프레임을 `타임 슬롯`으로 나눕니다.

예를 들어서 만약에 `여섯 개의 슬롯`으로 나눴다고 하면, 

전체 하나의 프레임을 똑같은 크기의 `여섯 개의 타임 슬롯`으로 나누고

각각의 타임 슬롯을 특정한 사용자한테 할당하는 방식입니다.

이 경우에도 `FDMA 방식`과 마찬가지로 어떤 노드가 데이터를 보낼게 없다면

그 `타임 슬롯을 낭비하게 되는 단점`이 있습니다.

그런데 `FDMA`보다 `TDMA`가 더 나은 점은 

`FDMA`에서는 주파수로 나눌 때,

할당 되는 주파수 외에도 중간에 `guard band`로 남는 주파수들이 있습니다.

`guard band`로 낭비 되는 그런 것들이 있어서 효율성이 더 낮은데

타임 슬롯으로 나눈 `TDMA`는 

이런 `guard band`가 없기 때문에 보다 효율성 측면에서는 `FDMA`보다는 낫지만

그래도 어떤 특정 노드가 보낼 데이터가 없을 경우에 그 슬롯은 놀기 때문에

`MAC 프로토콜`에 대한 희망사항 첫 번째 전송 하려는 노드가 하나 뿐일 때는 

<strong>전체 `bandwidth`를 다 쓰면 좋겠다</strong>라는 규칙을 지키지 못하게 됩니다.

# Channel Partitioning : CDMA

<img src="/images/2021년/0118/Channel Partitioning_CDMA.PNG">

그 다음 나온 기술은 <strong>CDMA, code division multiple access</strong> 입니다.

앞의 `FDMA`와 `TDMA`와 비슷한 형태로 명칭을 짓다 보니까 `CDMA`로 작명되었으나,

코드를 가지고 뭘 나누는게 아니라 <strong>암호화 하는 개념</strong>입니다.

과거에 휴대폰에서 `CDMA`를 많이 사용했습니다.

휴대폰 사용자들은 기지국으로부터 코드를 할당 받아서 자기가 보낼 데이터를 

이 코드로 인코딩을 해서 보내게 됩니다.

그러면 시간과 주파수를 동시에 다 같이 쓸 수 있는데

각각의 폰에서 나온 데이터들에는 개별 코드로

`코드 1`, `코드 2`, `코드 N` 처럼 적용이 되어서 전송이 됩니다.

전송이 되면 그것이 각각의 코드 별로 `채널 1`, `2`, `채널 N` 인 것 처럼 동작합니다.

그러면 `receiver(기지국)` 측에서도 `코드 1`에 대해서 알아야만 

이 `코드 1`을 사용하는 휴대폰이 보내는 데이터를 해석 할 수 있습니다.

그래서 이렇게 코드를 적용해서 `인코딩`과 `디코딩` 방식으로 

서로 데이터를 구별하는 것. 

이것을 `CDMA`라고 얘기를 합니다.

그래서 전체적으로 효율은 앞의 `FDMA`나 `TDMA`보다 훨씬 좋습니다.

---
