---
layout: post
title: "네트워크 - Reliable Data Transfer Principles(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

실제로 패킷로스가 어떻게 이루어지는가를 살펴 볼 텐데 

이런 패킷을 재 전송 하는 방식을 전체적으로 ARQ라고 부릅니다.

# ARQ 

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1127/Communication%20Error%20Recovery.PNG">

Automatic Repeat reQuest에서 ARQ를 따서 부릅니다.

단순한 방식으로는 `stop-and-wait` 방식.

한 번에 하나의 segment씩 보내는 것입니다. 

하나의 segment를 보내서 검사를 받고

또 하나의 segment를 보내고 검사를 받는 식입니다.

`Pipelining method` 같은 경우에는 

stop-and-wait 방식이 퍼포먼스가 상당히 낮은 편입니다.

좀 여러 데이터, segment를 한꺼번에 보내고 

한꺼번에 검사 받는 방식으로 제안이 되었습니다.

Pipelining method는 크게 `go-back-N`과 

`selective repeat` 이 두 가지 방식으로 나눌 수 있습니다.

## Stop-and-Wait

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1127/stop%20and%20wait.PNG">

sender가 하나의 패킷을 보내고 나서 receiver의 응답을 기다립니다.

그리고 ACK를 받게 되면 sender는 전송을 다시 재개하게 됩니다.

그런데 일정 시간이 지나도록 ACK를 받지 못한다고 하면

sender는 이전에 보냈던 그 패킷을 재 전송 하게 되는 것입니다. 

### loss 없이 전송 되는 경우

Sender는 패킷을 보내고, receiver는 패킷을 받은 뒤에 

그 패킷을 검사 합니다.

Checksum을 검사 해서 에러가 없다고 하면 ,

거기에 ACK를 보내게 되는 식입니다.

그러면 Sender는 아까 보낸 패킷 0 에 대한 ACK를 받았기 때문에 

이번에는 다음 패킷인 패킷 1을 보내게 됩니다.

그러면 receiver는 다시 패킷을 검사 하게 되고요. 

Checksum을 검사 해서 에러가 없다고 하면 다시 ACK를 보내 주고

ACK를 받고 나면 다시 다음 패킷을 보내는 식으로 동작을 하게 됩니다.

### Packet loss가 발생 하는 경우

sender가 패킷 0을 보내고 ACK를 받았습니다.

그 다음에 패킷 1을 보냈는데 중간에 패킷 1이 분실 되었습니다. 

그러면 receiver 입장에서는 아무 것도 오지 않았기 때문에 

아무 것도 할 것이 없습니다.

Sender가 보냈는데 중간에 없어 진 것인지 아닌지 모르기 때문에 

어떤 방식을 취할지 모르기 때문입니다.

Sender 입장에서는 패킷을 보내고 나서 시간을 잽니다. 

시간을 재서 이것이 일정 시간 내에 응답이 오지 않는면?

타이머가 종료 될 때 까지 ACK가 오지 않는다 하면 

`보낸 패킷이 분실 되었구나`라고 가정을 하고 

다시 패킷 1을 재전송 하게 됩니다.

Receiver 입장에서는 이것을 받아서 검사 하고 제대로 되었다면,

ACK를 전송 하면 됩니다. 

### ACK가 loss된 경우

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1127/stop%20and%20wait2.PNG">

앞서와 똑같이 이렇게 패킷 0은 왔다 갔다 제대로 동작을 했고,

두 번째 패킷 1이 전달 되었습니다.

receiver까지 잘 도달 했기에 receiver가 ACK를 보냈는데 

이 ACK가 loss 된 경우입니다.

앞에서는 패킷이 분실 된 경우(B)였고 

(C)는 패킷은 잘 도착했는데 ACK가 분실 된 경우고.

달라 보이지만 `Sender의 입장`에서는 똑같은 경우입니다.

Sender 입장에서는 

어쨌던 자기가 아는 것은 ACK가 시간 내에 오지 않았다는 것이지

내 데이터 패킷이 분실 된 것인지 아니면 

receiver가 보낸 ACK 패킷이 분실 된 것인지 

구별 할 수가 없다는 것입니다.

그래서 앞서 B의 경우와 마찬가지로 타이머가 종료 되면서

그 시간 내에 ACK가 오지 않으면 패킷 1을 재전송 하게 됩니다.

하지만 앞서 (B) 그림과 이 경우 `Receiver 입장`에서는 다릅니다.

Sender 입장에서는 같아도, 

Receiver는 앞에서는 데이터 패킷 1을 받지 않았던 것입니다. 

이게 없었기 때문에 도착한 패킷을 받아서 검사를 하고,

ACK를 보낸 것입니다. 

그런데 이 두 번째 경우 C 그림에서 receiver는 이것을 받았습니다.

받고 ACK를 보냈는데 다시 재전송이 온 것입니다. 

재 전송이 다시 온 것을 어떻게 아느냐 하면

sequence number가 0,1,0,1 순서대로 와야 되는데 1이 오고,

다시 1이 왔기에 sequence number가 동일한 것이 왔다는 것입니다.

그러면 receiver 입장에서는 

`아까 ACK를 보냈는데 그 ACK를 sender가 받지 못했구나` 라고 생각 합니다.

그렇게 해서 receiver는 이것을 받아서 sequence number가 같다고 하면

receiver는 앞선 패킷의 중복을 판단을 하고,

새롭게 받은 패킷은 버리고 ACK만 다시 재전송 합니다.

그래서 이 ACK가 sender에 도착 하게 되면 

sender는 패킷이 제대로 도달을 했다고 판단을 하는 것 입니다.

### 주의 해야 할 점 (D그림)

<strong>타임 아웃, 시간을 어떻게 잡느냐?</strong> 입니다.

만약에 타임 아웃 시간을 패킷이나 ACK가 분실 됐는데

너무 긴 시간을 기다리게 된다면, 

전체적으로 전송 속도가 낮아지는 문제가 있습니다.

그렇다고 타임 아웃 시간을 너무 짧게 잡으면, 

패킷 1이 제대로 도달 했고 ACK를 전송 했는데, 

중간에 약간 네트워크의 혼잡이 있어서

ACK1가 느리게 도착 하는 경우도 있을 수 있습니다.

인터넷에서 모든 데이터가 똑같은 시간에 도달 하는 것은 아니기 때문에

패킷 교환망에서는 들쭉날쭉한 시간이 있을 수 있는데

그것을 고려하지 않고 타임 아웃을 너무 짧게 잡게 된다면,

ACK가 제대로 도달 했을 텐데, 

타임 아웃이 너무 짧아서 재전송을 해 버리는 경우가 발생 합니다. 

타임 아웃 시간을 잡을 때는 sender와 receiver 간의 왕복 시간

`round trip time`을 잘 고려 해서 정해야합니다.


---
