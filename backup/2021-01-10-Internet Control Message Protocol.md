---
layout: post
title: "네트워크 - Internet Control Message Protocol"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# ICMP

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0110/ICMP(Internet%20Control%20Message%20Protocol).PNG">

데이터는 전달 되는데 있어서 

중간에 여러 호스트와 라우터들을 거치게 됩니다.

<strong>ICMP</strong>는 이런 호스트와 라우터들이 

<strong>에러 리포팅을 하기 위해서</strong> 사용 하는 겁니다.

그림에서 `호스트 A`가 계속 데이터는 보내는데 

어떠한 이유로 버려지는지 모르기 때문에 

데이터 전송이 어렵습니다.

그래서 이 중간의 라우터의 IP 프로토콜이 데이터를 버린다 하더라도

왜 버렸는지 그 이유는 알려 줘야 

`호스트 A`가 거기에 맞춰서 보낸 데이터를 

다시 수정을 해서 보내게 될 수 있습니다.

그렇기 때문에 `에러 메세지 프로토콜`을 만들어 둔 겁니다.

`ICMP 메시지`는 `IP 데이터그램` 위에서 동작합니다.

`IP 헤더`가 앞에 붙고 

뒤에 `ICMP 메시지`가 붙어서 `IP 형식`으로 전달이 됩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0110/ICMP(Internet%20Control%20Message%20Protocol)2.PNG">

`ICMP 메시지`는 `type`, `code`가 있고,

여기에 에러를 겪은 `IP 데이터그램`의 첫 번째 `8 byte`를 넣어서 

어떤 `IP`가 버려 졌는지 알려 주기 위한 내용과 `checksum 정보`가 들어갑니다.

오른쪽 표를 기준으로 예를 들면,

### type = 3 , code = 0

`type`이 `3`이고 `code`가 `0`이라고 하면 

해당하는 네트워크 ID를 가진 네트워크를 찾지 못하는 경우이고,

### type = 3 , code = 1

`type`이 `3`이고 `code`가 `1`이라고 하면 

해당하는 네트워크는 찾았지만 

`host ID` 까지 일치하는 호스트가 존재 하지 않는 경우입니다.

이런식으로 전달이 안 되는 경우가 어떤지 알려주게 됩니다.

<strong>ICMP</strong>라는 프로토콜은 `IPv4`에도 있었는데, 

`IPv6`가 나오면서 `ICMPv6`가 새로 만들어 졌습니다.

많은 것들이 비슷하고 몇 가지 타입이 추가가 되었는데 

하나가 `Packet too big`, 패킷이 너무 컸습니다.

`fragmentation`을 배울 때, 전달하려고 하는 패킷이 너무 크면

`fragmentation`이 기존에 일어났었습니다.

`IPv4`에서는 `fragmentation`과 목적지에 도착하면 

`reassembly`가 일어났었지만,

`IPv6`에 오면서 이런 것이 라우터에 오버헤드가 되는 것을 알고,

중간에 `fragmentation`과 `reassembly` 하지 않도록 바꿨습니다.

대신 `ICMPv6`에서 `packet too big` 이라는 `type`을 추가 함으로써

라우터가 전달 할 수 없을 만큼 너무 큰 패킷이 오면 버려 버리고

`source측`에 메시지를 전달 해 주면 

`source`는 전달하려고 하는 `fragmentation`이 크다는 것을 알게 되고,

그 다음부터 작게 보냄으로써 

중간에 `fragmentation`나 `reassembly` 없이도 

목적지에 잘 도달 할 수 있게 만듭니다.

---
