---
layout: post
title: "네트워크 - Link-State Routing"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Link-Statte Routing Algorithm

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1224/Link-State%20Routing%20Algorithm.PNG">

<strong>Link-state routing</strong>은 전체 `topology 정보`를 알고 상황에서 맞춰 길을 설정합니다.

이것을 위해 라우터들, node들은 자신이 알고 있는 정보들을 다른 라우터들과 교환 해야됩니다.

그 교환 메시지를 <strong>Link-state advertisement message</strong>, `LSA` 메시지 

또는 <strong>Link-state packet</strong>, `LSP`라고 부릅니다.

`LSA`에는 기본적으로 두 가지 정보가 들어갑니다.

## 첫 번째는 neighbor node information 

<strong>직접 연결 되어 있는 node들에 대한 정보</strong> 입니다.

"현재 A라는 라우터는 B, C와 연결 되어 있다"와 같은 상태 정보입니다.

## 두 번째는 link information to neighbors

<strong>neighbor들과 연결 된 링크에 대한 정보</strong>들이 들어갑니다

예를 들면 가장 기본적인 것은 이 링크가 현재 끊어져있거나 활성화 되어 있다.

또는 해당 링크를 거칠 때 지연 시간이 어느 정도 걸리는지에 대한 정보가 담깁니다.

이런 두 가지 정보들을 담아서 node들이 서로 주고 받게 됩니다.

전체적인 과정을 보면 각각의 라우터들이 언제 LSA를 만드냐 하면 

## 기본적으로 이웃이 바뀌었을 때

유선 망에서는 이웃이 바뀌는 경우가 잘 없지만 무선망 같은 경우에는 이웃이 바뀔 수 있습니다.

또는 유선망에서도 기존에 없던 라우터가 새로 연결이 될 수도 있습니다.

## link goes up/down

링크가 활성화 되었거나 비활성화 되었거나 하는 정보나 

네트워크 상태 정보 부여가 되고 이것들이 바뀌었을 때 입니다.

## Periodically

또는 주기적으로 생성 합니다.

그렇게 해서 만들어진 LSA 메시지는 해당 네트워크에 있는 모든 라우터들한테 전송이 됩니다. 

여기서 주의 할 점은 네트워크는 전 세계 네트워크가 아닌 

같은 네트워크의 모든 라우터들한테 `LSA` 메시지가 전달이 된다는 것입니다.

그렇게해서 `LSA` 메시지를 모은 뒤에 각각의 라우터들은 

네트워크의 전체 topology를 구성 해 볼 수 있습니다.

그리고 LSA 메시지들을 기반으로 `source`로부터 `destination` 까지의 `least cost path`. 

최소 비용 경로를 계산 해 볼 수 있습니다.

이런 계산에 쓰이는 대표적인 알고리즘이 <strong>Dijkstra’s algorithm</strong> 입니다.

# Dijkstra’s algorithm

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1224/Dijkstra's%20Algorithm.PNG">

하나의 `source`로 부터 네트워크에 있는 `모든 다른 node`로의 최소 비용 경로를

계산 할 수 있는 알고리즘 입니다.

각각의 라우터에서 <strong>Dijkstra’s algorithm</strong>을 수행을 하게 되면

자기로부터 네트워크에 있는 모든 node로의 최소 비용 경로를 계산 할 수 있습니다.

이것을 기반으로 `포워딩 테이블`을 구현 할 수 있게 됩니다.

# Dijkstra's Algorithm Operation

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1224/Dijkstra's%20Algorithm%20Operation.PNG">

Dijkstra’s algorithm에 대한 코드를 이해 하기 위해서 몇 가지 봐야 될 notation을 살펴봅니다.

<strong>C(x,y)</strong> : x와 y 사이의 link cost를 뜻 합니다. 

처음에 이 비용을 모를 때는 x, y가 직접 연결 되어 있으면 어떤 값을 가지겠지만

연결 되어 있지 않을 때는, 직접 연결이 없을 때는 무한대로 두고 계산을 하게 됩니다.

<strong>D(v)</strong> : 현재 source로부터 목적지 v까지 갈 때 드는 현재 알고 있는 최소 경로 비용을 뜻합니다.

<strong>P(v)</strong> : v로 가기 위해서 직전에 어디를 거쳐 와야 되느냐, 이전에 거치는 node를 뜻합니다.

<strong>N’</strong> : 현재 최소 비용 경로가 계산 된 node들의 집합을 뜻합니다. 

이제 왼쪽의 슈도 코드를 살펴봅시다.

이 알고리즘은 `출발지가 u`로 시작을 합니다.

맨 처음에 u 자신으로의 최소 비용 경로는 이미 알고 있다고 가정을 합니다.

그래서 N’ 이라는 집합에 u는 들어가게 됩니다.

그 다음 `모든 다른 node를 v`라고 표현 했습니다.

반복문 안에서 모든 다른 node들을 하나씩 체크하는데, 

이 node가 만약에 u의 이웃이라면 

즉, 직접 링크가 있다고 하면 u에서 v로의 link cost 값을 `D(v)`로 설정을 합니다. 

만약 직접 연결이 없다면 그 노드가 v로 갈 때의 비용을 무한대라고 설정을 합니다.

그리고 나서 계산 된 `D(v)` 중에서 가장 최소값을 가진 w를 골라 냅니다.

골라 내서 가장 최소값을 가진 어떤 node를 `N’`에 넣습니다. 

지금 `N’`에 포함 된 node를 제외 한 나머지 목적지들에 대해서 

현재 알고 있는 비용값 보다 w 라우터를 거쳐 갈 때 비용이 더 적게 드는지를 살펴 보는 것입니다. 

현재 w까지 가는 비용과 w로부터 목적지 v까지 가는 비용을 더해서

만약에 알고 있는 v로 가는 비용 보다 w를 거쳐 갈 때의 비용이 더 적다면 그 값으로 갱신을 합니다.

그렇게 갱신하고 loop을 돌면서 계속 `N’`이 전체 N하고 똑같아 질 때까지 반복을 수행 합니다.

---
