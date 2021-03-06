---
layout: post
title: "네트워크 - MAC Protocols with Collision(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

이전 네트워크 포스트에서 배웠던 <strong>slotted ALOHA</strong>도 `37%`의 효율성 밖에 내질 못했습니다.

왜 알로하의 효율성이 이것 밖에 되지 않는가에 대해 사람들은 연구했습니다.

그 결과로 <strong>"각 노드들이 다른 노드를 방해 하는 것을 고려하지 않는다"</strong>는 겁니다.

그래서 다른 노드들을 신경쓰는 프로토콜을 만들어내고자 했습니다.

# CSMA

<img src="/images/2021년/0125/CSMA(Carrier Sense Multiple Access.PNG">

<strong>CSMA</strong>는 `Carrier Sense Multiple Access`의 약자입니다.

데이터를 보내는 반송 주파수를 `carrier frequency`라고 합니다.

이 `carrier frequency`를 먼저 감지를 해 본다는 겁니다. 

그래서 채널이 현재 아무도 보내고 있지 않으면 (idle한 상태라면) 데이터를 전송을 하고 

채널이 사용 중 이라면 전송을 딜레이합니다.

```

그림의 예를 들어봅니다.

`workstation A` 가 보낼 데이터가 있으면,

`bus 상`에 누군가 전송하는 데이터가 흐르고 있는지를 살펴 보고

만약에 없다고 하면 자기가 전송을 하고 

그렇지 않으면 전송을 잠시 연기합니다.

```

# CSMA Collisions

<img src="/images/2021년/0125/CSMA Collisions.PNG">

`CSMA`를 사용하면 `collision` 문제가 다 해결이 되는가?

얼핏 생각하기엔 그렇게 될 듯 합니다.

그러나 이렇게 해도 충돌은 발생 했습니다.

```

예를 들어 그림에서 처럼 노드가 `bus 형태`의 네트워크에서 `t0` 라는 시각에 전송을 시작 했습니다. 

노드가 데이터를 전송 했다 하더라도, 

좀 <strong>"멀리 떨어져 있는 노드"</strong>까지 전달 되기 위한 시간(`propagation delay`)이 걸립니다.

그래서 노드가 전송을 하긴 했는데, 

거의 같은 시각에 <strong>"멀리있는 노드"</strong>도 전송 할게 있어서 `bus`를 감지를 합니다.

그런데 <strong>"멀리있는 노드"</strong>가 보내기 직전에 감지 했을 때는 

아무의 데이터도 전송 되고 있지 않았습니다. 

그래서 링크가 비어있다고 판단해서 `t1` 시각에 전송을 하게되고. 충돌이 발생하게 됩니다.

```

해당 예시를 `time diagram`으로 표시를 한 겁니다.

결국은 `격자무늬`로 되어 있는 부분에서 `데이터가 충돌이 난다`는 것을 알게 됩니다.

원래 `CSMA`가 처음 제안이 되었을 때 일단은 충돌이 나는 것은 감지를 할 수 있습니다.

충돌이 났을 때 충돌한 프레임은 버리고, 

다시 `t0`에서 전송 했던 프레임과 `t1`에 전송 됐던 프레임을 `재전송`하는 형식으로 디자인 되었습니다.

그런데 사람들이 유효한 전송을 하지 못하고 낭비 되는 시간이 아까워 

이것을 좀 줄여보고자 새로운 기술을 도입했습니다.

# CSMA/CD(Collision Detection)

<img src="/images/2021년/0125/CSMA_CD(Collision Detection).PNG">

<strong>CSMA/CD, Collision Detection</strong> 이라는 기술을 도입 했습니다. 

`Collision`을 `detection` 하는 것은 무선에서는 어렵습니다.

전송 중에 자기 전송 에너지가 높기 때문에 

다른 노드가 전송 해 오는 <strong>에너지를 같이 감지하는 것은 어렵습니다.</strong>

그런데 유선에서는 자기가 보내는 데이터 외에 

어떤 노드가 데이터를 보내면서 주변에 너무 많은 `에너지가 발생`하기 때문에

자기가 발생 시킨 에너지 이상의 에너지가 감지 되었다고 하면

누군가가 같은 시각에 전송 한 거라고 보고 `충돌을 감지` 할 수 있습니다.

그래서 이 <strong>CSMA/CD</strong>라는 것은 앞의 `기본적인 CSMA`와 비슷한데 

<strong>충돌을 감지하는 순간 바로 전송을 중단</strong>하는 겁니다.

그림의 예시에서 처럼 `t1`시간에 충돌을 감지 함으로써,

프레임을 계속 끝까지 보내지 않고 다음에 `jam signal`이라는 것을 보냅니다. 

> jam signal : 시스템에 약속 되어 있는 어떤 시그널

그렇게 해서 `기본적인 CSMA`에 비해서 충돌이 일어나는 것은 어쩔 수 없지만

낭비되는 시간을 줄여보자 하는 것이 <strong>CSMA/CD</strong>입니다.

---





