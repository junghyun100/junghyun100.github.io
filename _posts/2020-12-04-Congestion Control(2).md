---
layout: post
title: "네트워크 - Congestion Control(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# TCP Congestion Control Overview

TCP congestion control는 방식이 다양합니다. 

그중에서도 공통적으로 들어가는 컨셉이 있습니다. 

가장 대표적인 것이 `AIMD approach` 라는 것입니다.

## AIMD approach

<img src= "https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1204/AIMD%20approach.PNG">

AIMD(Additive Increase Multiplicative Decrease)에서

`Additive Increase`는 차근차근히 증가 시킨다는 것,

`Multiplicative Decrease`는 절반으로 확 줄인다는 것 입니다.

문제가 없을 때는 window size의 크기를,

`Maximum Segment Size`를 천천히 1씩 증가 시키다가 

혼잡을 감지 했다면 확 줄여버리는 것입니다. 

> MSS(maximum segment size) : 하나의 segment 크기가 가질 수 있는 최대 크기

갑자기 줄여서 일단 congestion을 해결 한 후에 

다시 천천히 조금씩 증가시키는 방향으로 하는 것입니다.

이런 식의 동작을 계속 취하면서 

전체 네트워크에 너무 많은 트래픽을 흘려 보내지 않도록 

스스로 조절을 해 가는 것입니다.

## Congestion Window(cwnd)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1204/Congestion%20Window.PNG">

`congestion window`라는 것은 

네트워크가 congestion을 일으키지 않는 한도 내에서

최대한으로 한꺼번에 보낼 수 있는 ACK를 받지 않은 상태로 

한꺼번에 전송 할 수 있는 데이터의 양 입니다.

그림에서는 노란색을 뜻하는 것입니다.

이것이 항상 `congestion window`와 `receive window(receive buffer)`보다는 작아야 됩니다.

그래서 CWND, congestion window는 다이나믹하게 변화 합니다.

Network congestion의 상황에 따라서 다양하게 변화 하는 것을 볼 수 있습니다.

# Changes of cwnd : Slow Start

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1204/Change%20of%20cwnd.PNG">

처음에 연결을 수립하고 나면 처음에는 `congestion window size`를 어떤 값으로 보내야 될지 모릅니다.

그래서 초기 congestion size 는 1 MSS. 이 말은 한 개의 segment를 보낼 수 있다는 것입니다.

그래서 호스트 A가 처음에 연결이 처음이 수립 되고 나면 한 개의 segment를 보내고,

호스트 B는 잘 받으면 거기에 대한 ACK를 보내게 됩니다.

이 ACK를 받고 나면 호스트 A는 아까 보낸 데이터가 잘 도달 했으니까 

이번에는 이것의 두 배를 보내 볼까 이렇게 생각을 합니다.

그래서 거기에 두 개의 segment를 보내게 되고, 호스트 B는 거기에 대해서 ACK를 보내게 됩니다.

이런 식으로 Congestion window size가 여덟 개의 maximum segment size로 증가합니다.

그렇게 해서 전체 Congestion window size가 `exponential`하게 

하나에서 두 개, 두 개에서 네 개, 여덟 개 이런 식으로 증가 하게 됩니다.

그래서 이 메커니즘의 이름이 slow start인데 이 이름을 보아서는 좀 천천히 보낼 것 같지만,

Initial rate는 굉장히 낮지만 이것은 굉장히 빠르게 증가하는 메커니즘 입니다.

# Changes of cwnd : Congestion Avoidance

모든 node들이 exponential하게 window size를 늘리게 되면, 

결국은 congestion이 금방 발생 할 것이고, 메커니즘을 수행해야 되고 하는것이 번거로웠습니다.

그래서 `congestion avoidance`라는 congestion을 회피하고자 하는 이런 방식을 도입 했습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1204/Change%20of%20cwnd%20-%20Congestion%20Avoidance.PNG">

이것은 slow-start의 지수적인 증가가 언제까지 이루어질 것이냐,

congestion이 금방 또 발생 할 것이니까, 이전에 congestion window size가 있었을 것입니다.

loss가 일어나기 이전 값의 절반인 congestion window size의 절반을 

`slow-start threshold` 라는 값으로 설정을 합니다.

그러면 congestion window size가 1에서 2로, 4로, 8로 증가 하다가

slow-start threshold 값, 이전 congestion이 발생하기 이전의 절반 값에 도달 하면

그 때부터는 Additive Increase 방식으로 이렇게 1씩 증가 합니다. 

그래서 ssthresh(slow-start threshold)를 넘어가게 되면 congestion window size 값을 

매 RTT마다 1 MSS씩 증가 시키는 이런 방식으로 동작을 하는 것이 `congestion avoidance` 입니다.

---
