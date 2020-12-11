---
layout: post
title: "네트워크 - Inside of Router(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Output Port Functions

중간에 `switch fabric`을 거친 후의 데이터는 외부로 보내기 위해 다음과 같은 과정을 거칩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1211/Output%20port%20Functions.PNG">

데이터는 `output buffer`에 잠시 저장이 됩니다.

저장이 필요한 이유는 `output port`가 물리적으로 유선 링크나 무선 링크에 연결 되어 있는 상황에서

각 링크마다 처리할 수 있는 `datarate`보다 스위치의 처리 속도가 더 빠를 수 있습니다.

즉, 데이터를 이더넷 프레임으로 만들고, 다시 전기 신호로 바꾸어서 전달하는데 시간이 필요하기 때문입니다.

이러한 이유로 `output buffer` 쪽으로 버퍼링이 많이 일어나는 현상이 발생합니다.

> Buffering : 링크로 흘러가는 것 보다 fabric으로부터 도착 하는 datagram이 더 빨리 도착하면서 발생

버퍼링 되어 있는 데이터를 어떤 순서로 처리 할 것인지 결정하는 것이 <strong>scheduling</strong> 입니다.

# Output Port Queuing

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1211/Output%20port%20queuing.PNG">

`switch fabric`의 처리 속도가 `transmission rate`보다 높을 때 일어나고,

링크의 `전송 rate`가 너무 낮으면 결국은 `output buffer`에도 사이즈의 제한이 있기 때문에
 
데이터가 버려지게 되는 `output buffer overflow`가 발생 할 수 있는 우려가 있습니다.

# Scheduling Mechanisms

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1211/Scheduling%20Mechanisms.PNG">

`scheduling`은 다음에 링크로 흘려 보낼 패킷을 어떤 패킷을 흘려 보낼 것이냐를 결정 하는 것 입니다.

일반적으로는 보통 <strong>first in first out</strong> 방식, 먼저 버퍼에 도착 한, 

버퍼의 맨 앞에 있는 패킷을 흘려 보내는 `FIFO 방식`으로 사용합니다. 

그런데 이것이 모든 서비스들의 만족도를 높이는 데 적합하지 않을 수 있습니다.

응용 프로그램에서 지연 시간이 만족 되어야 되는, 

어떤 특정 지연시간을 만족해야 하는 멀티미디어 응용이 있는가 하면

지연 시간에 상관 없이 목적지까지 정확하게만 도착 하는 응용도 있었기 때문입니다.

FIFO 방식이 아닌 다른 방식의 scheduling들을 사람들이 제안을 했습니다.

아래의 그림을 잠시 살펴봅니다.

데이터가 queue에 빠른 속도로 쌓이면 결국은 버퍼에 다 저장 되지 못하고,

버퍼 `overflow`가 일어 나서 데이터 패킷 drop이 일어나게 됩니다.

여기서 어떤 패킷을 drop 시킬 것인지 결정하게 됩니다.

### Tail drop 

가장 단순한 방식입니다. 

버퍼가 가득 찰 때 까지, 저장 했다가 그 다음에 <strong>도착하는 패킷들은 버리는 방식</strong>입니다.

### Priority 

datagram마다 `priority(우선순위)`를 두어서 데이터가 도착 했는데 더 이상 저장 될 곳이 없다면,
 
있는 것들 중에 priority가 낮은, <strong>우선 순위가 낮은 것들을 버려 버리고 대신 저장 하는 방식</strong>입니다.

### Random

어느 일정 수준을 넘어 서게 되면 그 다음부터는 <strong>무작위로 drop 시켜 버리는 방식</strong>입니다.

그렇기 때문에 저장이 될 수도 있고 안 될 수도 있는 방식입니다.

# Priority Scheduling

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1211/Priority%20Scheduling.PNG">

First in first out 방식에서는 별도의 queue가 필요 없이, 하나의 queue에 집어 넣으면 됩니다.

왜냐하면 먼저 도착한 순서대로 처리 하면 되기 때문입니다.

그러나 이 방식은 각 `datagram`마다 `priority`를 부여 해서 우선 순위 마다 별도의 queue를 둔 다음에 

우선 순위가 높은 queue의 데이터가 없을 때에만 낮은 우선 순위 queue에는 데이터를 처리 해주는 방식입니다.

오른쪽의 아랫 그림을 보면, 위에 있는 이 숫자가 도착 하는 순서입니다.

`빨간 색`으로 되어 있는 것은 우선 순위가 높은 것이고, `초록 색`으로 된 것은 우선 순위가 낮은 것입니다. 

우선 순위가 높은 패킷들이 먼저 departures 쪽으로 도착 하는 것을 볼 수 있습니다.

# Round Robin(RR) Scheduling

Priority 방식을 사용하니 낮은 우선 순위에 있는 패킷들이 너무 처리를 잘 못 받는 단점이 있었습니다.

그 점을 해결하고자 `Round Robin`을 이용한 방식이 나타났습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1211/Round%20Robin.PNG">

앞서 그림과 동일한데 1번 패킷, 2번 패킷, 4번 패킷은 `첫 번째 queue`에 속하고, 

3번, 5번은 `두 번째 queue`에 속 합니다.

가장 먼저 도착한 1번을 받아서 처리 하고 있는 동안에 2, 3번이 도착 했습니다. 

2번이 `첫 번째 queue`에 들어가고 3번이 `두 번째 queue`에 들어갔습니다.

방금 `첫 번째 queue`에 있는 1번을 처리 했기 때문에, 

그 다음 `두 번째 queue`에 있는 3번을 처리 하게 되는 것이고,

3번을 처리 하고 나면, 2번 패킷을 처리 하게 됩니다. 

그런데 2번을 처리 하는 중에 4번 데이터그램이 `첫 번째 queue`에 도착 했습니다.

2번 패킷을 처리 한 다음에는 `두 번째 queue`에서 데이터를 가져 와서 처리 해야 하는데

데이터가 없기 때문에 다시 위로 가서 위에 있던 4번 패킷을 처리 하는 방식입니다.

어떤 것이 우선 순위가 더 높다 이렇게는 볼 수 없습니다. 

그렇지만 똑같은 순서대로 기회를 주기 때문에

만약에 어떤 특정 클래스가 <strong>좀 더 숫자가 적다면 더 빨리 처리 될 수 있는 장점</strong>이 있습니다.

---
