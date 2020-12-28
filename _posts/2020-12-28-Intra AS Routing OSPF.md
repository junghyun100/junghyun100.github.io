---
layout: post
title: "네트워크 - Intra AS Routing OSPF"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Scalability of Routing

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1228/Scalability%20of%20Routing.PNG">

지금까지는 인터넷 네트워크가 오른쪽의 그림처럼 생겼다고 가정을 했습니다. 

여기서는 모든 라우터들이 동일하며, 네트워크가 <strong>"플랫"</strong>한 네트워크라고 가정합니다.

그러나 실제 인터넷은 <strong>트리 형태의 하위 계층을 가지는 형태</strong>의 네트워크입니다.

그리고 그림처럼 모든 라우터들이 플랫한 환경에 있으면 문제가 생니다.

왜냐하면 전 세계에 수많은 라우터와 인터넷 장비들이 있을텐데 

모든 데스티네이션, 모든 목적지에 대한 정보를 

라우팅 테이블이 담는다는 것은 사실 불가능 한 것입니다.

참여자의 수가 적을 때는 잘 동작을 하다가 만약에 참여자 수가 늘어남에 따라

급격하게 성능이 나빠져서 결국 어느 수 이상이 되면 전혀 동작하지 않는 경우

시스템에는 <strong>scalability issue</strong>가 있는 것입니다.

그래서 이런 것이 라우팅을 적용하는 데 있어서도 고려가 되어야 되는 이슈입니다.

# Autonomous System

인터넷의 의미 자체가 `inter`와 `net`의 합성어로 

네트워크의 네트워크다 의미를 가집니다.

또 각각의 네트워크들은 각자의 관리자들이 있고, 

각각의 관리자들이 관리하는 네트워크들의 전체 컬렉션, 

집합 네트워크가 인터넷입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1228/Autonomous%20System.PNG">

이 두 가지의 개념을 인터넷에서는 

<strong>Autonomous system</strong>이라는 개념으로 정의를 했습니다.

그래서 오른편에 있는 그림처럼 

각각의 `autonomous system A`, `B`, `C`, `D`, `E`로 구별을 한 뒤

링크로 연결해서 전체 네트워크를 구성하는 식의 개념이 인터넷입니다.

> autonomous system : single administrative entity, <br>한 관리 기관에서 관리하는 IP 네트워크의 집합.

그렇다고 하나의 <strong>autonomous system</strong>은 

라우터들이 모두 동일한 네트워크 ID를 갖는게 아니라 갖는다고 볼 수도 없고,

여러 개의 네트워크 ID가 들어갈 수도 있습니다.

> 서로 다른 네트워크 ID를 가지더라도 한 기관에서 관리하면<br> 하나의 AS로 볼 수도 있습니다.

# Internet Approach to Scalable Routing

<img src="https://github.com/junghyun100/junghyun100.github.io/blob/master/images/1228/Internet%20Approach%20to%20Scalable%20Routing.PNG">

`AS`를 정의를 하고 나면 라우팅을 

<strong>Intra-AS routing(AS 내부 라우팅)</strong>과 <strong>Inter-AS routing(AS 외부 라우팅)</strong>으로 구별 할 수가 있습니다.

## Intra-AS routing

하나의 `AS` 안에 속해 있는, <strong>같은 AS에 속한 라우터들 끼리의 라우팅 알고리즘</strong>입니다.

그래서 한 `AS` 내에서 모든 라우터들은 동일한 인트라 도메인 프로토콜을 사용 해야 합니다.

대표적인 intra-AS 라우팅은 `Link-state 프로토콜`, 

그리고 Distance vector 알고리즘의 대표 격들인 `OSPF`나 `RIP` 등이 속하는데

하나의 AS 내에서는 <strong>동일한 OSPF를 쓴다던지 아니면 동일한 RIP</strong>를 사용해야합니다.

## Inter-AS routing

하나의 `AS` 안에 여러 개의 라우터가 있을 수 있는데

그 중에 외부 다른 `AS`와 링크를 갖는 이런 라우터를 경계 라우터라고 해서 `border router`라고 이야기 합니다.

이러한 라우터들이 외부의 어떤 `gateway`로부터 통신을 하기 위해서는 동일한 프로토콜을 

사용해야 하고 그것의 대표격이 `BGP`입니다.

이렇게 외부 AS 끼리 라우팅을 할 때는 <strong>gateway router가 inter-domain routing을 동작</strong>을 해야합니다.

# Interconnected ASes

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1228/interconnected%20ASes.PNG">

그러면 하나의 라우터, 그것이 꼭 `gateway router`가 아니다 하더라도 어떤 `AS` 안에 있는 하나의 내부 라우터도

완전히 `intra-AS 라우팅 알고리즘` 만으로 포워딩 테이블을 구성하는 것은 아닙니다.

예를 들어 `1d`가 전달 해야 되는 패킷들이 목적지가 같은 AS 내부 일 수도 있지만 

목적지가 외부일 경우도 있습니다.

이 내부에서 내부 목적지를 찾아 갈 때는 `intra-AS 라우팅 알고리즘` 만으로도 충분하지만,

만약에 외부에 `2c`라는 목적지를 찾아간다고 하면 이 `1d` 라우터는 `2c`가 어디에 존재하는지 

그 경우에 `1b`로 내보내야 하는지 `1a`를 거쳐서 `1c`로 내보내야 하는지를 알지 못합니다.

그래서 `inter-AS 라우팅 알고리즘`의 도움이 필요합니다.

외부 AS간의 통신을 통해서 `2c`라는 라우터가 `AS2`에 존재한다는 사실을 

알아 내야만 `1d`가 `2c`를 목적지로 하는 데이터그램을 `1b`를 통과해서 나가게 만들 수 있습니다.

그래서 결국 하나의 라우터 내의 포워딩 테이블은 

`intra-AS 라우팅`과 `inter-AS 라우팅`의 동작이 합쳐져서 포워딩 테이블을 구성 할 수 있습니다.

---
