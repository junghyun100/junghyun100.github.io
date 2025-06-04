---
layout: post
title: "네트워크 - Network Address Translation"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Network Address Translation

20년 전에는 사람들이 집집마다 컴퓨터를 한 대 정도씩 가지고 있었습니다.

인터넷 서비스 제공 업자들한테 전화를 해서 가입을 하면 주소를 하나씩 할당 받았고,

고정 IP 주소를 받을 수도 있고 아니면 `DHCP`를 통해서 주소를 받아서 

사용 할 수도 있었는데 그 이후에 집집마다 컴퓨터가 더 늘어나게 되었습니다.

2000년대 후반 들어오면서 가족 구성원들이 전부 다 스마트폰을 갖는 경우와

여러대의 컴퓨터를 사용하면서 그것들도 IP 주소가 필요해 졌습니다.

그래서 인터넷 회사 입장에서는 너무 많은 인터넷 주소 pool이 쓰이게 되었습니다.

이런 문제를 해결하고자 해서 나온 것이 <strong>Network Address Translation</strong>입니다.

## 개념 예

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1220/NAT.PNG">

집에 있는 <strong>각각의 컴퓨터들은 구별</strong>이 되어야 됩니다.

IP 주소가 구별 되지 않고 동일한 IP를 쓰면 서로 충돌이 나서 

수신 받아야 될 데이터가 어디로 받아야 될지 알 수가 없게 됩니다.

그런데 사람들이 생각하기에 집 안에 있는 컴퓨터들이 

`"외부에 있는 다른 컴퓨터들과 구별이 되어야 될까?"` 라고 생각하게 되었습니다.

이 컴퓨터들은 공유기(하나의 라우터)를 거쳐서 나가니까 

`"공유기 안 쪽에 있는 컴퓨터들은 이 안쪽에서만 서로 구별되면 되지 않을까?"`라고 

그래서 결국 인터넷 공유기를 통하면 안쪽에서만 서로 구별 되는 

어떤 IP주소, ID를 가지면 되지 않겠느냐 생각을 한 것입니다.

이것을 위해 ICANN에서 인터넷 주소에 좀 특별한 영역을 만들었습니다.

# NAT example

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1220/NAT%20example.PNG">

`A class`에 10으로 시작 하는 `10.x.x.x`

`B class`의 `172.16.x.x ~ 172.31.x.x` 네트워크까지 범위

`C class`의 `192.168.0.x ~ 192.168.255.x` 네트워크까지 범위. 

이것들을 <strong>Private IP</strong> 주소라고 따로 설정 했습니다.

> Public IP : 외부에 알려진 공용 IP, 공식적인 IP 입니다.

> Private IP : 내부에서만 쓰는 비공식적인 IP입니다. 

이러한 주소들은 내부에서만 의미가 있습니다.

이 주소들은 외부 라우터들에서는 취급을 하지 않기 때문에 

<strong>Private IP</strong>를 목적지로 발송을 하게 돼도 전혀 도착을 할 수가 없습니다.

<strong>NAT</strong>는 즉, 공유기 안쪽에 있는 여러 대의 컴퓨터 `192.168.0`으로 시작하는

사설 IP를 사용하면서 구별은 하되, 외부로 나갈 때는 `138.76.29.7` 이라는 

`Internet Service Provider`한테 받은 이 공식적인 IP 주소를 가지고 

외부로 전송을 하게끔 만들자는 것 입니다.

# NAT example (cont'd)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1220/NAT%20example%20cont'd.PNG">

어떤 컴퓨터가 `192.168.0.2`라는 사설 IP를 가지고 있습니다.

사용자의 집의 공유기 또는 라우터가 ISP로부터 `138.76.29.7`이라는 `public IP`를 할당 받았습니다.

그러면 `192.168.0.2`라는 컴퓨터가 웹 브라우저를 켜서 

외부에 있는 `128.119.40.186` 서버를 접속 하고 싶은 상황입니다. 

> destination port가 80라는 것을 보고 웹 서버에 접속하고 싶다는 것을 알 수 있습니다.

그러면 `HTTP request message`를 만들어서 전송 할 것입니다.

source 주소는 `자신의 IP`로 되어 있고 destination은 `128.119.40.186`으로 되어 있습니다.

그렇게 해서 request message를 만들어서 공유기, 첫 번째 라우터에 전송을 합니다.

공유기는 사설 아이피를 내보내기 전에 <strong>network translation table</strong>를 만들어서 

주소를 기록 해 두는 것입니다. 

`192.18.0.2` 내부에서 사용하는 이 사설 IP 주소와 `3345`라는 포트 번호를 가진 이 프로세스는 

외부로 보낼 때 번역을 해서 공식 IP인 `138.76.29.7`이라는 IP를 담고

`5001`이라는 포트 번호를 부여 해서 밖으로 내보낸다는 것 입니다.

이렇게 해서 테이블을 만들어 놓고 이 컴퓨터가 보낸 이 IP 데이터그램을 앞에 헤더를 바꿔서 

목적지는 그대로 둔 채로 source IP를 `138.76.29.7`와  `5001`이라는 

임의로 결정한 이 포트 번호를 부여 해서 밖으로 보냅니다.

그렇게 되면 외부에 있는 서버는 이 메시지를 받고 응답을 하게 될 때 

`138.76.29.7`, `5001` 측에서 request message를 받았기 때문에 거기에 응답을 보내게 됩니다.

응답이 공유기에 돌아 오면, 공유기는 다시 내부에서 전송 해 줄 때는 

다시 이 부분을 목적지 주소를 번역 해서 `192.168.0.2` 그리고 `3345` 포트로 전송을 해 주게 됩니다.

이것을 우리가 <strong>network address translation</strong> 기술이라고 이야기 합니다.

# NAT Address Traversal Problem

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1220/NAT%20Address%20Traversal%20Problem.PNG">

network address translation을 쓸 때 <strong>한 가지 제약</strong>이 있습니다.

대부분의 사람들 같은 경우에는 집 안에서 어떤 웹 호스팅을 한다기 보다는 서버에 접속 해서

데이터를 소비하는 그런 경우가 많습니다. 

그런 경우는 클라이언트 쪽에서, `local area network` 쪽에서 먼저 request를 보내기 때문에 

외부의 서버가 그것에 응답하는 것은 문제가 없었습니다.

그런데 이런 경우도 있습니다. <strong>어떤 사용자가 집 안에 웹 호스팅 서버를 하나 두고 싶은 경우</strong> 입니다.

외부에 다른 클라이언트가 이것에 어떻게 접속 할 수 있겠냐는 경우입니다.

왜냐하면 Local area network 내에서 서버의 주소가 `192.168.0.15` 라고 부여를 해 놓았기 때문에 

외부 클라이언트는 이 주소에 접속 할 수가 없다는 것이 문제입니다.

가장 간단하게 해결하는 방법은 공유기에 있는 <strong>Port Forwarding</strong>이라는 기능을 사용 하는 것입니다.

# Port Forwarding

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1220/Port%20forwarding.PNG">

Port Forwarding은 로컬 쪽에서 먼저 request를 보내지 않아도 외부에 있는 클라이언트의 응답,

먼저 받아서 안쪽에 있는 서버에 전달 할 수 있도록 포트를 미리 설정 해 두는 것 입니다.

`138.76.29.7`이 네트워크가 가지는 공식적인 IP라고 하면

`138.76.29.7` 포트 `80`로 오면 `192.168.0.15` IP를 가지는 서버의 `80`번 포트로 전달 하라는 뜻입니다.

그래서 이렇게 간단하게 network address translation problem을 

비켜 갈 수 있는 그런 방법이 제공되고 있습니다.

---
