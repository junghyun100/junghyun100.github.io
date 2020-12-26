---
layout: post
title: "네트워크 - Distance Vector Routing"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Distance Vector Routing 

<img src="">

distance vector routing은 bellman-ford equation을 기본으로 합니다.

bellman-ford equation은 cost를 계산 할 때 한 경로가 아니라 

x로부터 y로의 최소 비용 경로의 cost를 Dx(y)로 둡니다.

Dx(y)는 c(x, v) + Dv(y) 들 중에 가장 작은 값 입니다.

> c(x, v) : x에서 v로의 링크 cost

> Dv(y) : v가 가지고 있는 y로의 경로값

# Bellman-Ford Example

<img src="">

예를 살펴 봅니다. 

현재 v에서 z로의 비용은 5, x에서 z로의 비용은 3, w에서 z로의 비용은 3으로 알고 있습니다.

여기서 bellman-ford equation을 통해서 

u에서 z로 가는 최소 비용 경로의 비용을 알고 싶습니다.

그러기 위해 v, x, w의 link cost와 

각각 v에서 z로의 비용, x에서 z로의 비용, w에서 z로의 비용을 더합니다.

그럼 2 + 5, 1 + 3, 5 + 3으로 3가지의 값이 나올 수 있게 됩니다.

그러면 중간에 있는 x를 거쳐 갈 때 1 + 3으로 최소 비용이 4가 된다는 것을 알 수 있습니다.

# Distance Vector Algorithm

<img src="">

distance vector algorithm도 node들은 기본적으로 

각각의 직접 연결이 있는 이웃으로의 경로 비용, c(x,v)를 알고 있습니다.

그리고 각각의 node들은 네트워크 내에 있는 다른 node들로의 비용 값을 

유지 하는 Dv 테이블을 가지고 있습니다.

그리고 때때로 node들은 distance vector table을 이웃 node들과 교환을 하게 됩니다.

그래서 distance vector algorithm이 Link-state algorithm 와의 차이점은

Link-state algorithm은 advertisement massege를 네트워크 전체로 broadcasting 하는 데 반해서

distance vector algorithm은 자신의 이웃 node들하고만 서로 교환을 합니다.

그 정보들이 바뀌었다면 그것을 기반으로 distance vector를 갱신 합니다.

갱신 하는 방식은 아까 보았던 bellman-ford equation을 통해서 갱신합니다.

# Distance Vector Algorithm : Example

<img src="">

예를 살펴 보면 오른쪽 그림과 같은 이런 간단한 그래프로 가정합니다.

x, y, z는 직접 연결이 되어 있는 상태이고, 

이것을 기반으로 node x, node y와 node z의 테이블을 만들었습니다.

여기서 x는 y와 z와 서로 distance vector message를 교환이 한 번 일어납니다.

그러면 x는 일단 계산 해 봅니다.

y로는 2의 비용으로 갈 수 있고, z를 거쳐가면 8의 비용이 들게 됩니다.

이 중 값이 적은 2의 값으로 가도록 설정합니다.

반대로 z로 가는 비용은 3, 그리고 7로 나오게 되는데 3이 비용이 적게 들기에

3으로 포워딩 테이블을 설정합니다. 

이렇게 계속 교환이 일어나면서 테이블이 완성이 됩니다.

---
