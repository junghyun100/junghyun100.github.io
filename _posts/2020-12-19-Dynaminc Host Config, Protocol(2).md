---
layout: post
title: "네트워크 - Dynaminc Host Config, Protocol(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

DHCP가 실제로 이루어지는 과정을 한 번 살펴 봅니다.

# DHCP example

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1219/DHCP%20example.PNG">

맨 처음에 클라이언트가 도착을 하게 되면,

`DHCP discover` 메시지를 처음 보내게 됩니다.

<strong>DHCP</strong>는 응용 레벨에 있는 프로토콜로써 

실제 전송 프로토콜로는 `UDP`를 사용하고 네트워크를 통해서 전송을 하게 됩니다. 

이렇게 전송 된 메시지가 `DHCP 서버`에 도착 하게 되면

그것이 헤더가 다 깨지면서 DHCP 응용층으로 넘어가게 되는 것 입니다.

거기에 대한 응답으로 서버는 `DHCP offer` 메시지를 보내게 됩니다.

# DHCP example(cont'd)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1219/DHCP%20example2.PNG">

이 반대의 과정으로도 왔다 갔다 하면서 

`discover`, `offer`, `request`, `ack` 메시지가 전송되면서 주소를 할당 받게 됩니다.

`DHCP 서버`는 다시 클라이언트한테 응답 할 때 

클라이언트가 사용 할 `IP address(DHCP 프로토콜 응답)`과 

두 가지를 더 줍니다. 

무엇이냐 하면 우리가 이것을 이해 하기 위해서 서브넷 마스크 쪽을 다시 봅니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1219/Subnet%20Mask.PNG">

여기서 보면 컴퓨터에 인터넷을 설정 할 때 IP 주소를 부여하고 

`기본 게이트웨이`와 다음에 사용 할 `DNS 서버 주소`를 함께 줍니다.

> 게이트웨이 : 컴퓨터가 인터넷에 접속 할 때 맨 처음 거치게 되는 라우터

이 두 가지가 함께 부여 되어야 실제로 인터넷에 접속 가능하기 때문에 

`DHCP 프로토콜`도 <strong>클라이언트가 사용 할 첫 번째 정보</strong>입니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1219/DHCP%20example2.PNG">

두 번째인 `IP address of first-hop router for client`라고 되어 있습니다.

이게 <strong>기본 게이트웨이</strong> 입니다.

클라이언트가 접속 해 있는 첫 번째 라우터의 주소를 주어야 

클라이언트는 외부로 전달 할 메시지를 이 라우터한테 전달 할 수 있기 때문입니다.

세 번째인 이 네트워크 내에 있는 <strong>로컬 DNS 서버의 주소</strong> 입니다.

이것으로 인터넷에 접속 하기 위해 DNS 서버에 데이터를 보낼 수 있기 때문입니다.

그렇게 해서 DHCP 서버의 응답을 받게 되면 

클라이언트는 모든 인터넷이 설정이 완료 되어,

자신의 `IP 주소`와 `DNS 서버의 이름과 주소`, `first-hop router의 IP address`를 

함께 가질 수 있게 됩니다.

# Advantages of DHCP

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1219/Advantages%20of%20DHCP.PNG">

DHCP 프로토콜을 사용하게 되면 어떤 점이 좋은가를 한 번 더 정리 해봅니다.

인터넷 주소는 기존에 IPv4, 32 bit의 인터넷 주소는 급격하게 고갈되고 있습니다.

인터넷에서 얻어 온 자료를 보면 2000년부터 2011년 까지의 전체 인터넷 주소 중에 

남아 있는 양을 뜻하는데

2000년도에는 그래도 32 bit 주소 중에 38% 정도는 여유가 있습니다.

인터넷에 접속하는 기기들의 수가 급격히 증가하면서 2011년에 1% 대로 떨어졌고,

이렇게 주소가 급격히 감소 하는 상황에서 

모든 컴퓨터들이 고정 IP, 자기만 사용 할 수 있는 IP를 독점적으로 사용하게 되면 

현재 인터넷에 접속하고 있지 않으면서도 주소를 자기가 계속 가지고 있는 것이기 때문에 

효율적인 사용이 어렵습니다.

그런데 DHCP를 씀으로써 적은 수의 인터넷 주소를 가지고도

많은 수의 컴퓨터를 지원 할 수 있게 되기 때문에 <strong>IP 주소의 효율적인 사용</strong>이 가능 해 집니다.

또 다른 장점을 생각 하면 DHCP 덕분에 <strong>plug & play가 가능</strong>한 점 입니다.

plug & play 라는 것은 실제로 컴퓨터를 꽂기만 하면

전원을 꽂거나 랜 선에 꽂기만 하면 컴퓨터를 사용 할 수 있게 됩니다. 

자동으로 IP 주소 받기, 자동으로 DNS 서버 주소 받기를 설정 한 후 컴퓨터를 켜기만 하면,

와이파이 엑세스 포인트나 랜 케이블 코드에 꽂기만 하면 자동으로 IP 주소를 받아오는 것 입니다.

무엇을 통해서? <strong>DHCP 프로토콜</strong>을 통해서 이 IP 주소와 DNS 서버 그리고 기본 게이트웨이 주소까지

# 결론

네트워크 자원을 효율적으로 사용하면서, 일반 네트워크 지식이 없는 사람도 

쉽게 인터넷을 사용 할 수 있게 해 주는 것이 DHCP의 아주 큰 역할이 되겠습니다.


---
