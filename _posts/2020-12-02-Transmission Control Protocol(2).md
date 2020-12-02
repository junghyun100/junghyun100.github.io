---
layout: post
title: "네트워크 - Transmission Control Protocol(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

## Usage of Seq & Ack Numbers

<img src="">

Sequence number와 ack number의 사용 방식을 다시 한 번 살펴 봅니다.

host A와 host B가 이전에 어떤 연결을 수립 했고, 유저가 C라는 데이터를 하나 보낸 것입니다.

그리고 이 sequence number는 42로 시작합니다.

Ack의 79는 이전에 C를 보내기 이전에 host B가 보낸 것이 있었다고 예측할 수 있습니다.

Host A에서 C를 보냈고, host B는 그것을 받았기에 이에 대한 Ack를 보내야합니다.

ACK에는 43 들어가 있습니다.

이것은 host A는 42번으로 시작 되는 데이터를 보냈는데 이게 1 byte이기 때문에,

host B는 `다음에 받아야 될 데이터가 43이다` 라고 알려 주는 것 입니다.

이 응답 메세지의 데이터 C는 host B가 A가 보낸 것을 다시 반복 하는 응용이라고, 

가정한 상황이기 때문에 데이터 C를 여기에 담아 둔 것이고,

따로 보낼 데이터가 있다면 데이터를 담으면 됩니다.

Host B가 메아리를 보낸 것이 79번으로 Ack가 왔으니 Host A는 80번이라는 번호로 ACK를 합니다.

`79번째 byte 까지는 잘 받았고, 80번째 byte를 받아야 된다` 하는 것을 

host A가 B 한테 말 해 주고 있는 것 입니다.

## TCP Reliable Data Transfer

TCP는 IP와 여러가지 네트워크 인터페이스를 기반으로 합니다.

이 부분은 사실 unreliable 한 상태로 에러가 자주 발생 합니다. 

그렇기 때문에 에러가 애플리케이션에 보이지 않도록 TCP가 잘 에러를 숨기고

서비스를 하기 위해서 TCP는 아래와 같은 요소들을 가지고 있습니다.

* pipelined segments 

segment들을 하나씩이 아닌 pipeline 방식으로 데이터를 전달

Throughput을 위한, 퍼포먼스를 위한 것

* cumulative acks

데이터 별로, 패킷 별로, segment 별로 하나 하나 보내는 것이 아니라

어떤 ACK를 보내면 그 이전에 보낸 패킷들에 대해서는, 한꺼번에 acknowledgment를 하는 기능

* single retransmission timer

cumulative acks 사용으로, 모든 segment에 대해서 별도의 타이머를 두지 않기에

single retransmission timer를 이용해 서비스 제공

### Go-back-N과 차이점

go-back-N하고 닮아 있다는 것을 보실 수 있는데 사실 앞의 go-back-N하고 다른 점은 있습니다.

go-back-N에서는 순서에 맞지 않는 데이터가 오면, 

예를 들어 패킷 2번을 받지 못했을 때, 이후의 패킷들이 오더라도 전부 버립니다. 

그런데 TCP는 버리지는 않습니다.

TCP에서는 cumulative ack를 쓰고 single retransmission timer를 쓰지만

sequence number에 맞지 않는 이후의 것이 오더라도 

일단 자기의 receive buffer가 허용 하는 수준 내에서 그것을 보관을 해 둡니다

TCP 연결의 수신 버퍼는 congestion window가 작고 receive 버퍼가 항상 큽니다.

그렇기에 보내는 데이터는 잘 도달하기만 하면 일단 보관은 할 수 있는 것입니다.

### Go-back-N과 Selective repeat의 절충

순서에 맞지 않게 도달 한 패킷을 보관한다는 의미에서는 selective repeat과 조금 유사하지만,

cumulative ack나 transmission timer 하나만 사용 한다는 것은 go-back-N하고 비슷합니다.

그렇기에 두 가지가 좀 절충 된 그런 형태입니다.

## TCP Flow Control

<img src="">

Flow control은 한 마디로 말씀 드리면 Receiver 버퍼가 있는데 그 버퍼 크기를 넘어 서는 데이터 양을 한꺼번에 보내는 것을 막기 위한 기능입니다. 

어떤 호스트가 자기하고 연결 된 다른 호스트한테 패킷을 전달 할 때 수신 버퍼에서 남아 있는 크기를 receive window에 담아서 보냅니다.

그러면 Sender는 receive window 크기를 넘어 서지 않는 크기를 데이터 제약을 해서 데이터를 보내게 됩니다.

수용 할 수 있는 크기 이상이 오게 되면 데이터가 버려지게 되기 때문에 데이터를 저장 할 수 없기 때문에 이런 식의 방식을 사용합니다.

많은 소켓 프로그램에서 보면 reciver 버퍼 사이즈를 사용자가 지정 할 수 있는데 보통 4096 byte, 4 kB 정도의 크기로 할당을 합니다.

이것은 보통 congestion window에서 제약하는 것 보다는 충분한 양이기 때문에

congestion에 의해서 sliding window로 보내는 window size가 제약을 받는다 이렇게 되어 있습니다.

## Connection Management

<img src ="">

flag에는 어떤 경우에 9가지의 flag가 있지만, 예시에는 6가지가 있습니다. (이유는 3가지는 거의 사용되지 않아서)

URG, ACK, Push, Push, Reset, synchronization, Finale로 각각이 다 한 bit씩 차지 하고 있습니다.

이 중에서 ACK는 acknowlegement number에 실제로 유용한 값이 들어 있는지 들어 있지 않은 지를 나타내기 위한 것이 ACK 필드입니다.

실제로 Flag에서 중요 한 것은 `synchronization`, `final` 그리고 `reset` 세 가지 인데,

이 부분은 TCP connection을 설립 하고 해제 하고 이런 데 필요한 내용입니다.



---
