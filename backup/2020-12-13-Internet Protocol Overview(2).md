---
layout: post
title: "네트워크 - Internet Protocol Overview(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# IP Fragmentation & Reassembly

분할 된 뒤에 목적지에 도달하면 재조립 하기 위해

`16-bit identifier`, `flag`, `fragment offset`를 `IP 헤더`에 담도록 했습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1213/IP%20fragmentation%20%26%20reassembly(2).PNG">

## Fragmentation ID(identifier)

하나의 데이터그램이 세 개로 분할 되었을 때 

분할된 모든 데이터그램이 `같은 ID`를 갖도록 합니다.

나중에 목적지에 도달 했을 때 세 개의 데이터그램이 

원래는 <strong>동일한 데이터그램에서 잘려 나온 것</strong>임을 알 수 있게 하기 위함입니다.

그러나 이렇게 동일한 ID를 부여 하면 동일한 곳에 속한다는 것은 알지만,

데이터그램을 조립할 때의 <strong>순서는 알 수가 없습니다.</strong>

그래서 순서를 알아내기 위해 `Fragment offset`을 사용합니다.

## Fragment offset

오른 쪽의 그림을 보고 예를 들어서 설명 드리겠습니다. 

하나의 큰 데이터그램이 있고, 데이터그램의 전체 길이는 `4000 byte` 입니다.

`fragmentation ID`가 x라는 ID를 가지고, 

전체 데이터그램을 처음부터 갖고 있기 때문에 `offset`이 0 입니다.

그런데 여기서 4000 byte의 크기를 지원 했었는데 

다음 네트워크는 `1500 byte`를 최대로 지원하기 때문에 분할을 시켜야합니다.

ID는 세 개 동일하게 x로 동일하게 주게 됩니다.

최대가 1500 btye이므로 분할된 데이터그램은 

20 byte의 헤더, 1480 byte의 데이터필드를 담을 수 있습니다.

첫번째 데이터 그램에는 <strong>0 ~ 1479 byte</strong> 까지의 데이터가 담기고,

두번째 부터는 <strong>1480 ~ 2959 byte</strong> 까지의 데이터가 담기게 됩니다.

여기서 전체 데이터그램의 시작 위치를 `offset` 정보에다가 담는다는 것입니다.

`offset` 값은 두번째 데이터그램을 예로 시작점 1480을 `8로 나눈 값`을 담습니다. 

그렇게 `185`가 담기게 됩니다.

> 8로 나누는 이유 :  length bit는 1 bit, offset은 앞의 flag가 3 bit를 차지합니다.
> 13 bit로 16 bit의 길이, 65536를 offset에 그대로 담을 수 없기 때문입니다

## Flag

Flag는 `3 bit`를 차지하는데 현재 세 bit 중에 <strong>첫 번째 bit는 사용하고 있지 않습니다.</strong>

따라서 항상 0을 나타냅니다.

두 번째 bit는 해당 IP 데이터 그램은 중간에서 <strong>자르지 말라</strong>는 것을 뜻 합니다.

만약에 잘라야 되는 상황이라면 자르지 않고는 전달 할 수 없는 상황이라면 버리게 됩니다.

일반적인 데이터의 경우에는 그런 경우가 드물기 때문에 앞의 두 비트는 0의 값을 가지고,

마지막 비트로 결정이 됩니다.

세번째 비트는 `more fragment`, <strong>뒤에 분할 된 데이터그램이 더 있는가</strong>를 뜻합니다.

# Time-to-Live(TTL)

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1213/TTL.PNG">

<strong>TTL</strong>은 어떤 데이터그램이 영원히 네트워크를 돌아다니는 일을 막기 위한 것입니다. 

라우터들이 처음 설정 된 TTL 값이 한 라우터를 거칠 때 마다 1씩 줄어들게 하고 

이것이 0이 되면 라우터가 그 데이터그램을 버려 버리게 함으로써

이런 좀비 데이터그램, 방랑 데이터그램을 네트워크에서 없앨 수 이는 그런 장점이 있습니다. 

그림을 예로 들어서, 처음 출발지에서 `TTL` 값을 `255`라고 설정을 했습니다.

`255`부터 `B`까지 오는데 `192`홉을 거친 상태이고, 

`A`까지 가는데 라우터 여러개를 거쳐 `TTL`이 `60`까지 줄어들게 됩니다.

만약 `A`가 최종 목적지라고 하면 마지막 컴퓨터는 몇 hop을 거쳐서 왔는지를 알 수 있게 됩니다. 

---
