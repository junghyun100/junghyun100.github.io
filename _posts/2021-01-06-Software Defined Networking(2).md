---
layout: post
title: "네트워크 - Software Defined Networking(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Software Defined Networking(SDN)

<img src="">

과거에는 라우터, 스위치 생산자가 인증한 특정한 라우팅 알고리즘만이 시스템에 들어 갈 수 있었지만

사용자가 "어떤 우선 순위를 기반으로 하는 라우팅 알고리즘"을 쉽게 실험을 해 볼 수가 있게 되었고

그것을 통해서 라우팅 알고리즘의 발전 속도도 빨라지게 되었습니다.

결국은 과거에는 통으로 되어 있던 것이 control plane과 data plane으로 

분할이 된 것을 software defined networking 이라고 부릅니다.

data plane의 밑에는 control agent가 있어서

remote controller에게 네트워크 topology에 대한 정보를 모아주는 역할을 합니다.

여기에서 모은 정보를 기반으로 remote controller는 

특정 source와 특정 destination 간의 최적의 경로,

또 특정한 데이터 플로우에 대한 최적의 경로를 선택 해서 각 라우터에 심어주고

각 라우터에서는 그 테이블을 기반으로 데이터를 전달하는 기능을 가지게 됩니다.

그래서 하부의 데이터 스위치들이 하는 포워딩 방식을 

generalized forwarding 이라고 이야기 합니다.

그리고 나머지 remote controller의 controller의 윗 부분에 방화벽 같은 것이 있습니다.

Firewall의 경우 어떤 특정한 fault에 접근 하려는 데이터나

아니면 특정한 source에서 온 데이터에 대한 접근 제어 정책 이나

데이터를 다른 길로 분산 시키는 로드 밸런싱처럼 사용자의 각종 정책을 반영 할 수 있습니다.

# Generalized Forwarding

<img src="">

generalized forwarding을 이해 하기 위해서는 기존의 traditional 방식의 포워딩이 

어떻게 이루어 졌는가 살펴 봐야 됩니다.

기존의 포워딩은 한 마디로 얘기하면 destination-based forwarding입니다.

> destination 전달 되기 위한 최적의 hop으로 포워딩을 해 주는 것<br> 결국 destination IP based forwarding

그래서 generalized forwarding는 어떤 특정한 하나의 필드를 기반으로 하는 포워딩이 아니고

사용자가 선택한 필드의 set을 고려해서 가장 적합한 경로를 찾습니다.

그래서 특정한 하나가 아니라 사용자가 정한 어떤 정보의 집합에 기반해서 포워딩 하는 것 입니다.

그러면 어떠한 정보를 기반으로 하느냐 하면 각 라우터가 가지고 있는 flow table을 기반으로 합니다.

# Network Control Applications

<img src="">

SDN 시스템은 크게 세 부분으로 구별을 하는데, 제일 윗 부분이 네트워크 제어 애플리케이션 입니다.

어떤 라우팅, 시스템에 대한 접근 제어 그리고 load balancing

데이터 부하를 분산 시키는 이런 정책들을 결정을 해 놓으면 

진짜 라우터에 반영이 되게끔 됩니다.

그래서 결국 이 부분이 결국은 brain, 전체 제어의 뇌, 핵심이라고 볼 수 있습니다.

이것을 라우터에 쉽게 적용 할 수 있게끔 

SDN 라우터 개발 회사들은 SDN 시스템의 SDN 컨트롤러에 API를 심어 놨습니다.

# SDN Controller (Network OS)

<img src="">

이 컨트롤러는 하부 스위치들에서 하부에 있던 control agent, CA들이 

네트워크 상태 정보를 전달해주면, 그것들을 모으고 그 정보를 기반으로 

위의 라우팅 알고리즘을 적용을 해서 길을 찾습니다.

그리고 상위에 있는 사용자의 정책과도 커뮤니케이션을 해야 하고

하부의 스위치와도 커뮤니케이션을 해야 하기 때문에 

거기에 맞는 API를 각각 만들어 두고 있고

이 API를 구별 해서 상부는 northbound, 즉 북쪽이라는 뜻이고 

하부는 southbound 이런 식으로 구별을 합니다.

그리고 SDN controller는 logically centralized 입니다.

중앙 집중형 시스템의 문제점은 고장이 날 수 있다는 겁니다.

고장이 나면 전체 네트워크가 망가지는, 

전체 네트워크가 동작을 하지 못하는 문제가 있습니다.

그래서 실제로는 여러 실제 물리적인 시스템에 분산을 해 놓고
 
이것을 서로 동기화를 시킴으로써 

외부에서 보기에는 정말 하나의 시스템인 것 처럼 보이게 만드는 것. 

그것이 centralized system 입니다.

그래서 실제로는 여러 군데 분산 되어 있는 컨트롤러가 

유기적인 동결을 통해서 하나의 컨트롤러로 보이게 만든다 하는 것이 

SDN 컨트롤러의 특징이 입니다.

# Data Plane Switches

<img src="">

마지막으로 제일 아래 부분은 컨트롤러가 계산한 플로우 테이블에 따라서

데이터를 전달만 하는 그런 스위치들이 존재 하는 것이 데이터 평면입니다.

그리고 밑의 스위치와 컨트롤러와 통신을 하는 프로토콜과 API가 존재합니다.

대표적으로 openflow라는 것이 현재 일반적으로 많이 쓰이고 있는 프로토콜 입니다.

---
