---
layout: post
title: "네트워크 - Datagram Forwarding"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# IP Network

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1217/IP%20network.PNG">

<strong>IP 네트워크</strong>는 IP 패킷들을 데이터그램이라고 하기 때문에 

<strong>데이터그램 네트워크</strong>라고도 이야기 합니다.

IP 네트워크의 특징은 중간에 네트워크 레이어에서 

`call setup` 같은 것이 일어나지 않습니다.

TCP 같은 경우에는 연결을 설립 해야하지만

IP에서는 `call setup`의 필요 없이 중간 라우터가 하나의 데이터그램을 받으면

목적지 주소와 포워딩 테이블에 있는 내용을 보고, 

다음 길을 결정 하는 방식으로 동작 합니다. 

이 데이터그램 포워딩을 하는 방식이 두 가지가 있습니다. 

### Destination-Based Forwarding

기존에 IPv4 방식으로, 

<strong>목적지 주소</strong>를 보고 해당 목적지 주소에 따라서 

다음 node로 가장 적합한 node가 어떤 것인가 결정 해서 보내는 것입니다.

### Generalized Forwarding

<strong>SDN</strong>, `Software Defined Network` 라는 것이 나오면서, 

<strong>Generalized Forwarding</strong>가 등장했습니다.

<strong>Destination-Based Forwarding</strong>은 목적지 주소를 가지고 포워딩을 합니다.

그와 다르게 SDN에 가서는 목적지 주소 만이 아닌 여러 가지를 고려 할 수 있습니다.

> 예 : 중간에 자원들이 어떻게 할당 되어 있는가, 얼마나 혼잡이 일어나고 있는가 등

그래서 하나의 특정한 값에 의해 Forwarding되는 것이 아닌 여러 사항,

그것을 일반화 된 <strong>Generalized Forwarding</strong>으로 말하는 것 입니다.

# Destination-Based Forwarding Table

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1217/Destination-Based-Forwarding%20Table.PNG">

어떤 데이터그림이 도달하게 되면 목적지 주소를 살펴보고,

그 목적지 주소에 따라서 가장 적합한 output 링크를 결정 해서 

내보내는 그런 기능에 도움을 주는 것이 <strong>Destination-based forwarding table</strong>입니다.

그런데 목적지 주소는 `32 bit`나 되기 때문에 개수로 따지면 

`40억개`가 넘는 IP 주소를 갖는 것이고, 이렇게 많은 주소를 다 담게 되면 

매치 하는데 오래 걸리기에 모든 IP 주소를 테이블에 담지는 않습니다.

대신 네트워크 ID만을 담아서 네트워크 주소만을 가지고 전달을 합니다.

# Destination-Based Forwarding

모든 네트워크의 ID를 담는 것도 사실 만만치 않은 작업입니다.

Class C 같은 경우에는 `2의 21승` 개의 네트워크가 존재 할 수 있습니다.

그래서 `destination address`를 아래 그림과 같이 <strong>range를 두는 방식</strong>을 사용합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1217/Destination-Based-Forwarding.PNG">

실제로 포워딩 테이블 내에 이런 숫자가 써져 있지만,

"through"으로 작성되어 있는 것은 아닙니다. 

이것을 어떻게 나누냐면

공통적인 숫자들 까지만 두고, 나머지를 와일드카드(`*`)로 두게 되면 

이 범위에 해당하는 모든 것은 이 범위에 들어 갈 수 있게 되는 것입니다.

# Longest Prefix Matching

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1217/Longest%20Prefix%20matching.PNG">

실제 형태는 그림처럼 작성되어 집니다.

제일 첫 번째 필드에 여기서 through라고 되어 있었는데 총 21 bit 까지가 일치하기 때문에 

나머지는 와일드카드(`*`)로 둔 모습입니다.

그런데 여기서 한 가지 궁금하실 수 있는 것은 무엇이냐 하면 

두번째 케이스와 세번째 케이스를 살펴 보시면 `11001000 00010111 00011(21 bit)`까지는 동일합니다.

두개의 차이는 뒤의 3 bit가 더 있고 세번째의 경우 그 3 bit가 와일드카드 처리 되어 있다는 점 입니다.

여기서 `21 bit`까지면 생각하면 `link interface 1번`도 될 수 있고 `2번`도 후보가 될 수 있는 Address가 존재할 수 있습니다.

그러면 라우터는 더 자세한 숫자가 더 길게 일치하는 것이 사실 이 주소를 가진 데이터그램한테는 

더 적합한 `output link`일 확률이 높다고 판단하고 해당 인터페이스 측으로 `forwarding`하게 됩니다.

목적지 주소가 가장 길게 일치하는 하나를 찾아서 거기로 내보내는 것을 <strong>longest prefix matching</strong> 라고 합니다.

---
