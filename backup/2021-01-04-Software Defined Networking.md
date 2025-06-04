---
layout: post
title: "네트워크 - Software Defined Networking"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Traffic Engineering with Traditional Routing

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0104/Traffic%20Engineering%20with%20Traditional%20Routing.PNG">

<strong>Traffic Engineering</strong>이라는 말이있습니다.

기존의 데이터 트래픽이 특정한 라우터만 너무 많이 쓰이고 

어떤 특정한 링크에만 데이터가 몰림으로써 

전체적으로 지연시간이 길어지거나 이런 문제가 발생 할 수 있었습니다.

그래서 <strong>데이터 트래픽을 분산</strong>시키거나 

아니면 <strong>보내는 사람이 의도하는 길로 가게 만듦</strong>으로써

전체적으로 `트래픽의 전달을 효율적`으로 만드는 것. 

이것이 <strong>Traffic Engineering</strong>이라는 연구 분야입니다.

예를 들어 위와 같은 그림의 네트워크가 있다하면,

`u`에서 `z`로 가는 트래픽은 `v`와 `w`와 `z`를 통해서 가도록 하고 

`x`에서 `z`로 전달하는 트래픽은 `x`에서 `w`로 그리고 `w`에서 `y`를 

통해서 전달하게 만들고 싶었습니다. 

이렇게 `w`에서 `z`로 전달 되는 트래픽의 양을 조절 하는 개념입니다.

그런데 실제로 기존 라우팅 프로토콜에서는 잘 안됩니다.

### Why?

예를들어 트래픽이 `w`에 도착해서 목적지를 살펴보니 `z`였습니다.

`w`라우터는 이 트래픽이 `u`에서 왔건 `x`에서 왔건 목적지 만을 봅니다.

`u`에서 온 것은 `z`로 바로 보내고 

`x`에서 온 것은 `y`를 통과해서 보내는 것.

이렇게 네트워크 부하를 균형적으로 분산시키는 것을 

<strong>load balancing</strong>이라고 말하지만,

기존 라우팅 프로토콜로는 할 수가 없기 때문에 

네트워크 자원을 효율적으로 사용 할 수가 없었습니다.

# Traditional Network Layer

기존의 라우팅 프로토콜이 아래와 같이 동작 했습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0104/Traditional%20Network%20Layer.PNG">

개별적인 라우터에는 개별적인 라우팅 알고리즘이 들어 있고,

서로가 데이터를 교환 하면서 

각각의 라우터가 전체적인 `topology`를 알아내거나,

`distance vector`로 `vector 알고리즘`을 통해서 

다른 어떤 목적지로의 최단 경로의 최소 cost를 구하는 역할만 했습니다.

그래서 라우팅 알고리즘을 임의로 사용자나 관리자가 교체를 한다던지

하는 것이 불가능 했습니다.

기존에는 회사에서 생산 되어서 나올 때 

이미 라우팅 알고리즘이 들어 가 있고, 그것을 수정하고 싶으면

회사에 다시 연락을 해서 라우팅 알고리즘을 교체할 수 있었지만 

유연하게 할 수는 없었습니다.

# Logically Centralized Control Plane

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0104/Logically%20Centralized%20Control%20Plane.PNG">

이제는 이 라우터들을 <strong>programmable</strong>하게 만듭니다. 

그래서 원하는 라우팅 알고리즘을 넣을 수 있게끔,

그리고 라우팅 알고리즘에서 중요하게 생각하는 요소를 

사용자가 결정하도록 만들 수 있게 되었습니다.

그것을 위해서 네트워크 전체 정보를 하나의 중앙 집중 시스템으로 모은 뒤

정보를 기반으로 각 `source`로부터 각 `destination`으로의 길을 

<strong>remote controller</strong>가 결정을 해 포워딩 테이블을 

각 라우터에 전달 해서 각 라우터들은 이 `remote controller`가 지시한

테이블을 기반으로 전달만 하게 만드는 것이 

<strong>SDN(software Defined Network)의 개념</strong>입니다.

이렇게 `remote controller`는 직접 사용자가 프로그래밍 할 수 있기 때문에

기존처럼 어떤 컨트롤, 제어 영역을 

라우터나 스위치 생산자가 결정하는 것이 아니라

오픈해서 사용자가 직접 그걸 만들 수 있게끔 했습니다.

---
