---
layout: post
title: "네트워크 - Reliable Data Transfer Principles(3)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

stop-and-wait 방식을 보면 전체적으로 효율이 어떻게 되는가 살펴봅니다.

# Performance Limit of Stop-and-Wait

<img src= "https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1128/Perfomance%20Limit%20stop%26wait.PNG">

만약에 한국과 미국 사이에서 통신을 한다고 가정을 합니다.

둘 사이 링크의 밴드위스가 1 Gbps 입니다.

둘 사이의 거리가 멀다 보니까 전파 지연 시간이 15 ms가 걸립니다.

내가 한 번 보내는 패킷의 사이즈가 평균 8000 bit, 1 kB 정도가 된다고 가정을 해 봅니다.

> 1 kB를 전송 하기 위한 전송 시간(transmission delay) =  L / R

R은 1 기가 bps, 10의 9승 bit, L의 크기는 8000 bit이므로 `transmission delay`는 8 μs입니다.

`stop-and-wait` 방식은 sender가 1 kB 데이터를 보내고, 

ACK를 응답받아야 합니다.

전체적으로 우리가 `utilization`을 계산 해 보면 U = (L / R) / (RTT + (L / R)) 으로 계산 할 수 있습니다.

한 번 갔다가 오는데 15 ms의 전파 지연 시간이 존재합니다.

전파 지연 시간이 있고 `transmission delay`가 8 μs 포함 되어 있습니다.

실제 걸리는 시간은 왕복 시간(Round Trip Time), 

`RTT` 30 ms 더하기 전송 지연 시간 R 분의 L, 8 μs 하면 총 30.008 ms가 걸립니다.

전체 30.008 ms 중에서 8 μs. 

이것을 계산 해 보면 0.00027 입니다.

결국 퍼센트로 보면 0.027%. 전체 네트워크 자원 중에 0.027% 밖에 사용 하지 않는다는 뜻입니다.

그래서 이것은 실제로 1 Gbps나 되는 링크를 설치 해 놓고 `stop-and-wait` 방식의 네트워크 프로토콜 때문에

물리 자원을 굉장히 낭비 하는 문제가 있습니다.

## Stop and Wait Operation

이것을 시간 축으로 살펴 보면 그림과 같습니다.

<img src ="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1128/Stop%20and%20Wait%20Operation.PNG">

세로축이 시간이고 가로축은 sender에서 receiver 까지 가는 거리라고 생각 해 보면

sender가 0초에서 부터 시작을 해서 첫 번째 bit가 receiver에 도달 하는 데 까지 전파 지연 시간이 15 ms가 걸립니다.

다음에 이 사이에 전송 지연 시간이 아까 8 μs가 있는 것이죠. 

그래서 15 ms 더하기 8 μs로 receiver에 도착을 하고 receiver의 ACK 메시지가 거의 사이즈가 없다.

여기서는 전송 지연 시간 고려 없이 한다고 하더라도 1 bit 라고만 가정 하더라도, 

15 ms의 전파 지연 시간이 있다는 겁니다.

그렇게 해서 총 30.008 ms 중에서 실제 데이터가 전달 되는 시간은 8 μs 밖에 안 된는 것 입니다.

# Pipelined Protocols

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1128/Pipelined%20Protocol.PNG">

먼 거리로 데이터를 보낼 때,

예를 들어 미국 서부에서 동부까지 굉장히 긴 거리를 패킷 하나 보내고 ACK 하나 받고 하면 너무 낭비가 심합니다.

따라서 한 번에 여러 개를 보내고 ACK도 병렬적으로 오게 되면 훨씬 효율이 높지 않겠느냐는 것 입니다.

이런 방식을 나중에 좀 더 세분화 해서 `go-back-N`과 `selective repeat`이란 방식으로 제안이 되어 있습니다.

## Increased Utilization

<img src= "https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1128/Pipelined%20Protocol2.PNG">

Pipelined Protocol을 쓰게 되면 앞서 `stop-and-wait` 보다 utilization을 높일 수 있습니다. 

그림에서 보시면 하나의 패킷을 보내고 다음 ack를 기다리고 있는 것이 아니라 

다음 패킷을 또 보내고 또 보내고 있습니다.

그러니까 전체적으로 전송 시간이 일단은 세 배로 늘어난 다는 것입니다. 

여기서 사용하는 전체 시간은 똑같이 30.008초라고 하더라도

세 개의 패킷이 연달아 왔기 때문에 그 세 배, 8 μs의 세 배인 24 μs를 데이터를 보내는 데 사용을 했다는 것이고

그렇게 하면 0.081%, 이전에 비해서 세 배로 늘어 났다는 것입니다.

물론 0.081%도 매우 낮은 것이지만 예를 들면 세 개를 연달아 보내게 해 주면 이렇게 된다는 것이고

30개, 300개를 한다고 하면 30배, 300배를 보낼 수가 있습니다.

이렇게 pipeline 방식을 사용 해서 연달아 데이터를 보내서 전체 utilization을 높일 수 있습니다.

## Pipelined Protocols : Overview

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1128/Pipelined%20Protocol%20Overview.PNG">

Pipelined protocol은 앞서 말씀 드린 대로 `Go-back-N`, `selective repeat` 이렇게 두 가지로 나누어 볼 수가 있습니다.

둘 다 동일하게 한 번에 N개의 데이터를 pipeline 방식으로 전달 할 수 있게 허락을 해 두었습니다.

그런데 `Go-back-N`과 `selective repeat`의 가장 기본적인 차이는 뭐냐 하면 

### 차이점1

`Go-back-N`은 전송 중에 쭉 패킷이 전송 되어 왔을 때,

Sender 입장에서 전달을 했는데 중간에 receiver가 판단을 해 보니 한 지점이 잘못 되었다고 하면 그 다음부터 다시 모두 재 전송을 해야 됩니다.

`selective repeat`은 중간에 하나가 잘못 되었다면,

잘못된 패킷만 다시 재 전송을 하게 됩니다.

그렇게 하려다 보니까 ACK도 좀 다른 방식을 쓰게 됩니다.

### 차이점2

`go-back-N`은 `cumulative ack`을 쓰게 되고 cumulative라는 것은 누적 ack라는 것입니다.

sender가 여러 개를 동시에 보냈는데 일일이 모든 패킷에 대해서 ack를 보내자니 receiver의 입장에선 상당히 번거롭습니다.

그래서 앞에 세 개를 받았다면 세 번째에 대해서만 ack를 보내는 것입니다.

그러면 이 ack가 뜻하는 바는 해당 패킷만 잘 왔다는 것만 아니라 해당 순번까지 잘 왔다고 알려주는 것이 누적 ack 입니다.

반면에 `individual ack`는 뒤에는 잘 왔다 하더라도 하나는 잘못 될 수 있기 때문에,

중간에 하나가 잘못 되어서 해당 패킷만 재 전송을 요구 하려면 개별적으로 ack가 필요 합니다.

그래서 `selective repeat`에서는 여기에 대한 ack가 온다면 이것 하나가 잘 도착했다는 의미지 이 앞이 다 잘 도착했다는 의미는 아닙니다.

### 차이점3

또 하나의 차이점은 타이머입니다.

`go-back-N`은 개념 상 잘못된 지점부터 모든 패킷을 다 재전송 하면 되니까

굳이 개별적인 타이머가 필요 없이, ack를 받지 못한 패킷들 중에서 가장 먼저 보낸 패킷에 대해서 타이머가 설정 되어 있고

그 타임 아웃 내에 ack를 받지 못하면 이 뒤에 있는 모든 패킷을 재전송 하면 됩니다.

`selective repeat`은 모든 패킷에 대해서 별도의 개별적인 타이머를 가지고 있습니다.

---
