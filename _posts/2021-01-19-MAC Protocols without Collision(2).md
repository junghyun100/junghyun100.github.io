---
layout: post
title: "네트워크 - MAC Protocols without Collision(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

`FDMA`, `TDMA`, 그리고 `CDMA` 이렇게 채널을 나누는 방식이 있었습니다.

그 외에도 노드별로 데이터를 전송 할 수 있는 <strong>순서를 정해 주는 방식</strong>을 알아봅니다.

# Taking Turns : Polling

<img src="/images/2021년/0119/Taking Turns_Polling.PNG">

블루투스에서 이런 방식을 사용합니다.

마스터 노드가 있고 여러 개의 슬레이브 노드들이 있으면

`어떤 순서`대로 보내라 하고 알려 주는 겁니다.

```

첫 번째 호스트가 데이터 전송이 끝나고 나면 

마스터가 두 번째 호스트 측에 데이터를 보낼게 있는지 확인합니다.

없다면 다음단계로 넘어가서 세번째 노드 측에 확인합니다.

이와 같이 순서대로 돌아가면서 확인합니다. 

```

이렇게 전송 기회를 받은 호스트만 전송을 하기 때문에 

`collision`은 발생 하지 않겠지만 일단 `polling`를 함으로써 <strong>오버헤드가 발생</strong>합니다.

> polling : "물어보면서, 확인을 하면서"로 해석

그래서 좀 `지연 시간`이 발생 할 수 있고, 마스터 노드에 문제가 생기면

모두가 통신을 못 하게 되는 <strong>single point of failure</strong>가 발생 할 수 있습니다.

# Taking Turns : Token Passing

<img src="/images/2021년/0119/Taking Turns_Token Passing.PNG">

<strong>Token Ring</strong>은 `Token Passing`이라는 기술을 쓰게 됩니다.

`Polling`에서처럼 하나의 마스터가 아닌 `token`을 소유한 호스트만 전송을 하는 방식입니다.

그림을 보고 예시를 들어봅시다.

12시 방향 호스트가 `token`을 가지고 있다가, 두 번째 호스트에게 넘겨 줍니다. 

두 번째 호스트 측에서는 보낼 데이터가 없다면,

바로 다음 호스트측에 `token`을 전달해 줍니다.

만약 세 번째 노드에는 보낼 데이터가 있다면, 

`token`을 가진 상태에서 데이터를 전송하고나서 다음 노드에게 넘겨주게 됩니다.

이 과정에서 `token`을 관리하는 <strong>오버헤드</strong>가 있고, 

`token`이 보낼 데이터가 없는 호스트들 한테도 거쳐서 와야 되기 때문에 

<strong>latency가 존재</strong> 할 수 있습니다.

`token`을 가지고 있는 호스트가 갑자기 고장난다면 `token`이 전달 되지 않기 때문에

망 전체가 마비 되는 <strong>single point of failure</strong> 문제가 발생 할 수 있습니다.

그래서 `token`을 사용하는 `token ring` 형태의 많이 사용 되는 망이 

<strong>FDDI(Fiber Distributed Dual Interface)</strong>로 

일반적인 `token ring`에 비해서 좀 더 견고한, 안정성을 위해서 

`ring`을 서로 다른 방향으로 도는 두 개의 `ring`을 두었습니다.

# ARP : Routing to Another LAN

<img src="/images/2021년/0119/ARP_Routing to Another LAN.PNG">

이번에는 서로 다른 랜에서 어떻게 <strong>ARP</strong>가 동작하는지 살펴 보겠습니다.

`A`가 `source`고 `B`가 `destination`일 때,

`A`는 자신의 `source` 주소와 데이터그램의 목적지 주소를 살펴 봅니다.

그러면 `source`는 `111.111.111.111`인데 `destination`은 `222.222.222.222`이므로

네트워크 ID가 다르기 때문에 같은 네트워크가 아니라 판단 할 수 있습니다.

그러면 `A`는 같은 네트워크가 아니기 때문에 `ARP` 테이블을 살펴 봐도 소용이 없고 

그래서 중간에 있는 라우터 `R`의 `MAC` 주소를 찾아서 해당 주소를 담아서 전달합니다.

라우터는 왼쪽 랜과 오른쪽 네트워크에 다 포함이 되어 있기 때문에,

인터페이스 카드를 갖고 주소도 두 개씩 갖게 됩니다.

<img src="ARP_Routing to Another LAN2">

`A`라는 컴퓨터는 `R` 라우터의 `IP` 주소와 `MAC` 주소를 알고 있고

`B` 컴퓨터는 라우터가 `IP` 주소와 `MAC` 주소를 갖고 있습니다.

`A`의 입장에서 `B`는 외부 호스트이기 때문에 외부 호스트의 `MAC` 주소는 알 수 없고

이 라우터로 내보내야 되니까 라우터에 `MAC`주소를 담아서 `A`는 `R`에게 전송을 합니다.

그렇게 되면 라우터는 그것을 받아서 목적지 `IP` 주소를 확인해서 

`source`는 자신의 `MAC` 주소를 담아서 전송을 하게 됩니다.

이렇게 전송 하면 `B`가 전송 되어 온 링크 프레임을 링크 레이어 프레임을 받을 수 있습니다.

---
