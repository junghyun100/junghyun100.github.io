---
layout: post
title: "네트워크 - Inside of Router(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Switching Fabrics

`input port`를 거쳐서 온 데이터를 `switching fabric`이

`output`쪽으로 보낼때 어떻게 하드웨어적으로 처리하는지 알아 봅시다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1210/Switching%20Fabric.PNG">

### Switching rate(스위칭 속도)

<strong>Switching rate</strong>는 `input port에서 output port로 전달 되는 속도`를 의미 합니다.

라우터가 재 기능을 수행하기 위해서는 모든 `input buffer`의 `datagram`이 

도착 하는 도착률 보다는 더 빠르게 처리 해 줄 수 있어야 합니다.

기존의 라우터들을 몇 가지 구별 해 보면 아래와 같습니다.

1. 메모리(memory) 방식
2. 버스(bus) 방식
3. 크로스바(crossbar) 방식

# Switching via Memory

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1210/Switching%20via%20Memory.PNG">

가장 첫 번째 있었던 <strong>메모리 방식</strong>은 가장 간단한 형태의 라우터라고 생각 할 수 있습니다.
 
이것은 특별히 라우터로 별도로 만들 수도 있지만, 보통의 PC를 갖고 라우터로 구성 할 수 있습니다.

`IP datagram`이 이더넷 카드나 랜 카드로 도착을 하면 `주 메모리`로 복사 하고,

IP 헤더 주소를 보고 목적지 주소가 있는 포트쪽으로 내보냅니다.

소프트웨어적으로 처리 하기 때문에 특별히 라우터라는 장비를 따로 살 필요도 없고,
 
그래서 비용적으로나 구현적으로나 매우 간단합니다.

문제점은 시스템 내에서 메모리 간의 복사가 한 번 일어나고,

주 메모리에서 랜 카드에 있는 메모리로 복사가 일어나기에 <strong>시간이 오래 걸린다는 것</strong>입니다.

# Switching via Bus

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1210/Switching%20via%20BUS.PNG">

메모리 방식에서는 주 메모리를 거쳐서 주 메모리에서 다시 복사가 일어나는 과정이 있었습니다.

메모리 방식에서 나타나는 라우터의 속도 제약을 완화하기 위해서 나온 것이 <strong>BUS 방식</strong>입니다.
 
`input port에 있는 버퍼`와 `output port에 있는 버퍼` 간에 <strong>직접 복사가 일어남</strong>으로써,

직접 복사가 일어나기 때문에 <strong>훨씬 빠르게 처리</strong> 할 수 있습니다.

만약에 그림처럼 input buffer 여러 개와 output buffer가 여러 개가 있으면 

이들 사이에 input buffer 측에서 어느 output buffer로도 갈 수 있어야 하기 때문에

중간에 `공용적인 버스`를 두어서 직접 복사가 일어 날 수 있도록 되어 있습니다.

그런데 메모리 방식보다는 조금 낫지만, 

버스를 공유하기 때문에 버스 전체에 데이터가 흘러 가게 되겠습니다. 

그래서 이렇게 흘러가는 와중에 다른 input쪽에서 데이터가 다시 들어 오면 

충돌이 일어나면서 데이터가 제대로 전달이 안 될 수가 있습니다.

즉, 동시에 전송 할 수는 없고 버스를 누군가 하나의 input buffer와 하나의 output buffer,

이 쌍이 <strong>특정 시간에는 이 버스를 독점적으로 점유 해야 되는 제약</strong>이 있습니다.

# Switching via Interconnection Network

버스를 사용하는 것은 메모리를 사용하는 방식에 비해서 속도 면으로 향상이 되었다고 하더라도, 

사용하는 데 있어서 경쟁을 거쳐야 되기 때문에 <strong>대규모 라우터에서 사용하기에는 적합하지 않았습니다.</strong>

그래서 버스 방식의 문제점을 완화하기 위해서 나온 방식들이 있습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1210/Switching%20via%20interconnection%20Network.PNG">

오른쪽 그림을 보면 `crossbar` 형태로, 바둑판 모양의 네트워크를 사용하는 방식이 있습니다.

이 경우 세 쌍이 동시에 통신을 할 수 있게 됩니다. 

중간에 스위치가 있기 때문에 스위치가 전달을 해 주는 것입니다.

두 곳의 데이터가 동시에 한쪽으로 나가야 한다면 전달 되는 동안

한 지점의 스위치에서만 받아서 전달 할 수는 없습니다. 

그러나 물리적으로 길이 다른 경우, 모든 input과 output 쌍들이 동시에 전송이 가능합니다.

경우에 따라서 switching fabric이 `Banyan 네트워크`를 구성하는 경우도 있었습니다.

`crossbar switch`는 모든 input과 모든 output에 하나하나의 switch를 다 두었습니다.
 
input port의 개수가 n이고, output port의 개수가 m이면 n 곱하기 m 만큼의 switch가 필요 합니다.

그러나 banyan 네트워크의 특징은 crossbar switch처럼 <strong>많은 switch가 필요 하지 않습니다.</strong>

만약 input이 8개이고 output이 8개인 경우 

crossbar switch 방식에서는 8 * 8 = 64, 64개의 switch가 필요하지만

banyan 네트워크 방식은 오른쪽 그림처럼 중간에 레벨을 두어 구성하므로써,

switch의 개수가 적기 때문에 비용적인 측면에서는 저렴하지만, 

<strong>HOL blocking을 겪을 수 있는 확률</strong>이 좀 더 높아질 수가 있겠습니다.

---
