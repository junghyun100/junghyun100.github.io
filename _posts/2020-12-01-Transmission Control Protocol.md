---
layout: post
title: "네트워크 - Transmission Control Protocol"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# TCP Segment Structure

TCP가 여러 가지 기능들을 제공 하기 위해서는 정보가 있어야 합니다.

그 정보는 당연히 헤더에 들어 갑니다. 

<img src="">

윗 부분에 있는 것이 TCP segment header, 아래 부분이 UDP header 입니다.

UDP의 경우에는 header의 필드가 매우 간단했습니다.

Source port, destination port, length 그리고 checksum이 있습니다. 

TCP는 그것보다 훨씬 복잡합니다.

기본적으로 TCP는 20 byte, 한 줄에 4 byte로 다섯 줄이 있습니다.

UDP에 비해서 2.5배 정도 긴 헤더 길이를 갖고 있고,

필요한 경우에 옵션 헤더를 더 붙일 수 있게 되어 있습니다.

제일 첫 번째 word는 source port와 destination port.

Multiplexing, demultiplexing을 제공 하기 위한 필수요소 입니다.

이것은 UDP 하고 동일한 형태입니다. 

다음에 나오는 sequence number나 acknowledgement number입니다.

그 다음으로는 data offset이라는 것이 있습니다.

이것은 UDP length하고 좀 비슷한 기능을 합니다.

UDP length 같은 경우에는 헤더 길이를 포함 해서 전체 데이터, 뒷 부분에 데이터가 붙습니다.

즉, 헤더 더하기 데이터 길이를 이야기 하는 것입니다.

그런데 date offset은 어떤 그림에는 date offset이라고 되어 있고,

어떤 그림에는 header length라고 되어 있습니다.

그에 대한 이유로는 기본 헤더는 20 byte이고, 

뒤에 선택적인 정보가 더 붙을 수 있다는 점 때문입니다.

수신 측에 실제 데이터의 시작점을 알려 줄 필요성이 있었습니다.

그래서 date offset은 뒤의 옵션 헤더가 끝나고 나면,

뒤에 데이터가 어디서부터 시작 하는지를 알려 주는 것 입니다.

즉, UDP는 항상 8 byte이지만, TCP 같은 경우에는 옵션이 붙을 수 있어서

길이가 달라질 수 있기 때문에 date offset을 사용한다는 것입니다.

다음에 res는 reserve의 약자입니다. 

다음에 TCP의 담을 정보가 있다고 하면 거기에 담겠다 것 입니다.

현재는 아무런 정보를 담고 있지 않다는 뜻 입니다. 

Header checksum은 UDP와 동일한 방식의 checksum을 사용합니다.

urgent pointer 라는 것은 예를 들어 설명하겠습니다.

FTP로 파일 전송을 하는 중에, 착각 해서 다른 파일을 보냈다면?

파일 전송을 중단이 필요할 수 있겠습니다. 

Ctrl + C를 누르면 파일 전송이 중단 되게 되는데

그런 긴급한 명령이라던지, 전달 하기 위한 것이 Urgent data 입니다.

## Sequence and Acknowledgment Number

<img src="">

TCP나 UDP는 둘 다 segment라는 단위로 나누어서 데이터를 보내게 되는데 maximum segment size를 1000 byte로 했다. 

그러면 TCP 프로토콜은 응용에서 내려 준 데이터를 1000 byte 단위로 자릅니다.

잘라서 첫 번째 부분은 첫 번째 segment에 넣고, 두 번째 부분은 두 번째 segment에 넣어서 순서대로 전송을 하게 되는 것이죠.

나중에 데이터들이 segment들이 모두 목적지에 도달 했을 때 receiver는 다시 합쳐서 원래 메시지를 만들려고 하면

어떤 데이터 보다 더 앞선 것인지 또 뒤에 선 것인지 알아야 합니다. 

그래서 sequence number를 담는 것입니다.

sequence number는 항상 0 부터 시작 하는 것은 아니고 둘의 connection을 설정 할 때, 

두 연결의 당사자가 sequence number를 어떤 번호로부터 시작 하자 라는 것을 결정 해 둡니다.

그래서 그것을 만약에 n에서 시작 한다고 했다면 첫 번째 segment는 n이라는 sequence number를 갖는 것이고

두 번째 segment는 maximum segment size가 1000 byte이고 첫 번째 segment에서 1000 byte의 내용을 담았다고 하면

두 번째 segment의 segment number는 n+1000이 되는 것이고, 두 번째 segment도 1000 byte의 데이터를 담았다고 하면

세 번째 segment는 n+2000의 sequence number를 갖는 것입니다.

이렇게 sequence number를 가지고 receiver에 도착 하면, 

receiver는 이 sequence number 번호를 보고 순서를 맞출 수가 있는 것입니다.

다음에 그 밑에 있는 acknowledgment number도 sequence number와 동일한 크기를 갖는데

TCP에서의 acknowledgment는 receiver가 다음에 받아야 되는 segment의 sequence number를 전달 하는 것입니다.

예를 들면 receiver가 첫 번째 segment를 받았습니다. 

해당 sequence number가 n이고 데이터 사이즈를 보니까 1000 byte 이라면,

마지막 byte는 n+999였겠죠. N 부터 999 까지 받았단 뜻입니다.

acknowledgment number를 사용 할 때는 한 가지 주의해야 할 점이 있는데

처음에 sender가 데이터를 보내는 경우인데 이 때는 이전에 받은 것이 없으니까 acknowledgment number가 의미가 없습니다.

그렇지만 뭔가 들어 있긴 들어 있을 거에요.

그래서 실제로 acknowledgment number가 의미를 갖기 위해서는,

실제 의미 있는 값을 담고 있다 설명 하기 위해서 앞에 flag의 bit 중에 Ack bit 라는 것이 있습니다.

이 ack bit가 1로 설정 되어 있으면 acknowledgment number라는 필드가 실제 의미 있는 ack를 담고 있다는 것을 뜻합니다.

또 한 가지 TCP에서 주의 해야 할 점은 TCP 연결은 full-duplex 방식이라는 점 입니다. 

그래서 꼭 sender와 receiver가 항상 정해져 있는 것은 아닙니다.

A 한테 데이터가 올 때 마다 B가 ACK를 별도로 만들어 보낸 것이 아니라,

A가 보낸 데이터에 대한 ACK를 자기가 보낼 데이터, 

B가 보낼 데이터에다가 이 필드만 A가 보낸 데이터에 대한 ACK로 사용 하는 것이고, 

나머지는 B도 데이터를 담아서 보낼 수 있다는 뜻입니다 .

그래서 같은 형태의 데이터가 서로 왔다 갔다 하면서 만약에 아주 특별한 경우에 실제 자기 데이터가 없이 ACK로만 사용 하는

segment도 있을 수도 있지만 대부분의 경우에는 B도 자기 데이터를 보낼 때,

A가 보낸 데이터에 대한 ACK를 필드에 담아서 보내는 이런 형태를 취하고 있다 하는 것을 알아 두셔야합니다.

---
