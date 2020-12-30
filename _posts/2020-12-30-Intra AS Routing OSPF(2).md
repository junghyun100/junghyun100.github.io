---
layout: post
title: "네트워크 - Intra AS Routing OSPF(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Routing Protocols

<img src="">

라우팅 프로토콜을 간략하게 구별을 해 보면 intra-AS과 inter-AS 라우팅으로 구별을 할 수 있고,

Intra-AS 라우팅의 대표적인 것이 Distance vector와 Link-state으로

각각에 속하는 상용 프로토콜은 RIP, Routing Information Protocol과

OSPF, Open Shortest Path First 라는 알고리즘이 있습니다.

Inter-AS 라우팅의 대표적인 것은 Path vector 입니다.

이것에 속하는 프로토콜은 현재 거의 표준으로 쓰이는 BGP, Border Gateway Protocol 입니다.

# OSPF(Open Shortest Path First)

<img src="">

OSPF는 Open Shortest Path First의 약자입니다. 

### Open

여기서 Open은 publicly available 하다는 뜻으로,

한 AS 내에 여러 라우터들이 있다면 라우터들의 제조 회사가 다 동일하지 않을 수도 있습니다.

그럴때 각각의 라우터가 가지고 있는 라우팅 알고리즘, OSPF이 동일하지 않을 수도 있습니다.

그렇지만 OSPF의 표준 문서에서 요구하는 요구 사항을 따르기만 하면

같은 AS에 속하는 다른 라우터들과 통신하는데 전혀 문제가 없다 는 뜻으로 Open이라 합니다.

> 개방형 프로토콜 : 다른 노드와 통신하는 방식을 공개된 표준 문서의 요구사항에 맞춘 프로토콜

> 폐쇄형 프로토콜 : 다른 노드와 통신하는 방식을 알려주지 않는 프로토콜

### Shortest Path First

라우팅 알고리즘이 경로를 결정하는 가장 주된 기준이 shortest path입니다.

다양한 Link-state 알고리즘 중에서 가장 많이 쓰이는게 OSPF고, 

OSPF는 오른쪽 그림 처럼 OSPF 내용이 있으면 앞에 IP 헤더가 붙습니다.

필요로 하는 데이터의 IP 헤더가 붙는다는 것은 IP보다 상위 layer에서 온 메시지라는 뜻 입니다.

OSPF는 Intra-AS 라우팅 프로토콜이고, 

같은 AS에 속한 라우터들 끼리 서로 broadcast 방식으로 

link-state advertisement message를 주고 받기 때문에

이걸 통해서 해당하는 AS 내의 topology map은 정확하게 알 수 있게 됩니다.

그걸 기반으로 Dijkstra’s 알고리즘을 통해서 최단거리를 찾아냅니다.

# Hierarchical OSPF

<img src="">

만약 하나의 AS가 너무 많은 수의 라우터를 포함 하고 있다면 

라우터들 간에 주고 받는 broadcast massage가 AS에 굉장히 부담이 될 수 있습니다.

그래서 하나의 네트워크를 서브넷으로 나누어서 관리를 했듯이 

하나의 AS도 여러 개의 area로 나누어서 

각각의 area에 OSPF가 동작하게끔 별도로 동작하게끔 할 수 있습니다.

이런 것을 hierarchical OSPF라고 합니다.

<img src="">

AS를 위에서 내려다 보는 장면으로 가정을 해 보면 

하나의 autonomous system가 몇 개의 area로 구별이 되고,

area 간을 이어주는 border gateway router들이 존재 합니다.

여기서 Link-state advertisement message는 

한 area 내에서만 공유가 되므로, topology에 대한 상세한 정보들은 

속한 area에 대해서만 상세한 정보를 각 노드들이 가질 수 있으며,

border router도 area border router와 backbone router로 나눌 수 있습니다.

area border router는 이 하나의 area의 정보를 요약해서 전달 하는 기능을 갖고 있고

backbone router는 이런 area들을 연결 시켜 주는 그런 역할을 합니다.

---
