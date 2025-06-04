---
layout: post
title: "네트워크 - TCP Congestion Control Algorithm(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# TCP NewReno(1999)

## TCP Reno방식의 문제점

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1206/Reno.PNG">

TCP Reno를 사용하다가보니 문제점을 발견했습니다.

네트워크에서의 어떤 에러는 개별적으로 발생하기 보다는 `한꺼번에 발생하는 경우`가 많이 있었습니다.

> 기차를 타고 가면서 인터넷을 한다고 생각하면, <br>
> 기차가 터널을 통과 하는 동안 연속적으로 패킷이 잃어버리는 경우

터널 뿐만 아니라 어떠한 바람이 불거나 환경에 의해서 짧은 시간이지만 

그 기간 동안에 연속적으로 패킷이 에러가 나는 경우가 많았다는 것 입니다.

그림에 있는 것 처럼 X 표시가 에러로, 연속적으로 에러가 발생했습니다.

Reno 방식을 쓰게 되면 첫 번째 에러가 발생 했을 때,

이 문제를 해결하기 위해서 congestion window가 반으로 줄게 되고 

그러면 이렇게 해서 첫 번째 문제를 해결 하고 나면 두 번째, 세 번째도 반복하게 됩니다.

결국은 세 개만 연속으로 있어도 8분의 1로 줄어 드는 문제가 발생했습니다.

이러한 문제점을 해결하고자 나온 것이 `NewReno`입니다.

## TCP Reno

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1206/newReno.PNG">

다른 것은 Reno방식과 동일하지만 fast recovery를 조금 바뀌었습니다.

Reno에서는 fast recovery를 중복 되지 않은 패킷(non-duplicate ACK)이 올 때 까지

기다리다가 받았을 때 종료하고, 다시 새로운 작업에 들어갔습니다.

NewReno는 현재 전송한 데이터 대해서 ACK는 받지 못했지만 

ACK를 기다리고 있는 데이터가 있다고 하면 loss가 발생 했음을 감지 했을 때 

보낸 모든 패킷에 대한 ACK가 끝날 때 까지는 `fast recovery를 종료하지 않습니다.`

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1206/newReno2.PNG">

State 1에서 에러를 발견하면 State 2처럼 congestion window가 

반으로 줄어드는 것 까지는 똑같지만, duplicate ACK를 받을 때 마다 

옆으로 움직이면서 크기는 유지 한 채로 계속 하나씩 전송을 합니다.

State 4의 까만 점은 non-duplicate ACK를 뜻합니다.

이것은 앞의 에러가 났던 패킷은 제대로 전송이 되었다는 것입니다.

State 6까지 계속 전송을 하고 여기에 대한 duplicate ACK가 올 때 까지

congestion window size는 `유지 한 채로 계속 전송`은 합니다.

재전송을 계속 하다가 최초의 에러를 발견 하기 전 까지 발송 했던 패킷들의 

전송이 모두 끝나고 non-duplicate ACK가 들어 오면 fast recovery가 끝이 납니다.

# Tahoe → Reno → NewReno

변천사와 함께 정리를 합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1206/%EB%B3%80%EC%B2%9C%EC%82%AC.PNG">

Tahoe의 특징은 `slow start`, `congestion avoidance`, `fast retransmit`을 도입 했다는 것이고,

Reno는 거기에다가 `fast recovery`라는 개념을 도입 해서 fast retransmit의 경우에는 좀 다르게 동작하게끔

타임 아웃에 의한 loss 발견 하고는 좀 다르게 동작하게 만들었다는 것이고,

NewReno는 Reno와 거의 비슷하지만 연속적인 패킷 에러를 위해서 `좀 개선 된 fast recovery 방식`을 도입했습니다.

# TCP SACK(1996)

TCP SACK은 `Selective ACK`의 약자입니다. 

individual ACK와 비슷한 개념이지만, 다른 점은 TCP 메시지 안에 여러 개 서로 연결 되지 않은

segment에 대한 acknowledgement를 보낼 수 있게 만들었습니다.

하나의 ACK 메시지 내에 `필드를 분할 해서 특정 segment들에 대해서 ACK 응답`을 보낼 수 있도록 

디자인 되었다는 것이 특징입니다.

---
