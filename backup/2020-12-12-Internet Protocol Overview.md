---
layout: post
title: "네트워크 - Internet Protocol Overview"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Internet Layer

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1212/InternetLayer.PNG">

인터넷 계층의 주 역할은 `라우팅`과 `포워딩`입니다. 

라우팅 일을 하는 프로토콜들은 그림의 오른쪽에 약자로 적혀있습니다.

이 라우팅 프로토콜들이 공통적으로 하는 일은 

<strong>소스로부터 목적지까지 길을 결정 해 주는 역할</strong>입니다.

라우팅 프로토콜들, 라우팅 알고리즘들이 동작 하기 위해서는 

IP 프로토콜이 제공하는 정보가 필요합니다.

이 정보들은 

* 가장 기본적으로 주소
* 주소가 부여 되는 형태
* 주소가 부여 되면 어떤 형식으로 전체 네트워크가 구성 되는가?
* 데이터그램의 구성
* 패킷들을 어떻게 처리하는가?

를 포함하고 있습니다.

IP에서도 에러가 발생 하면, 라우터들이 알려 주어야 하는데 

`ICMP(Internet Control Message Protocol)`가 그 역할을 담당합니다.

# IP Datagram Format

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1212/IP%20datagram%20format.PNG">

그림은 IP 데이터그램의 헤더 형태 입니다. 

TCP와 유사하게 전체 기본적인 헤더가 `20 byte`의 길이를 갖습니다.

선택적인 옵션 헤더가 더 붙을 수도 있습니다.

### Version

첫 번째 필드는 `version`인데 4 bit 길이를 갖습니다.

인터넷 프로토콜 버전을 어떤 버전을 쓰느냐를 나타냅니다.

대부분은 버전 4지만 최신은 버전 6으로, 4 또는 6의 값을 가집니다.

### Header length

다음에 `header length`가 있습니다. 

`UDP`의 경우 옵션이 없었기에 명확하게 `8byte 헤더`를 갖기 때문에 없었다면,

TCP나 IP의 경우에는 옵션으로 인해 헤더가 어느 정도의 길이인지 

명확하게 나타내기 위해서 header length가 필요합니다.

### Type of Service

`type of service`라는 것은 `type of data`, 

멀티미디어 데이터인지, 실시간성이 필요한 데이터인지와 같이

데이터의 종류를 뜻하기 위해서 만들어 놓은 필드입니다.

### Length

`length`는 전체 데이터그램의 길이를 뜻합니다.

### 16-bit Identifier, Flag, Fragment Offset

다음에 세 개의 필드(16-bit identifier, flag, fragment offset)은

라우터가 너무 큰 segment를 데이터그램으로 분할하고,

목적지에 도착해서 재조립 하기 위해 필요한 정보를 담는 곳입니다.

### TTL

다음은 `TTL(Time To Live)` 입니다. 

TTL은 도착할 때 까지 최대 몇 hop을 거칠 수 있는지 정하는 필드입니다.

파랑 글씨로 설명은 "남아 있는 hop의 최대 값"으로 되어있는데,

출발 할 때 결정 하는 값 입니다.

하나의 라우터를 거칠 때 마다 1씩 줄어들고, 

0이 되어도 목적지에 도달하지 못하면 datagram을 drop 시킵니다.

### Upper Layer

`upper layer` 라는 것은, 트랜스포트 계층에서의 프로토콜이 무엇인가를 의미합니다.

TCP 또는 UDP Seement가 들어가고, 목적지에선 TCP 또는 UDP 처리부 중

어느쪽으로 넘겨줘야할지 결정하는 역할을 합니다.

### Header Checksum

마지막으로 `header checksum`이라는 것은

`CRC(Cyclical Redundancy Check)`라는 방식으로

데이터를 담아서 전달을 하면, 헤더가 정확한지 아닌지 알 수 있습니다.

헤더만을 checksum 하는 이유는 굳이 IP에는 전체 데이터가 모두 정확하게 도착 할 필요는 없고,

헤더만은 중요하게 다루겠다는 의미입니다.

헤더가 문제가 생긴다는 것은 주소가 잘못 될 가능성이 있는 것이기 때문입니다.

# IP Fragmentation & Reassembly

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1212/IP%20fragmentation%2C%20reassembly%20.PNG">

그림으로 예를 들면 source쪽 네트워크는 `최대 사이즈 4000 byte를 지원 하는 네트워크`입니다.

4000 byte 크기의 데이터가 전송되어 날아가고 있습니다. 

이 라우터의 다음은 이더넷을 사용하는, `최대 사이즈 1500 byte를 지원하는 네트워크`로 바뀐다면,

IP 프로토콜로 4000 byte 짜리를 <strong>잘게 나누어야 된다</strong>는 것입니다.

세 개의 프레임으로 나누어서 보내야 된다는 것입니다. 

빨간색으로 되어 있는 부분이 데이터이고 파란 부분이 헤더라고 하면 

빨간 부분을 잘게 세 개로 나눠서 각각에 헤더를 붙여서 전달을 하게 해야 됩니다.

이것이 `fragmentation & reassembly`의 역할이라는 것입니다.

그래서 네트워크 링크나 링크 계층에서는 `maximum transfer unit`, `maxium transfer size`가 

정해져 있고, 링크 레벨에 의해서 제약을 받게 됩니다.

그래서 이더넷이나 FDDI나 서로 다른 방식의 링크 프로토콜을 사용하게 되면,

허용하는 `MTU`, 최대 전송 사이즈가 달라지게 됩니다.

그래서 큰 데이터그램을 지원하던 네트워크에서 작은 크기의 데이터그램을 지원하는 네트워크로 

패킷을 전달하기 위해서는 데이터의 분할이 일어나야 합니다.

이렇게 분할이 된 데이터를 목적지에 도달해서는 원래의 데이터그램으로 복원해야 하기 때문에

그런 제약이 있는 것이고 이를 위해서 IP 헤더에 몇 가지 정보를 담도록 했습니다.

그것이 `16-bit identifier`, `flag`, `fragment offset`입니다.

---
