---
layout: post
title: "네트워크 - Inter AS Routing BGP(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# IP-Anycast

<img src="">

IP 주소 클래스에 A, B, C class는 

실제 라우터, 인터넷 호스트에 주소를 할당하는 규칙을 이야기 했습니다.

그런데 주소에 보면 class D와 E라는게 있습니다.

이것들은 실제로 라우터에 할당하기 위한 주소가 아니고

목적지 IP 주소 필드에다 사용하기 위해서 만든 것이 class D와 E 입니다.

> 앞의 4 bit가 1110일 때 class D 주소이며, multicast의 용도

> 앞의 4 bit가 1111일 때 class E 주소이며, Anycast의 용도

### Multicast

<img src="https://upload.wikimedia.org/wikipedia/commons/3/30/Multicast.svg">

Multicast는 source가 하나고, destination이 여러 개인 경우입니다.

multicast group ID를 28 bit에 따라서 multicast group에 속하는 다른 노드들이, 

호스트들이 이 데이터를 받을 수 있게끔 디자인 된 것이 class D 주소입니다.

### Anycast

<img src="https://commons.wikimedia.org/wiki/File:Anycast.svg">

Anycast는 source가 하나고, destination이 여러 개인 경우 중 하나만이 데이터를 받습니다.

Anycast가 사용 되는 대표적인 예는 Domain Name System으로

컴퓨터에서 발생 시킨 DNS query가 전 세계에 흩어져 있는 route DNS 시스템 중에 

가장 가까 시스템을 찾는 것이 anycast로 가능 하기 때문입니다.

### Example

<img src="">

도메인 시스템이라고 하면 서버 A, B, C에 동일한 IP주소를 할당합니다.

예를 들어 봅니다.

전 세계 호스트는 서로 유니크한 주소를 가져야만 그 호스트를 찾아 갈 수 있지만,

anycast를 구현 할 때는 이 중에 모든 Domain Name Server나 

Contents Delivery Network Server들 한테 10.10.10.10의 동일한 IP주소를 할당합니다.

각각의 라우터들은 각각의 AS에 포함되어 있다고 가정합시다.

AS들 간의 BGP 메시지를 통해서 정보가 전달 되어 올 겁니다.

그러면 이 정보를 받은 호스트는 

AS1을 통하면 10.10.10.10으로 갈 수 있고,

AS2, AS3을 통하면 10.10.10.10으로 갈 수 있고,

AS4, AS5, AS6을 통하면 10.10.10.10으로 갈 수 있다 라고 판단을 합니다.

실제 물리적으로는 서로 다른 서버지만 다른 서버라는 것을 알지 못하고

하나의 서버라고 생각을 하고 하나의 서버로 가는 서로 다른 길로 인식합니다.

그래서 그 중에 가장 적은 AS를 거치는 AS1과 통하는 길로 메시지를 전달하게 되면

결과적으로 여러 DNS 시스템 중 가장 가까이 있는 DNS 시스템에게 query를 전달합니다.

# Difference between Intra- and Inter-AS Routing

<img src="">

BGP 마지막으로 Intra-AS routing과 Inter-AS routing을 비교하면서

왜 서로 달라야 하고 그리고 어떤 부분이 다른가 확인합니다.

### Why

서로 달라야 하는 이유는 scalability 때문입니다.

모든 라우터가 한 레벨에 다 놓여져 있다고 하면 

서로 주고 받아야 되는 라우팅 테이블 업데이트 트래픽 때문에 

전체 네트워크가 마비가 되는 사태까지 벌어질 수 있습니다.

그래서 autonomous system이 관리하는 라우터의 집합으로 구별을 하고,

하나의 AS 내에서는 Intra domain routing protocol로 자세한 정보를 주고 받고,

서로 다른 AS 간에는 inter domain routing protocol을 가지고 연결함으로써 

좀 간단한 연결 정보만을 주고 받는 식으로 동작합니다.

### What

어떤 부분이 다르냐 보면 inter-AS에서는 administrator가 내부를 통과하는

트래픽들을 정책을 통해서 관리하고 싶은 것이 목적입니다.

반면에 Intra-AS는 내부의 라우터들은 자기가 속한 AS에 대한

최신의 그리고 가능한 한 가장 자세한 정보를 가지게 함으로써

경로를 설정할 때 가장 효율적인 경로를 설정할 수 있게 하기 위함입니다.

---
