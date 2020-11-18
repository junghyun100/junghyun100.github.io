---
layout: post
title: "네트워크 - E-Mail"
tags: [컴퓨터 네트워크]
comments: true

---


컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의


---
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
 
### Mail Access Protocols

보통 사용자가 서버에 접속하거나 서버 끼리는 SMTP를 쓰는데,

서버에서 메일을 가져 올 때는 보통 `메일 엑세스 프로토콜(mail access protocol)` 이라는 것을 사용 합니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1117/mail%20access%20protocol.PNG">

* POP

Post Office Protocol의 약자이고 버전 3이 많이 쓰이기 때문에 POP3라고 불립니다.

기본적으로 POP3는 데이터를 가져 오면 서버에 있던 데이터를 다 지우게 됩니다.

그래서 내가 데이터를 데이터를 가져 오려고 할 때 마다 TCP 연결을 수립하고 데이터를 가져오고 나면

메일을 서버에서 삭제하고 연결을 끊고 이런 과정을 거칩니다.

* IMAP

IMAP은 Internet Mail Access Protocol의 약자이고, POP3와 다르게 데이터를 서버에 온전히 남겨 두고,

메일 박스에 있는 내용들을 폴더처럼 정리 할 수 있게 되어 있습니다.

메시지 폴더를 정리 할 수 있게 되어 있기 때문에, 어느 디바이스로 언제 접속을 해도 똑같은 구조의 메일을 받아 볼 수가 있습니다.
 
> synchronization : 어떠한 디바이스를 가지고 언제 접속을 해도 항상 똑같은 형태를 볼 수 있는 기능

그리고 연결이 POP3처럼 자동으로 끊어지지 않고, Connection을 유지하고 있기 때문에 지속적으로, 주기적으로 메일을 다운로드 받을 수 있는 특징이 있습니다.

서버의 오버헤드는 좀 크지만, 사용자의 입장에서는 좀 더 편리한 그런 구조 입니다.

### HTTP

웹 기반으로 이메일을 쓸 때는 웹 브라우저를 열어서 그 서버에 접속을 합니다.

유저에서 서버로 가는 경우, 또는 서버에서 유저로 데이터를 보내는 경우 HTTP를 쓰게 됩니다.

90년대 중반에 Hotmail에서 가장 먼저 했고 google, yahoo도 역시 HTTP를 통해서 메일을 사용하는 예 입니다.


---
