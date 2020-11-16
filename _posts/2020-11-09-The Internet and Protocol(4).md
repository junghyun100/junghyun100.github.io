---
layout: post
title: "네트워크 - Brief Introduction to Internet Functions(4)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

이전 까지는 Content Provider Network이 무슨일을 하는가 까지 살펴봤습니다.

추가적으로 Traceroute에 대해서 알아본 후 Performance Metrics(성능 측정)에 대해 알아봅시다.

## Traceroute

누구도 인터넷이 어떻게 되어있다 전체 그림을 제공 할 수 있는 사람은 없습니다.

그럼에도 불구하고 우리는 가끔 내 데이터가 목적지까지 어떤 식으로 가는지 알아 볼 필요가 있습니다.

그런 목적으로 쓸 수 잇는 프로그램이 있는데 그게 traceroute 입니다.

<img src="https://i2.wp.com/ipwithease.com/wp-content/uploads/2018/01/152-how-traceroute-works-01.png?w=1277&ssl=1">

<a href="https://ipwithease.com/how-traceroute-works/">그림 출처</a>

Traceroute는 어떤 식으로 동작 하냐 하면 source가 traceroute 명령어를 날리면 이 명령어가 첫 번째 라우터에 갔다가 돌아옵니다.

돌아 올 때 해당 라우터의 이름과 갔다 오는 데 까지 걸린 시간이 얼마나 됐는지 알려 주게 됩니다.

그리고 첫 번 째 probe 메시지가 갔다가 돌아 오고 그리고 나면

두 번째 probe 메시지가 1 hop, 2 hop 갔다가 돌아오고 세 번째 probe 메시지는 1 hop, 2 hop, 3 hop 갔다가 돌아오고,

그래서 돌아 올 때 마다 어떤 라우터에서 돌아 왔는지 그 라우터에 대한 정보를 가지고 옵니다.

각각의 probe 메시지는 홉의 크기만큼 전달을 해서 그렇게 하는 이유는

<strong>갔다가 오는 시간을 측정 할 때 평균 값을 정확하게 내기 위해서</strong> 입니다.

# Performance Metrics

우리가 네트워크의 성능을 평가하기 위한 성능 지표들이 어떤 것들이 있는가 살펴 보겠습니다.

어떤 네트워크를 평가 할 때 보는 주요한 성능 지표가 세 가지 입니다. 

### Delay(지연시간)

지연시간은 데이터를 발생 시키는 소스(source)로 부터 목적지까지의 패킷을 전달하는 데 걸리는 시간이 평균적으로 얼마나 되느냐,

당연히 딜레이가 짧은 네트워크가 좋은 네트워크입니다.

### Packet Loss(손실률)

패킷을 쪼개서 보내는데 보낸 패킷 중에 얼마 정도가 분실이 되는가 라는 것 입니다.

비슷하게 어떤 성과를 평가할 때 있어서 packet loss를 비교하기도 하지만 

그와 반대로 `PDR(Packet Delivery Ratio)`라는 것을 평가하기도 합니다. 

전체 전달한 패킷 중에 얼마만큼이 성공적으로 전달됐냐 하는 것을 보기 때문에

packet loss rate과 반대의 개념입니다.

### Throughput(전송률)

전송률은 단위 시간 동안에 전달 될 수 있는 트래픽의 총 량을 뜻합니다.


## Four Source of Delay

전체 딜레이는 네 가지로 구성되어 있다고 볼 수 있습니다.

### 첫째, processing delay 

패킷이 도달 했을 때 라우터에서 패킷을 처리하기 위한 주소가 무엇이며 이 주소에 따라서 어디로 내보내야 하는가의 성능에 대한 딜레이

캐시에 도착 했을 때

비트 에러가 포함이 되어 있느냐 라는 것을 결정 해야하고 목적지 주소를 보고 아웃풋 링크를 결정해야 하는 일들이 일어납니다.

그런데 요즘에 굉장히 하드웨어 속도가 빨라졌기 때문에 이런 작업은 매우 빠른 속도로 이루어질 수 있고 소프트웨어적이 아니라 하드웨어적으로 처리를 하게 되면

굉장히 빠른 속도로, 하나하나의 패킷을 처리하는 속도는 무시할 수 있을 정도의 값이기 때문에 그렇게 중요하게 보지 않습니다.

### 둘째, queueing delay

어떤 패킷이 도달 했을 때 이전에 미리 도착한 패킷이 있었다면 큐(queue)에 들어가서 대기시키는 것에 대한 딜레이

중간 라우터에 도착해서 보내지기 까지 거기서부터 다음 hop으로 전달 되기 까지 

버퍼에서 얼마나 기다리는가 하는 것이 Queueing delay입니다.

그런데 라우터가 겪는, 라우터에 얼마나 많은 데이터가 도달해 있는가 라는 것에 의존적입니다.

사용자의 행태 하고도 관련이 있기 때문에 정확하게 얼마 만큼이 걸릴 것이다 라는 것이 보장이 안됩니다.

그래서 좀 많이 차이가 나고, 어떤 때는 굉장히 큰 포션을 차지하기 때문에 

이것은 상당히 중요하게 다루고 있습니다.

### 셋째, Transmission delay

링크의 전송률에 따라서 패킷 사이즈가 굉장이 크다면 걸리는 시간에 대한 딜레이

Transmission delay도 이것은 아웃풋 링크의 밴드 위스(bps)에 따라 다릅니다.

패킷의 length와 링크의 밴드 위스(bps), 링크의 capacity에 따라서 계산이 될 수 있습니다.

### 넷째, propagation delay

전파 시간, 전파 딜레이입니다.

propagation이라는 것은 ‘전파 속도’ 라는 것이죠. Transemission delay는 전송 속도라고 합니다.

---