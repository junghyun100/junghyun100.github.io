---
layout: post
title: "네트워크 - Multiplexing Socket"
tags: [컴퓨터 네트워크]
comments: true

---


컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

전송 계층의 기본 기능인 멀티플렉싱(Multuplexing)에 대해 알아봅니다.

논리 회로에 있는 Multiplexer, Demultiplexer와 개념이 같습니다. 

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1122/Multiplexing%20and%20Demultiplexing.PNG">

## Multiplexing(다중화)

하나의 컴퓨터에 여러 개의 프로그램 또는 여러 개의 프로세스가 동작하고 있습니다.

익스플로러나 크롬도 통신을 하고, 다양한 것들이 통신 기능을 제공하고 있습니다.

각각 통신 기능을 제공하지만 결국에는 랜 케이블이라던지 노트북 같은 경우 

랜 카드를 통해서 특정 주파수를 통해 인터넷에 접속이 됩니다.

결국은 하나의 컴퓨터에 있는 여러 프로그램, 여러 프로세스들은 

아래 인터넷 레이어, 링크 레이어, 피지컬 레이어로 내려가면

<strong>결국은 하나의 회선, 하나의 주파수 채널로 전송이 되어야 합니다. </strong>

멀티플렉싱은 동시에 내려오는 데이터들을 동일한 하나의 공유하고 있는 회선을 통해서 

전달하고자 하는 것 입니다.

## Demultiplexing(역 다중화)

Multiplexing와는 반대로 섞여서 날아온 데이터들은 

목적지 컴퓨터에 도달하게 되면 각각에 맞는 프로세스로 나뉘어서 전달이 되어야 됩니다.

각각에 맞는 프로세스한테 전달 해 주는 것이 `reciever의 역할`이고,

그 역할이 demultiplexing입니다.

## Who Provide Multiplexing and Demultiplexing

결론부터 말하자면 `전송 계층에서 제공` 합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1122/Whoprovide%20MpDmp.PNG">

왼쪽을 클라이언트를 클라이언트 1번, 오른쪽은 클라이언트 2번이라고 보고, 가운데가 서버입니다.

클라이언트 1번이 프로세스 P3라는 것을 띄워서 전송 합니다. 

그리고 서버에 접속 합니다.

접속을 해서 서버의 P1이라는 프로세스와 통신을 하게 됩니다.

마찬가지로 클라이언트 2는 P4라는 프로세스를 띄워서 서버에 접속을 해서 P2와 통신을 합니다.

이렇게 되면 서버 입장에서는 어떤 데이터를 자기가 생성 했을 때,

그것이 P1이 전송하는 데이터인지 P2가 전송하는 데이터인지 나누어서

링크로 흘려 보내는 기능이 Multiplexing 입니다.

반대로 보면 클라이언트 1이 보내오는 데이터나 클라이언트 2가 보내 오는 데이터는

결국 똑같은 물리적 채널을 통해서 이 컴퓨터에 들어오게 됩니다.

피지컬 링크 네트워크 까지는 같이 오다가, 트랜스포트 까지 오면 여기서 분리가 되어서

P3가 보내 온 내용은 P1에, P4가 전해 온 내용은 P2에 나누어서 보내는 것이 demultiplexing입니다.

## How Demultiplexing Works

이런 기능을 제공하기 위해서는 결국 어떤 정보가 있어야 합니다.

앞서 보신 그림에서 어떤 클라이언트가 보낸 데이터인지 구별 할 수 있는 정보가 필요하고,

그 정보가 `포트 넘버(port number)`가 입니다.

<img src ="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1122/HowDemultiplexing.PNG">

그리고 전송 계층의 segment들은 특별한 필드를 갖고 있는데 이 포트 넘버를 갖는 필드가 있습니다.

TCP나 UDP나 그 안에 필드들은 다 똑같지 않은데 맨 첫 번째에 있는 것이 포트 넘버 입니다.

이 포트 넘버가 서로 다른 애플리케이션 또는 서로 다른 역할을 하는 프로세스들한테 

다른 포트 넘버를 할당해서 구별 할 수 있게 만드는 것입니다.

앞서 그림에서 보신 것 처럼 P3가 P1 하고 통신을 하고 싶을때, 

그러면 P1에 포트 넘버를 담아 주어야 하는 것이고,

그것은 트랜스포트 레이어가 직접 할 수 있는 것이 아니라,

응용 프로그램이 `소켓(socket)`을 통해서 트랜스포트 레이어에 지시를 내립니다. 

## Socket

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1122/Socket.PNG">

보통 OS들은 트랜스포트 레이어와 애플리케이션 레이어 사이에 어떤 API, 함수들을 제공 합니다.

API(Application Program Interface)라고 하는 것은 어떤 함수이고, 

이 함수를 애플리케이션이 호출 하면 데이터를 보낼 수 있습니다.

API들의 집합이 여러 가지가 있는데 이런 것들을 묶어서 소켓이라고 이야기 합니다.

프로세스들은 소켓 인터페이스를 통해서 자기의 데이터를 주거나 받거나 할 수가 있습니다.

API가 제공하는 parameter의 규격에 맞게끔 데이터를 넣어주면 데이터가 전달 된다는 것 입니다.

---
