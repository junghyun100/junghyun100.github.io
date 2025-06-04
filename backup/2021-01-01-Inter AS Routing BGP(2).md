---
layout: post
title: "네트워크 - Inter AS Routing BGP(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Path Attributes and BGP Routes

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0101/Path%20Attributes%20and%20BGP%20Routes.PNG">

`BGP 메시지`를 통해서 전달하는 정보는 여러가지가 있지만 

가장 중요한 것이 <strong>network prefix</strong>입니다.

> network prefix : IP 주소 중에 네트워크 ID에 해당하는 부분입니다.

`네트워크 ID`를 목적지로 하는 데이터그램이 있으면 

처리방식에 대한 정보가 `attribute`에 있고,

`network prefix`와 처리 방법이 기술 됨으로써 

이게 하나의 루트, 경로 정보로 전달이 되게 됩니다.

`attribute` 중에 가장 중요한 두 가지를 꼽으라면 

<strong>AS-PATH</strong> 와 <strong>NEXT-HOP</strong>가 있습니다.

<strong>AS-PATH</strong>는 `path vector(AS넘버로 이루어 진 path)`입니다.

<strong>NEXT-HOP</strong>은 AS로 전해 주기 위해서 

다음에 선택 해야 될 `next hop router`를 뜻 합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0101/Path%20Attributes%20and%20BGP%20Routes(2).PNG">

그림처럼 가정 해 봅시다.

`3a`라는 border router가 `AS3`에 `X`라는 네트워크가 

존재한다는 것을 알아 냈습니다.

그래서 `3a`는 `AS2`에 있는 border router `2c` 측으로

`X`라는 목적지로 가려한다면 `AS3`를 거쳐야 `X`로 갈 수 있다는 정보를

<strong>eBGP 메시지</strong>를 통해서 `2c` 한테 알려줍니다.

그러면 `2c`는 `AS2` 내부의 다른 라우터들 한테도 정보를 알려줘야합니다.

그래서 <strong>iBGP 메시지</strong>를 통해서 전달을 하게 되는데 

이 때 전달하는 정보가 `X`가 가지고 있는 

<strong>network prefix</strong>와 <strong>AS-PATH</strong> 정보 입니다.

> 정보의 내용 1. AS3와 X를 거치면 그 네트워크로 갈 수 있다는 것

> 정보의 내용 2. NEXT-HOP은 2c라는 것

이를 통해 기본적인 BGP는 동작을 살펴 보았습니다.

### Policy-Based Routing

`inter domain routing protocol`은 분류를 하자면 

<strong>policy-based routing</strong> 이라고 합니다.

<strong>intra domain routing protocol</strong>은 `AS` 내부에 대한 정보를 

모든 노드들이 정확하게 공유하고 있는 것이 중요했습니다.

그래서 정확하게 공유한 정보를 기반으로 목적지를 선택하면 

가장 효율적인, 가장 좋은 경로를 선택 할 수가 있었는데

BGP는 <strong>policy를 더 중요</strong>하게 생각합니다.

# BGP Policy Achievement via Advertisements

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0101/BGP%20Policy%20Achievement%20via%20Advertisements.PNG">

그림과 같은 네트워크가 있다고 가정 합니다.

네모로 되어 있는 `A`, `B`, `C` 네트워크는 ISP,

여기서 `B`와 `C`는 서로 경쟁 관계의 인터넷 서비스 Provider입니다.

> ISP : Internet Service Provider 인터넷 제공 업자들의 네트워크

그리고 고객 네트워크들을 `W`, `X`, `Y` 라고 표현했습니다.

그러면 `A`와 `B`와 `C`는 각각 개별적인 AS이기 때문에 

서로간에 BGP 메시지를 주고 받을 텐데

`A`는 `B`와 `C`한테 `Aw`라는 네트워크로 가려면 

결국 `A`를 통하면 `W`로 갈 수 있다는 정보(path vector 정보)를 가르쳐 줍니다.

그러면 `B`는 `BAw`라는 경로 vector 정보(path vector 정보)를 알게 됩니다.

이것을 `X`측에 전달 해 `X`는 ‘`W`로 보내려면 `B`와 `A`를 통과해서 `W`로 갈 수 있구나’ 

라는 것을 알 수 있겠지만 `B`는 `C`한테는 알려 주지 않는다는 겁니다.

<strong>그에 대한 이유</strong>로는 이런 경우를 생각해 봅니다.

`C`의 입장에서 `C`와 `A` 사이에 링크를 전달해서 가면 되는데 

`C`가 이 링크의 `bandwidth`를 아껴 보겠다고 `B`를 통과해서 보낼 수도 있다는 겁니다.

그렇게 했을 경우에 `B`는 괜히 링크의 `bandwidth`만 소모되게 되고, 

내부 노드의 자원도 소모되기 때문에 포워딩 해 주기 싫을 것 입니다.

일부러 알려 주지 않음으로써 `C`가 `W`쪽으로 보내야 될 정보를 

`B`를 통과해서 가지 않도록 만드는 Policy가 구현되게 됩니다.


---
