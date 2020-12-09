---
layout: post
title: "네트워크 - Inside of Router"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Router Architecture Overview

라우터는 모든 기능을 구성하는 요소들이 똑같지는 않지만 

전체적인 개요는 그림과 같이 구성 되어 있습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1209/Router%20Architecture%20Overview.PNG">

중간에 포워딩을 해 주는 `High-speed switching fabric` 장치가 있습니다.

시스템에 따라서 다른 방식으로 구현 되기도 합니다.

왼쪽 편에는 외부에서 라우터로 데이터가 들어 오는 `input port`가 있습니다.

그리고 오른 쪽에는 `output port`가 있습니다.

간략하게 말하자면 `input port로 들어온 데이터`가 

어느 `output port로 나가야 하는지 결정` 해 주는게 `switching fabric`의 역할입니다.

그리고 input으로 들어온 어떤 datagram이 output으로 나가는 데 

가장 핵심이 되는 정보는 IP 주소이고, 이런 정보를 얻기 위해서는 

라우팅 알고리즘이 필요 하게 됩니다.

포워딩의 역할을 담당하는 이 부분은 하드웨어로 많이 구성이 되어 있고, 

길을 설정 해 주는 라우팅 쪽은 소프트웨어 쪽으로 구현이 많이 되어 있습니다.

# Input Port Function

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1209/Input%20Port%20function.PNG">

Input port의 기능을 3가지로 나누어 보면 Physical Layer, Data Link Layer, Decentralized switching이 있습니다.

<strong>Physical Layer</strong>는 유선이나 무선 링크를 통해서 전달되어 오는 정보를 1인지 0인지 구별 해 주는 그런 역할을 합니다.

<strong>Data Link Layer</strong>는 1과 0로 구별이 된 데이터들을 이용해 에러 체크를 하는 등의 역할을 합니다.

> 데이터 링크 계층에서의 가장 대표적인 프로토콜이 이더넷(ethernet)입니다.

그 다음에 위로 전달 해 줄 때 IP 데이터그램 형태로 올려 주게 됩니다.

이렇게 만들어 낸 IP datagram은 항상 실시간적으로 처리 되지 않을 수도 있습니다. 

중간에 switching fabric이 처리해야 될 데이터가 많다거나 하면 데이터를 저장 해 둡니다.

이것을 `input queue`라고 합니다.

되도록이면 queue에 쌓이지 않도록 fabric이 처리 할 수 있도록 해야 됩니다.

# Input Port Queuing

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1209/Input%20Port%20Queuing.PNG">

`switch fabric`이 빨리 처리 하지 못한다면 어쩔 수 없이 `queuing`이 저장 되는 지연 시간을 좀 겪게 됩니다.

그런 일을 방지하기 위해서는 기본적으로 라우터를 설계 함에 있어서 중간의 switch fabric의 처리 속도가

모든 input port에서 들어오는 데이터를 합친 것 보다 빨라야 합니다.

* Queuing이 많이 일어나다 보면 딜레이 뿐만 아니라 데이터 loss나 buffer overflow 발생 가능 

### HOL blocking

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1209/Input%20Port%20Queuing.PNG">

input port에서 겪는 지연 중에 <strong>HOL blocking 지연</strong>이라고 부르는 것이 있습니다.

아래쪽 그림에서 보시면 각각의 `datagram`이 색 별로 구분 되어 있습니다.

색은 어느 output port로 나가야 하는가를 표현하고 있습니다.

여기서 `첫 번째 input port에 있는 첫 번째 데이터`와 `세 번째 input port에 있는 세 번째 데이터`가 

똑같이 빨간색으로 나가야 합니다.

이 두가지가 한꺼번에 나갈 수 없기 때문에 하나는 어쩔 수 없이 대기를 해야 합니다.

이 왼쪽 그림에서 가정하기를 이 위의 패킷을 먼저 내보내기로 했다면

세번째 input port의 초록색 datagram 입장에서는 어떠한 충돌도 없지만, 

앞에 있는 데이터 때문에 빠져나가지 못하게 됩니다. 

이런 것을 <strong>HOL blocking(Head-of-the-Line blocking)</strong>이라고 이야기 합니다.

---
