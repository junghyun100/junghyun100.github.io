---
layout: post
title: "네트워크 - Link Layer Basics Protocol"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Link Layer Terminologies

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0111/Link%20Layer%20Terminologies.PNG">

<strong>Node</strong>는 네트워크의 `호스트`나 `라우터`를 뜻합니다.

<strong>Link 또는 media</strong>는 호스트와 라우터같은 

서로 이웃한 두 개의 노드를 연결 해 주는

`물리적인 어떤 연결`을 뜻 합니다.

그리고 Link 또는 media는 

`wired Link`와 `wireless Link`로 구분 합니다.

<strong>Frame</strong>은 `링크 레이어`에 생성 되는 `패킷`입니다.

이 <strong>Frame</strong>은 3계층의 패킷인 `Datagram`을 

품고 있는 `encapsulate`된 형태를 띕니다.

# Link Layer Basic Functions

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0111/Link%20Layer%20Basic%20Functions.PNG">

이번엔 링크 레이어는 어떤 역할을 맡고 있는가 살펴봅니다.

<strong>physically adjacent</strong>라고 되어있습니다.

뜻은 <strong>"물리적으로 서로 연결 되어 있는"</strong>으로

물리적으로 연결 되어 있는 두 개의 노드 사이에서 

`데이터그램을 전달 해 주는 역할`을 합니다.

그래서 오른편의 그림을 보시면 

`sending machine`과 `receiving machine`이 있는데

물리적으로 유선으로 연결이 되어 있거나 아니면 

둘 사이가 무선 채널이 닿을 수 있는 거리에 있습니다.

그리고 둘 사이에서 `payload` 부분에 

3계층의 `datagram`이 들어가고 

`datagram`의 `헤더`와 `트레일러`를 붙여서 `프레임`을 만든 뒤 

물리 링크를 통해서 실제로 전달 하게 됩니다.

이렇게 전달 되는 과정에서 

`비트 에러`가 발생 할 수 있기 때문에 

링크 레이어에서는 `bit error handling`이 

하나의 중요한  기능이 됩니다.

또 하나는 이 링크는 여러 노드들이 공유 할 수 있습니다.

만약에 무선으로 연결 되었다고 하면 

`receiving machine`과 통신 할 수 있는 

다른 `machine`들 있을 수 있기 때문에

같은 채널을 쓸 경우 동시에 전송을 하게 되면 

패킷의 충돌이 일어날 수 있습니다.

그래서 이러한 패킷의 충돌도 핸들링 해야 됩니다.

# Data Transmission Base

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/2021%EB%85%84/0111/Data%20Transmission%20Base.PNG">

`링크 레이어 기술`은 데이터 전송의 기반이라고 말 할 수 있습니다.

실제 네트워크를 봤을 때 여러 호스트들이 서로 연결 되어 있습니다.

그림처럼 호스트와 호스트를 연결하는 로컬 네트워크는 

서로 다른 기술로 이루어 질 수 있습니다.

`IP의 역할`은 서로 다른 형태의 네트워크들을 통합해서 

전 세계를 아우르는 하나의 가상의 통합 네트워크를 만드는 것이고,

`IP`를 가지고 `목적지 네트워크`를 찾아 갈 수 있습니다.

그리고 이 과정에서 실제로 목적지 `receiving host`가 포함 되어 있는 

호스트에게 데이터를 전달 하기 위해서는 여러가지 링크 레이어 기술을 사용 해야만 

그 목적지 컴퓨터에 실제적으로 데이터를 전달 해 줄 수 있다는 뜻입니다.

---
