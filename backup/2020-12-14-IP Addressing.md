---
layout: post
title: "네트워크 - IP Addressing"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Introduction to IP Address

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1214/Introduction%20to%20IP%20address.PNG">

우리가 많이 사용하고 있는 `IPv4의 길이`는 32 bit로 되어 있습니다.

각각의 `호스트와 라우터의 인터페이스`에는 32 bit 짜리 IP 주소를 부여합니다.

> 인터페이스 : 호스트/라우터와 물리적 물리적인 링크를 연결해주는 랜카드 같은 것

각각의 호스트가 인터페이스 카드를 가지면 하나씩 주소를 할당 할 수 있습니다.

반대편의 라우터의 경우에는 보통 multiple, 복수 개의 인터페이스를 갖게 되는데,

각각의 인터페이스에 서로 다른 IP 주소를 갖게 됩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1214/IP%20address.PNG">

실제 각 인터페이스에 부여 되는 주소는 그림과 같은 형태 입니다.

1과 0으로 이루어져 있는 32 bit 주소를 8 bit 단위로 나누어서

`223.1.1.1`의 식으로 기억 하는 것입니다.

# Hierarchical Addressing

IP 주소는 마구잡이로 부여되는 것이 아니라 규칙성에 의해 부여 됩니다.

그렇게 `계층화` 되어 있음으로 인해서 전체 IP 주소는 다음과 같이 구성 됩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1214/IP%20address%20class.PNG">

1.0.0.0, 2.0.0.0으로 시작 되는 주소나 255.0.0.0을 쓰는 주소처럼 

전 세계에서 `네트워크를 관리하는 기관(ICANN)`에서 각각의 기관에

주소를 하나씩 할당 해 주면 그 할당 기관 내에서는 자신의 네트워크를 

다시 분할 해서 하나 하나 관리를 할 수 있는 계층화 된 IP 주소 형태를 가지게 됩니다.

# IP Address Class

인터넷이 처음에는 국가 기관이었지만, 

국가에서 통신망을 관리한다는 것이 일반 시민들의 자유 등을 침해 할 우려로 인해

점점 민간화 되면서 `ICANN(Internet Corporation for Assigned Names and Numbers)`라는

기관에서 각국의 네트워크 주소를 부여하거나 인터넷을 관리하는 역할을 하게 됩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1214/Hierarchical%20Addressing.PNG">

ICANN에서는 주소 부여와 함께 `DNS`, `Domain Name System`을 관리하는 역할을 합니다.

그렇게 해서 인터넷에서 사용하는 주소는 class A,B,C 부여 되는 주소는 그림과 같이 나뉩니다.

class D,E는 실제로 네트워크에 할당하는 주소가 아니라 데이터를 `multicast`한다거나,

`anycast`를 지원 하기 위해서 사용합니다.

class A,B,C의 공통적인 부분은 전체 주소가 네트워크 파트와 호스트 파트로 나뉜다는 것입니다.

<strong>네트워크 ID</strong>는 그 네트워크 집단 전체, 호스트 집단 전체를 대표하는 ID가 되고,

<strong>호스트 ID</strong>는 네트워크 집단을 관리하는 관리자가 그 안에서 

각각의 컴퓨터들한테 할당 하는 ID라는 것 입니다.

그래서 class A,B,C를 구별하는 방법은 총 32 bit 중에 

첫 번째 bit와 두 번째 bit, 세 번째 bit를 보고 구별 할 수 있는데

* 첫 번째 bit가 0이라면 class A 주소, 

* 첫 번째 bit가 1이고, 두 번째 bit가 0이라면 class B 주소

* 첫 번째 bit가 1이고 두 번째 bit가 1이고 세 번째 bit가 0이면 class C 주소

이렇게 판단을 합니다. 

## Class A,B,C의 차이

<strong>class A</strong> 같은 경우에는 앞에 총 8 bit가 네트워크 ID로 사용되고,
 
뒤의 부분 24 bit는 호스트 ID로 사용 됩니다.

<strong>class B</strong> 같은 경우에는 앞의 2 bit 포함 해서 16 bit가 네트워크 ID, 

나머지 16 bit가 호스트 ID 입니다.

<strong>class C</strong> 같은 경우에는 앞의 3 bit 포함 해서 24 bit가 네트워크 ID,

나머지 8 bit가 호스트 ID 입니다.

이렇게 되면 앞의 8 bit가 될 수 있는 숫자는 0부터 127까지,

class A의 네트워크 ID로 될 수 있는 것은 총 0부터 127까지, 128개가 가능 합니다

> 왜 127까지인가 : 첫 번째 bit가 1이 되는 순간 128이 되는데 <br>
> 128은 첫 번째 bit가 1이 되어야 하기 때문입니다.

class B 같은 경우에는 처음에 2 bit가 1,0이 되고 나머지 14 bit를 보면 191이 됩니다. 

따라서 128부터 191까지가 class B 주소가 됩니다.

class C 같은 경우에는 192 에서 223 까지가 해당합니다.

### 정리

<strong>class A</strong> 주소는 전 세계에 128개의 네트워크 주소를 가질 수 있습니다.

네트워크 내에서는 (2의 24승)개의 호스트를 구별 할 수 있다는 것입니다.

<strong>class B</strong> 같은 경우에는 14 bit(2의 14승)개의 네트워크 주소를 가질 수 있습니다.

네트워크 내에서는 65536개의 호스트를 구별 할 수 있다는 것입니다.

<strong>class C</strong> 같은 경우에는 21 bit(2의 21승)개의 네트워크 주소를 가질 수 있습니다.

네트워크 내에서는 256개의 호스트를 구별 할 수 있다는 것입니다.

사실 각 호스트 범위에서 -2개씩 해주어야하는데 

네트워크 주소, 브로드캐스트 주소 사용으로 인해 호스트 주소에서 제외해야 합니다.

---
