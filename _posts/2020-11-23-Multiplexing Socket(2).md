---
layout: post
title: "네트워크 - Multiplexing Socket(2)"
tags: [컴퓨터 네트워크]
comments: true

---


컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

## Socket Program

소켓은 API의 형태로 `OS`에 의해서 제공이 됩니다.

특별히 트랜스포트 레이어에 있는 프로토콜을 구현한다거나 할 필요 없이 

소켓의 함수들을 잘 다루기만 하면 데이터를 보낼 수 있습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1123/Socket.PNG">

위의 것은 서버이고 밑의 것은 클라이언트인 단순한 형태의 소켓 프로그램입니다.

파이썬으로 구현 된 소켓 프로그램입니다. 

먼저, 소켓을 사용하기 위해서는 소켓 모듈을 import 해야합니다.

서버 쪽에서는 자기의 프로세스를 만들고, 소켓에다가 이러한 주소를 스스로 `할당(bind)` 해 놓은 것입니다. 

예제는 컴퓨터에서 로컬하게 돌아가는 서버와 클라이언트이기 때문에

주소를 `127.0.0.1(본인 컴퓨터)` 이라고 할당 되어있습니다.

while문 내부를 보면 `accept`라는 것이 있고 `receive`라는 것이 있습니다.

이것들의 역할이 뭐냐면 클라이언트에서 똑같이 커넥트, 연결을 설립 하는 것입니다.

### ACCEPT

TCP 같은 경우에는 커넥션을 먼저 설립하고 데이터를 보내기 때문에 커넥트 라는 것을 통해서 서버에 연결을 합니다.

커넥트 하고 상대방 주소, 여기서 제가 서버나 클라이언트를 로컬 호스트에서 동작하게 했기 때문에

상대방 인터넷 주소도 로컬 호스트인 127.0.0.1이 되는 것입니다. 

그리고 옆에 있는 `8089`는 localhost라는 주소를 가진 컴퓨터에서 돌고 있는 프로세스 중에서

포트번호 `8089`라는 번호를 가진 프로세스와 연결을 하겠다는 뜻입니다.

인터넷 주소와 포트 번호를 주고 커넥트 명령을 수행 하면 

해당 위치를 찾아 가서 `connection request`라는 메시지를 보내게 됩니다.

그러면 서버는 동작하다가 클라이언트의 요청에 도달 하면 그것을 `accept` 합니다.

`Accept`를 할 때는 주소와 포트 번호가 일치 해야 하고, 일치한다면 연결이 수립됩니다.

### RECEIVE

연결을 수립하고 나면 클라이언트는 데이터를 `send`, 보낼 수가 있습니다. 

클라이언트 측을 보시면 Get / index.html 로 작성이 되어있는데 send해서 이것을 보내게 되면,

수신자는 이것을 받아서 처리를 할 수 있습니다.

GET INDEX이기 때문에 뒤에 사이트 정보를 보내 주게 될 것입니다.

### 요약 

1. 소켓을 서버 쪽에서 생성 하고, 

2. 클라이언트가 보내 온 연결 요청을 accept 하여 연결

3. 클라이언트가 보내 온 데이터를 receive해서 적절한 조치

4. 응답을 보냄으로써 간단하게 연결을 해서 통신

## Demultiplexing Example

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1123/Demultiplex%20Example.PNG">

보내는 쪽에서 포트 번호를 달리 해서 데이터를 보내게 되면

받는 쪽에서는 포트 번호에 따라서 트랜스포트 레이어가 그것을 구별 할 수 있게 됩니다.

통신하는 형태를 보시면 중간이 서버, 양 쪽이 클라이언트인데 왼쪽에 있는 P3가 하나의 클라이언트 프로그램 입니다.

왼쪽의 IP 주소는 `A`이고, 서버가 가지는 IP 주소는 `B`이고, 오른쪽 클라이언트가 가지는 IP 주소는 `C`입니다.

먼저 그러면 왼쪽의 클라이언트가 데이터를 보내게 되는데,

`A` 주소가 담겨져 있고, P3가 갖고 있는 포트 넘버는 `9157`이라는 번호를 가지고 전송을 한 것입니다.

그러면 이제 목적지 주소는 `B`이고, 포트 번호는 `80`입니다. 

그러면 `B` 컴퓨터에 `80`번 포트를 찾아서 이렇게 전송을 하게 됩니다.

그렇게 되면 P4는 여기에 응답을 보내 주게 되고 

이 때 response는 거꾸로 소스 주소와 포트 번호는 `B`의 `80`이 되는 것이고, 받는 쪽인 `A`의 `9157`이 되는 것입니다.

이번에는 오른쪽 클라이언트를 살펴봅니다.

P2 같은 경우에는 보내는 컴퓨터의 IP 주소는 `C` 이고 포트 번호는 `5775`다. P2가 그런 `5775` 라는 포트 번호를 갖습니다.

통신하고자 하는 대상은 서버에서 동작하고 `8089` 포트 번호로 동작하고 있는 프로세스(P6)입니다.

이제 P6에서 응답이 옵니다. 

### 동일한 포트 번호를 가질 수 있는가?

우리가 보통 포트 번호를 가지고 프로세스를 구별한다고 하는데 

그러려면 사실 동일한 포트 번호를 가지는 프로세스가 없어야 할 것 같습니다.

그런데 꼭 그렇지는 않습니다.

그림을 보면 오른쪽의 P3 프로세스가 데이터를 보냅니다.

보내는 쪽의 IP 주소는 `C`이고 포트 번호는 `9157`이라는 것이고

받는 프로세스의 주소를 보니까 `B` 컴퓨터의 `80`번 포트라는 것입니다. 

그래서 P5와 연결이 되어있습니다.

왼쪽의 P3 통신하는 프로세스도 포트 번호가 `80`번이라고 했습니다.

결국 여기서 보시면 `B`컴퓨터의 P4와 P5가 둘 다 `80`번 포트를 갖는다는 것을 알 수 있습니다.

이게 `connection-oriented`. `TCP` 같은 `connection-oriented 프로토콜`을 사용 할 때 나타나는 현상입니다.

이 `80`번 포트는 웹 서버가 사용하는 포트입니다.

웹 서버는 하나의 클라이언트가 접속 할 때 마다 똑같은 프로세스를 여러 개 띄우게 됩니다.

A라는 클라이언트에게 제공 할 서비스를 하기 위한 프로세스, 그것이 P4 인 것 입니다.

그리고 여기 클라이언트 C에 서비스 하기 위한 프로세스는 P5이고,

이런 것을 하나 하나 다시 띄우게 됩니다.

완전히 동일한 프로세스를 띄우다 보니까 포트 번호도 똑같이 `80`번이 됩니다.

트랜스포트 레이어가 양쪽의 패킷을 동시에 받았다고 하면 둘의 IP 주소와 포트 번호가 동일 하다는 것을 알 수 있습니다.

그러면 이것을 어떻게 구별하느냐, 이런 경우는 `connection-oriented 프로토콜`에서만 나타납니다.

connection-oriented 프로토콜은 데이터를 보내기 전에 커넥션을 먼저 설정하기 때문에

커넥션에는 수신자의 IP나 포트 번호 외에도 송신자의 IP와 포트 번호도 구별하는 인자로 사용이 되는 것입니다.
 
결론적으로 demultiplexing을 할 때는 엄격히 말하면 포트 번호 만으로 되는 것은 아닙니다.

---
