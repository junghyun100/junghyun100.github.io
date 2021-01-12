---
layout: post
title: "네트워크 - Link Layer Basics Protocol(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Link Layer Services

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0112/Link%20Layer%20Services.PNG">

링크 레이어에서 이루어지는 서비스를 

좀 구체적으로 나누어 보면 

### 1. Framing, link access

<strong>Framing</strong> 이라는 것은 `프레임(frame)`을 만든다는 겁니다.

`프레임`은 `2계층`에서 사용하는 데이터 패킷으로

<strong>Framing</strong>을 한다는 것은 

`3계층`에 있는 `데이터그램`을 전달 받아서

데이터그램의 앞 부분과 뒷 부분에 

링크 레이어 기술에서 필요 한 정보를 담음으로써 만듭니다.

그 다음에 보통 하나의 컴퓨터는 

`여러 대와 동시에 연결` 될 수 있습니다.

그래서 하나의 호스트와 다른 호스트들이 

서로 다 대 다 연결을 할 수 있는 상황이 되는거고 

이럴 때는 `채널을 서로 공유`하게 됩니다.

그런 경우에 

<strong>"누가 지금 전송을 할 것인가?, 누구는 기다릴 것이냐?"</strong>를 

결정 해 주는 기술이 필요합니다.

이러한 기술을 <strong>channel access</strong>기술이라고 합니다.

그 다음에 링크 계층에서도 

실제 데이터를 전달 하기 위해서는 

보내는 사람과 받는 사람을 

명확하게 기술 할 수 있는 방법이 있어야 합니다.

`IP`도 `destination 주소`와 보내는 `source 주소`가 있었던 것 처럼

링크 계층에서도 `MAC address` 라고 부르는 주소가 있어서

각 기계마다 주소를 부여 받고 

이 주소를 가지고 데이터를 주고 받는데 사용을 하게 됩니다.

### Flow control

<strong>Flow control</strong>은 `TCP의 flow control` 하고 비슷합니다.

`TCP Flow control`은 

처음에 보내는 `source 노드`와 `destination 노드` 사이에 

전송 속도를 조절하는 것이었습니다.

여기서 다른점은 `TCP 연결`은 원거리에 `원격`으로 있을 수 있기 때문에 

둘 사이가 하나의 링크로 되어 있는 것은 아닙니다.

`링크 레이어에서의 Flow control`은 

<strong>물리적으로 연결 된 두 노드</strong>가 

너무 서로 빠르게 보내지 않도록 데이터를 보내는 `sending machine`가

`전송속도를 조절하는 것`이 <strong>flow control</strong>입니다.

### Error detection

링크를 통해서 데이터를 보낼 때 비트 에러가 발생 할 수 있는데

여기에 `에러가 있는지 없는지를 검사 할 수 있는 기술`이 있어야 됩니다.

에러 감지만 가능하면 어떻게 처리 하는가는 

각 링크 레이어 기술이나 룰에 달려있습니다.

### Error correction

어떤 링크 레이어 기술 같은 경우에는 

`detection 감지` 뿐만 아니라 

에러가 난 상황을 보고 에러 패킷의 형태를 보고 

`패킷을 복원하는 기술`도 링크 레이어에서는 제공이 되고 있습니다.

# Link Communication

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0112/Link%20Communication.PNG">

`sending side`과 `receiving side`로 나누어서 살펴봅니다.

## Sending side

데이터그램을 프레임에 담고 거기다 헤더와 트레일러를 붙이는

<strong>framing 기술</strong>을 제공합니다.

그 다음에 `error detection`이나 `error correction`을 위한 

<strong>부가 정보를 붙인다던지</strong>,

아니면 `receiver`가 요구하는 전송 속도를 지켜 준다던지와 같은

<strong>error control, flow control 기술</strong>을 제공을 해야합니다.

## Receiving side

`Sending side`에서 보낼 때 담겨진 부가 정보를 이용해서 

받은 데이터에 <strong>에러가 있는지 없는지를 검사</strong>를 할 수 있어야 되고,

`source`가 보내는 쪽에서 너무 빨리 보낸다면 

속도 조절을 위한 <strong>flow control 기술</strong>도 있어야 됩니다.

이렇게 `sending side`에서 보낸 `패킷 프레임`을 받아서 

데이터그램을 뽑아내어 <strong>상위 계층에 올려주는 기술</strong>이 있어야 됩니다.

---
