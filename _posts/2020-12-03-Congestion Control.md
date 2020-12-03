---
layout: post
title: "네트워크 - Congestion Control"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Network Congestion

1980년대 이전에 1970년대에 처음 인터넷이 만들어 졌을 때에는

`congestion`이라는 것에 대한 고려가 없었습니다.

당시에는 연결 되어 있는 node의 수도 작았고,

각각의 node가 발생 시키는 트래픽의 양도 작았기 때문에

네트워크에서 혼잡으로 인해 목적지에 제대로 도달하지 못하는 경우를 

잘 발견하지 못 했기 때문입니다.

1980년도에 발표 한 내용에서의 그림입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1203/NetworkCongestion.PNG">

가로 축이 네트워크에 제공 되는 전체 트래픽의 양,

세로 축이 실제로 목적지까지 잘 전달 되는 트래픽의 양입니다.

`capacity` 라는 것은 네트워크가 가지고 있는, 

전달 할 수 있는 최대의 용량이라는 것입니다.

제공 되는 트래픽에 따라서 얼마나 전달이 되느냐 살펴 보니

`offered load`가 작을 때는 네트워크에 트래픽들이 잘 도달 했습니다.

`offered load`와 `effective load`가 linear하게 증가 하다가 

어느 순간이 되면 조금씩 낮아집니다.

조금씩 곡선으로 가다가 최대 점을 찍고나면 다음부터 줄어들게 됩니다.

결국은 트래픽이 굉장히 많아지면 전달하지 못하게 되고, 

실제 전달 되는 양이 `0`으로 줄어들어 버리는 문제가 발생했습니다.

## Why?

트래픽의 양이 많아지면 처리 해야 될 데이터의 양도 많아지고,

각각의 패킷이 겪게 되는 지연 시간도 길어 집니다.

지연 시간이 길어 지면 sender에서 타임 아웃이 자주 일어나고,

타임 아웃이 일어나면 재전송이 일어납니다.

아까 전송 한 패킷도 전달 되지 못한 상태에서 새로운 패킷이 더해지고,

결국은 점점 아무 것도 전달 하지 못하는 현상이 발생 하는 것입니다.

`congestion`으로 인해서 전달 되는 데이터 양이 무너져 버리는 현상에

`congestion collapse`라는 용어를 사용 했습니다.

## Different from flow Control

`flow control`와의 차이점으론 flow control은 `point-to-point 이슈`, 

그러니까 sender와 receiver 간의 문제 라는 것입니다.

Flow control은 receiver의 송신 버퍼의 크기를 넘어서는 속도로,

sender가 보내지 않도록 합니다.

`Receive window`의 헤더에 비어 있는 `receive buffer 크기`를,

알려 줌으로써 flow control은 해결이 되었습니다.

`congestion control`은 1:1의 문제가 아니라 

해당 네트워크를 공유하고 있는 모든 node들이 양보를 해야 합니다.

즉, 전체가 관련 된 글로벌한 이슈입니다.

# Congestion Control Approaches

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1203/Congestion%20Control%20Approaches.PNG">

Congestion control은 여러 가지 방법들이 제안 되었습니다.

크게 두 부류로 나누면 하나는 `end-to-end 방식`, 또 하나는 `network-assisted 방식` 입니다.

### end-to-end 방식

라우터가 전혀 관여하지 않고 어떤 새로운 정보를 주는 것이 아니라 

호스트들이 자기가 보낸 데이터가 얼마나 잘 도달 하는가 이것을 관찰 해서 

중간에 있는 네트워크에서 혼잡이 있다 없다를 판단합니다.

그리고 거기에 맞는 동작을 취하는 방식입니다.

예를 들어서, 데이터를 보내는 족족 타임 아웃이 걸린다, 보내도 응답이 오지 않는다면

이것은 네트워크에 문제가 있는 것이라고 판단을 하고, 

congestion 메커니즘을 수행 하겠다는 것입니다.

end-to-end 방식은 대표적인 것이 `TCP`입니다.

### network-assisted 방식

여러 개의 소스가 있고 중간에 라우터를 거치게 되는데

이 라우터가 호스트들한테 `지금 혼잡이 일어나고 있으니까 얼마로 줄이세요`라고 

라우터가 정보를 알려 주는 것이 `network-assisted 방식` 입니다.

이렇게 하려면 사실 여러 가지 복잡한 문제가 있기 때문에 TCP에서는 end-to-end 방식을 씁니다.

network-assisted 방식은 대표적인 것이 `ATM`입니다.

TCP 헤더에 flag 중에 하나를 사용 하면 `Explicited Congestion Notification`이라고 해서

중간의 라우터가 지금 congestion이 어느 정도 발생하고 있다는 것을 알려 줄 수는 있습니다.

---
