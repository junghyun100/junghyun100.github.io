---
layout: post
title: "네트워크 - Inter AS Routing BGP"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

AS 간 라우팅의 대표 프로토콜은 BGP 입니다.

Border Gate Protocol이 어떤 식으로 동작하는지 알아봅니다.

# Inter-AS Tasks

그림에서 AS1에 있는 1d 라우터가 자기가 전달해야되는 데이터그램을 받았습니다.

이 데이터그램의 목적지 주소의 네트워크 ID를 보니까 AS1에 속한 라우터가 아닙니다.

다른 AS에 있으니 외부로 내보내야 하는데

이 데이터그램을 1d는 1b 쪽을 거쳐서 AS2를 통과해서 내 보내야 할지

1c를 거쳐서 AS3 쪽으로 전달 해야 될 지를 판단하지 못합니다.

그래서 inter-AS routing protocol이 해당 역할을 담당해줘야 합니다.

그러기 위해서는 AS1에 있는 라우터들은 

AS2를 통해서 전달 할 수 있는 destination들과

AS3를 통해서 전달 할 수 있는 destination 주소들이 

어떤 것들이 있는가를 반드시 알고 있어야합니다.

여기서 AS2나 AS3를 통해서 전달 할 수 있는 네트워크 정보를 

reachability information 이라고 합니다.

# BGP(Border Gateway Protocol)

<img src="">

inter-AS routing protocol의 대표가 BGP, Border Gateway Protocol이고,

국제기구에서 표준으로 결정 한 것은 아닙니다.

그렇지만 BGP가 거의 표준처럼 동작하고 있습니다.

그래서 그림에서도 모든 AS들 간에 border router들 끼리 통신을 할 때

BGP로 서로 통신을 합니다.

실제로 인터넷이라는 것이 네트워크의 네트워크듯이 

각각의 AS들은 개별 관리자에 의해서 관리가 된다 하더라도 

결국 AS들이 연결 되어야만 그것이 진정한 의미의 인터넷입니다.

여기서 인터넷으로 만드는 것은 BGP의 역할이나 다름이 없습니다.

BGP는 또 크게 두 부분으로 나눠 볼 수 있습니다.

하나는 eBGP, external BGP라는 거고 iBGP는 internal BGP라는 겁니다.

# eBGP, iBGP Connection

<img src="">

### eBGP

서로 다른 AS에 속하는 border router들이 

BGP를 통해서 정보를 주고 받을 때는 eBGP

> 정확히는 eBGP 프로토콜을 통해서 데이터를 주고 받습니다.

### iBGP

eBGP를 통해서 얻어 온 AS A에 대한 정보를 

내부에 있는 AS B에 있는 다른 라우터들과도 공유할 때 

정보를 전달해 주는 역할을 하는게 iBGP 입니다.

> BGP 정보를 내부 라우터들한테 전달 해 주는 것이 iBGP 프로토콜 입니다.

# BGP Basics

<img src="">

두 개의 Border 라우터들이 BGP 메시지를 주고 받는데 

BGP 메시지는 TCP를 기반으로 정보를 주고 받습니다.

> 중요한 정보로 안정성, 신뢰성 있는 TCP를 기반으로 동작합니다.

사실 완벽한 permanent, 항상 영원한 연결은 아니지만 semi-permanent로

두 AS 간에는 TCP 연결이 거의 항상 연결이 되어 있다 하는 거고

그 연결 된 상태에서, BGP 메시지를 주고 받습니다.

BGP 메시지에는 기본적으로 특정한 destination network prefixes

특정한 네트워크 주소를 목적지로 가진 데이터그램을 전달하기 위한 path 정보를 전달합니다.

---
