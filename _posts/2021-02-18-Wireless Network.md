---
layout: post
title: "네트워크 - Wireless Network"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Wireless Network Elements : Hosts

<img src="/images/2021년/0218/Hosts.PNG">

wireless 네트워크를 구성하는 요소들을 보면 먼저 `무선 호스트`가 있습니다.

이것은 유선망하고 크게 다르지 않습니다. 

## Laptop, SmartPhone

그런데 유선망에서는 데스크탑이나 라우터나 서버들이 호스트였는데

Wireless 호스트는 `랩탑`이나 `스마트폰`이나 `태블릿 PC`와 같이 

<strong>무선 통신기술이 적용 되어 있는 디바이스들</strong>이 wireless 호스트가 됩니다.

## Run applications

보통의 유선망에서의 앤드 호스트들처럼 

Wireless 호스트들에도 <strong>애플리케이션(휴대폰 앱 같은)이 작동</strong>하고 있습니다.

## May be stationary(non-mobile) or mobile

wireless 호스트들은 <strong>한 곳에 머물러 있을 수도 있고 이동 할 수도 있습니다.</strong>

그러나, 언제나 모빌리티를 의미하는 것은 아닙니다.

무선이더라도 접속해 있는 엑세스 포인트의 통신 범위를 벗어나게 되면

접속이 끊어져 버리기 때문에 

그리고 나중에 새롭게 다른 곳에 사용자가 접속을 해야 되는 경우기 때문에

이런 경우는 모빌리티가 없는 경우입니다.

# Wireless Network Elements : Base Station

<img src="/images/2021년/0218/Base Station.PNG">

`Base Station`은 호스트의 일종입니다.

## Typically connected to wired network

그런데 일반적인 호스트가 아니라 

네트워크 구조의 `인터넷과 같은 망에 접속 하는 지점`입니다.

베이스 스테이션이 하는 역할은 인터넷 망 같은 유선망과

무선 호스트를 연결 해 주는 `접점의 역할`을 해줍니다.

### Relay

유선망과 무선망의 접점 역을 하다보니 

그 사이에서 패킷을 서로 `relay` 해 주는 역할을 합니다.

예로는 무선 `전화망의 기지국`과 

`802.11 엑세스 포인트(와이파이 엑세스 포인트)`를 말합니다.

# Wireless Network Elements : Link

<img src="/images/2021년/0218/Link.PNG">

유선에서는 유선 링크가 있고, 무선망에서는 `wireless 링크`가 있습니다.

wireless 링크의 역할은 wireless 호스트와 Base Station을 이어줍니다.

Base Station와 백앤드에 있는 인터넷망 

또는 무선 전화망을 연결하는 망을 `backbone 링크`라고 하는데,

일부 기술은 이것도 무선으로 만들어 둔 경우가 있습니다. 

> 대표적인 예 : 와이맥스, 와이브로 


## 알아둘 점

## 1번

무선 네트워크라고 하더라도

무선인 구간은 <strong>Base Station와 wireless 호스트 사이</strong> 밖에 없습니다.

기지국과 무선 전화망과 인터넷망을 연결하는 링크들은 유선입니다.

## 2번

무선 주파수를 여러 디바이스들이 나눠 쓰다 보니

서로 충돌을 완화하기 위한 `MAC 프로토콜`이 존재 합니다.

그리고 무선 기술들에 따라서 data rate나 전송 거리가 천차만별 입니다.

`NFC(Near Field Communication)`이라는

신용카드 같은데 들어 있는 NFC 기술 같은 경우에는 

몇 cm 사이에서만 통신이 가능하고

`블루투스`는 10m 사이, `와이맥스`에서는 15km도 통신이 가능합니다.

# Network Topology : Infrastructure Mode

<img src="/images/2021년/0218/Infrastructure Mode.PNG">

무선통신망을 topology에 따라서 나누게 되면 두 가지로 나눌 수 있습니다.

그 중 하나는 `Infrastructure Mode` 입니다.

`infra` 어떠한 기반이 갖추어져있다고 해석 할 수 있습니다.

이 기반이라는 것은 Base Station을 기준으로 

유선망 또는 전화망과 연결 해 주는 기반 구조가 이미 갖춰져 있다는 뜻 입니다.

그래서 Base Station에 연결 되는 각각의 호스트들이 

Base Station을 통해서 망에 연결하는 구조를 `infrastructure 모드`라고 이야기 합니다.

## Handoff 또는 Handover

하나의 호스트가 접속 해 있던 Base Station을 변경 할 때 

`자동으로 변경이 되는 기술`을 `handoff`, `handover`라고 합니다.

그래서 handoff, handover 기술이 적용이 되면 

어떤 호스트가 하나의 Base Station, 하나의 엑세스 포인트에서 다른 데로 이동을 하더라도

<strong>사용자의 개입 없이 자동으로 연결이 계속 유지</strong>됩니다.

> 제공되는 서비스 예 : Wifi, cellular(무선 전화망)과 WiMAX




---
