---
layout: post
title: "네트워크 - E-Mail"
tags: [컴퓨터 네트워크]
comments: true

---


컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Electronic Mail

전자우편을 구성 하는 요소는 크게 세 가지 입니다. 

일단 `user agents`, 우리가 메일을 작성할 수 있게 만드는 클라이언트 프로그램을 뜻합니다.

`mail server`, gmail이라던지 야후 메일이라던지 네이버 메일이라던지 핫메일이라던지 이런 메일 서버들이 존재합니다.

mail server 또는 user agents들이 데이터를 주고 받기 위한 SMTP, POP3, IMAP와 같은 `protocol`이 존재합니다.

이 메일 서버는 `메일 박스(mail box)`와 `메시지 큐(message queue)`가 있습니다.

> 메일 박스 : 외부에서 어떤 데이터를 받으면 저장 해 놓았다가, 접속을 했을 때 그 데이터를 전달 해 주는 것

> 메시지 큐 : 메세지를 외부로 보낼 것을 저장을 해 놓았다가, 연결이 성립 되면 그 때 메시지를 보내는 것

메일 서버들 간에 서로 다른 프로토콜을 쓰게 되면 메일의 내용을 주고 받을 수 없으므로,

SMTP(Simple Mail Transfer Protocol)이라는 것으로서 메일 서버들이 서로 연결이 되어 있습니다.

> 지메일 서버와 네이버 서버라고 하더라도 동일한 프로토콜을 써서 통신을 하기 때문에 메일의 내용을 정확하게 서로 주고 받을 수 있게 되는 것

## SMTP(Simple Mail Transfer Protocol)

일단 하부의 트랜스포트 레이어 프로토콜로는 TCP를 씁니다.

이메일에 오류가 생겨 의미가 변색되지 않도록 신뢰성이 있는 이메일 전송이 필요하기 때문입니다.

SMTP는 `handshaking`, 서로 메세지를 보내는 서버와 받는 서버를 확인 하고

다음에 메세지를 서로 보내고 끝내는 간단한 구조로 되어 있습니다.

### SMTP Example

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1117/SMTP.PNG">

예를 들어서 alice가 사용하는 특정 메일 서버의 이름은 crepes.fr 입니다.

hamberger.edu라는 서버의 ID bob이라는 사람한테 메일을 보내려고 합니다.

SMTP로 접속을 하게 되면 서버는 클라이언트한테 먼저 자기의 이름을 전달 해 줍니다.

서버가 hamberger.edu라는 서버ID를 알려 주는 것이고, 

그러면 클라이언트가 그것에 대한 응답으로 crepes.fr를 이야기 하는 것입니다. 

이렇게 데이터가 시작 되고, 실제 데이터가 다 날아가고 나면 첫 번째 자리에 점을 하나 찍음으로써 메세지 전달이 끝나는 것입니다.
 


---
