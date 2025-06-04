---
layout: post
title: "네트워크 - Overview of Network Layer"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Overview of Network Layer

네트워크 계층 프로토콜의 역할은 원격에 있는 호스트를 구별하게 해 주는 것입니다.

> 예 : 편지에 주소를 적으면 편지를 받아야 되는 사람의 주소지 까지는 전달이 되는 것

네트워크 계층에 있는 IP 프로토콜을 이용해 IP 주소가 각각의 호스트에 부여 되어서

해당 주소를 가지고 다른 서버나 컴퓨터를 찾아 갈 수 있게 해 주는 역할입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1208/NetworkLayer.PNG">

<strong>클라이언트(Sending side)</strong>에 있는 네트워크 계층이 하는 역할은 

트랜스포트 계층에서 내려주는 `TCP`나 `UDP segment`를 `Encapsulate`합니다.

`Encapsulate`은 IP 프로토콜에 프로토콜의 기능을 위한 헤더 정보를

`segment` 앞에 붙여서 IP 레벨에서의 `datagram`을 만듭니다.

그래서 IP 헤더가 붙어 있는 `datagram`을 아래 계층으로 내려 주어서

유선이든 무선이든 물리적인 링크를 따라서 목적지로 전달이 되게 됩니다.

그 과정에서 라우터를 통과 해서 서버에 있는 네트워크 계층에 다다르게 됩니다.

<strong>서버(Receving side)</strong>에 있는 네트워크 계층이 하는 역할은 

`datagram` 받아서 전송 계층에 올려 주는 그런 역할을 합니다.

중간 마다 있는 라우터가 하는 기능은 전달 되어 오는 IP datagram 헤더를 살펴 보고, 

헤더의 주소를 보고 어느 방향으로 보내야 하는지를 결정합니다.

# Two Key Functions in Network Layer

네트워크 계층에는 두 가지 핵심적인 기능이 있다고 말을 합니다.

첫 번째는 `라우팅(routing)` 두 번째는 `포워딩(Forwarding)` 입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1208/Two%20Key%20Function%20in%20Network%20Layer.PNG">

<strong>라우팅</strong>은 목적지부터 도착지까지 거쳐가야 되는 길을 결정 해 주는 방식입니다.

여기에 사용되는 알고리즘을 라우팅 알고리즘이라고 이야기 합니다.

<strong>포워딩</strong>은 결정 되어 있는 라우터를 따라서 패킷을 옮겨 주는 역할을 합니다.

결국 포워딩이라는 것은 패킷을 다음 node로 전달 해 주는 그 기능입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1208/Two%20Key%20Function%20in%20Network%20Layer%202.PNG">

그림이 있는 것처럼 어떤 패킷이 라우터에 도착 했습니다.

도착하면 라우터가 하는 역할은 어떤 포워딩 테이블이 결정 되어 있다고 하면

그 테이블에선 `0111은 어디로 가야 되느냐, 2번 링크로 내 보내야 된다`라고 작성되어 있습니다.

이 라우터가 주소를 보고 바로 2번 링크로 내보내 주는 역할을 하는 것이 포워딩입니다.

그것을 위해 포워딩 테이블을 만들 때 정보를 제공 하는 알고리즘이 라우팅 알고리즘들 입니다.

그래서 기존에 라우터에서는 사실 이런 용어를 잘 쓰지는 않았었는데 

흔히 `SDN(software defined network)`이라고 줄여서 말 하는 기술이 있습니다.

네트워크를 제어하는 역할이라고 해서 `control plane` 역할이라고도 말합니다.

라우팅 알고리즘이 결정 해 주는 길을 따라서 단순 포워딩만을 하는 `data plane` 라고 말합니다.

최근에는 라우팅 알고리즘의 역할을 `control plane function 기능`이라고 말을 하고,

포워딩 하는 것은 `data plane function` 이렇게 이야기 합니다.

# Traditional vs SDN Network

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1208/SDN.PNG">

기존의 IP 네트워크는 라우팅과 포워딩이 하나의 동일한 시스템 내에서 다 이루어 졌습니다.

반면에 최근에 나온 software-defined network라고 하는 것은 이 라우팅과 포워딩이 분리됩니다.

물리적으로 분리가 되거나 합해져 있을 수 있지만, 논리적으로는 분리되어 있습니다.

SDN은 오른쪽 그림처럼 각각의 라우터들은 포워딩 기능만을 하고,

길을 설정 해 주는 것은 어떤 중앙 집중적인 시스템이 있다 이렇게 가정을 하시면 됩니다.

---
