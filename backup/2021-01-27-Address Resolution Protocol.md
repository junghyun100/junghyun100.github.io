---
layout: post
title: "네트워크 - Address Resolution Protocol"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

<strong>Address Resolution Protocol</strong>을 공부해봅니다.

# MAC address

<img src="/images/2021년/0127/MAC Address.PNG">

<strong>ARP</strong>에서 말하는 `address`는 <strong>MAC address</strong>를 뜻합니다.

네트워크 레이어에서 많이 쓰는 주소는 <strong>IP address</strong>로, `logical address`입니다.

소프트웨어적으로 할당 할 수가 있어서 비교를 하자면 집 주소라고 말 할 수 있습니다.

반면에 <strong>MAC address</strong>라는 것은 

링크 레이어에서 부여되는 주소인데 이것은 `physical address` 입니다.

비교하자면 주민등록번호 같은 것 입니다.

```
이사를 가면 나의 집 주소는 변경이 된다. = 논리적 주소

이사를 가더라도 나의 주민등록번호가 바뀌는건 아니다. = 물리적 주소
```
`physical address`라고 불리는 이유는 

`네트워크 인터페이스 카드`에는 `ROM(Read Only Memory)`이 들어있습니다.

<strong>이미 내장되어 있어서</strong> 사용자가 주소를 마음대로 바꿀 수 없기 때문입니다.

`DOS창`을 열어서 `ipconfig`를 하면 주소가 `16진수`의 형태로 나타나게 되고,

> e.g : 1A - 2F - BB - 76 - 09 - AD

`16진수`이니 하나의 숫자에 `4bit`씩 사용이 됩니다.

그래서 `총 48bit`, `6byte`의 주소의 값을 사용하고 

이 값들은 전 세계적으로 유니크하도록 생산이 됩니다.

그런데 이미 변할 수 없는 주소이기 때문에 

하나의 랜에서 다른 랜으로 옮겨서 사용 해도 상관 없습니다.

이더넷 카드를 뽑아서 다른 컴퓨터에 꽂아서 써도 아무 상관이 없다는 뜻입니다.

반면에 IP주소는 계층 형태를 가진 논리적 주소이기 때문에 엉뚱한 주소를 가지고,

설정을 하면 컴퓨터를 찾아 올 수 없습니다.

# MAC Address and ARP

<img src="/images/2021년/0127/MAC Address and ARP.PNG">

하나의 `source`에서 `destination`까지 도착 하는 데 있어서 여러 네트워크를 거쳐 가는데

각각의 링크에는 서로 다른 형태의 링크 레이어 프로토콜을 사용합니다.

이것을 아울러서 목적지 네트워크까지 찾아가는게 `IP주소`이지만,

최종적으로 데이터를 전달 하기 위해서는 `MAC 주소`가 필요 합니다.

# ARP(Address Resolution Protocol)

<img src="/images/2021년/0127/ARP(Address Resolution Protocol.PNG">

라우팅을 통해서 목적지 네트워크 까지 도착을 했습니다.

데이터그램이 가지는 목적지 `IP주소`를 보았을 때 `137.196.7.78` 이라면,

해당 IP를 쓰는 곳으로 전달을 해 주어야 되는데, `MAC 주소`를 알아야 전달 할 수 있습니다.

그런데 링크 레이어 주소를 모르는 상태라면,

각각의 랜 안에 있는 컴퓨터들은 `ARP table`를 이용합니다. 

여기에는 아래와 같은 정보들이 담겨집니다.

* 어떤 컴퓨터가 사용하는 IP 주소 
* 그 컴퓨터의 실제 MAC 주소 
* Time To Live, 매핑의 유효 시간.

그래서 라우터가 데이터그램을 받아서 

목적지 주소에 해당하는 `entry`를 찾고 

`137.196.7.78`은 `MAC 주소`를 `1A-2F-BB-76-09-AD`를 사용하는 것을 압니다.

이 `MAC 주소`를 담아서 이더넷 프레임을 만들어서 전송을 하는 것입니다.

---
