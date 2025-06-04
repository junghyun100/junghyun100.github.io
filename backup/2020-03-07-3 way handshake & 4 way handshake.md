---
layout: post
title: "3 way handshake & 4 way handshake"
tags: [Network]
comments: true

---

이번 포스팅에서는 3 way handshake & 4 way handshake 에서의 간단한 정리입니다.

---

### 3 way handshake

TCP는 연결지향적이며 오류제어, 흐름제어, 혼잡제어, 타이머재전송 등의 기능을 하며 

<strong>연결지향</strong>이란말은 데이타를 전송하는 측과 데이타를 전송받는 측에서

전용의 데이타 전송 선로(Session)을 만든다는 의미입니다.

데이타의 신뢰도가 중요하다고 판단될때 주로 사용됩니다.

따라서 접속이 되어있느냐의 판단유무가 상당히 중요합니다. 

논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행됩니다.

<img src="https://media.geeksforgeeks.org/wp-content/uploads/TCP-connection-1.png">

1) 클라이언트가 서버에게 연결요청 메시지 전송(SYN) 패킷을 보냄 (sequence : x)

2) 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (sequence : y, ACK : x + 1)

3) 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄

- SYN(Synchronization) : 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄

- ACK(Acknowledgement) : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

이렇게 3번의 통신이 완료되면 연결이 성립됩니다.

<br>

### 조금 더 알아보기 <a href="https://sleepyeyes.tistory.com/4"> 취약점을 이용한 공격 </a>

3way handshaking의 취약점을 이용해 서버를 공격 하는 방법이 있는데 이를 SYN Flooding 이라한다

 3way handshaking 과정중 서버는 2단계 에서 (클라이언트로 부터 요청을받고 응답을 하고난 후 다시 클라이언트의 응답을 기다리는 상태)
 
 이 연결을 메모리 공간인 백로그큐(Backlog  Queue) 에 저장을 하고 클라이언트의 응답 
 
 <br>
 
 즉 3단계를 기다리게 되고 일정 시간 (default 로 UNIX/LINUX : 60초 , Windows : 256초 , Apache : 300 초이며 수정 가능) 동안
 
 응답이 안오면 연결을 초기화하는 점을 이용한 공격법이다.

 <br>

 악의적인 공격자가 실제로 존재하지 않는 클라이언트IP로 응답이 없는 연결을 초기화 하기전에
 
 또 새로운 연결 즉 1단계 요청만 무수히 많이 보내어 백로그 큐를 포화 상태로 만들어 
 
 다른 사용자로 부터 더이상에 연결 요청을 못 받게 하는 공격 방법이다.

 <br>

대응책으로는 연결 타이머 시간을 짧게 하거나 백로그 큐 사이즈를 늘리는법, 

정해진 시간동안 들어오는 연결 요구의 수를 제한하는법, 쿠키(cookie)라는 것을 이용해서 전체 연결이

설정되기 전까지는 자원의 할당을 연기하는 법이 있다.

<br>

### 4 way handshake - 연결 해제


연결을 종료하기 위해서도, 3 Way Handshake 처럼, 4 Way Handshaking이라는 과정을 거친다.

<img src="https://media.geeksforgeeks.org/wp-content/uploads/CN.png">

1) 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.

2) 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. 

3) 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.

4) 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. <br>
(아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.)

- 서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)

- TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)

- FIN(Finish) : 세션을 종료시키는데 사용되며 더 이상 보낸 데이터가 없음을 표시

이렇게 4번의 통신이 완료되면 연결이 해제된다.

<br>



##### [참고 자료]

[링크](<https://www.geeksforgeeks.org/tcp-connection-termination/>)

---
