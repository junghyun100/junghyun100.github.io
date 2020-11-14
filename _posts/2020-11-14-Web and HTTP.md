---
layout: post
title: "네트워크 - Web and HTTP"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

## HTTP Overview

### HTTP란?

데이터를 주고 받는 프로토콜을 HTTP라고 이야기 합니다.

> HTTP : HyperText Transfer Protocol

HTTP라는 용어가 왜 생기게 되었느냐 하면, 하이퍼링크(Hyperlink)라는 용어가 있습니다.

하이퍼링크는 이것을 마우스로 클릭 했을 때 어딘가로 이동 할 때, 클릭을 함으로써

특정한 데이터를 읽어 올 수 있는 링크를 하이퍼링크라고 합니다.
 
하이퍼링크가 포함 된 텍스트라고 해서 하이퍼텍스트라고 합니다.

이런 하이퍼텍스트를 전달 하는 프로토콜이라고 해서 HTTP라는 이름이 붙었습니다.

그리고 웹 프로그램은 가장 대표적인 client/server model의 하나 입니다.

항상 특정한 서버의 데이터가 필요하면 먼저 유저가 PC나 스마트폰을 통해서 HTTP request message 보냅니다.

request message를 받은 서버는 거기에 대한 응답을 보내오게 되고,

응답 메세지를 받아서 클라이언트는 화면에 디스플레이 하게 됩니다.

### Based on TCP

웹 프로그램은 데이터 로스가 일어나서는 안 됩니다. 

그래서 하부의 TCP 프로토콜을 사용 합니다.

TCP 프로토콜을 통해서 데이터를 전달하기 위해서는 먼저 TCP 연결을 설정해야 되기 때문에

웹 브라우저를 통해서 어느 웹 서버에 접속하고자 하면,

눈에는 보이지 않지만 그 밑의 트랜스포트 레이어 계층에서는 TCP가 동작을 하고

TCP는 다시 네트워크 레이어를 거치고 데이터 링크 레이어를 거쳐서 물리단을 거쳐 이렇게 통신을 하게 됩니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1114/RTT%2CHTTP_response_Time.PNG">

HTTP는 시간 축으로 비교를 해서 보면 그림과 같이 진행이 됩니다. 

만약에 서버에 접속을 하겠다 해서 HTTP 명령을 날리면 HTTP 명령이 처음부터 날아가는 것이 아니라 

<strong>먼저 TCP 연결을 수립 합니다.<strong>

그래서 TCP 연결을 수립 하기 위해서 어느 정도 TCP 연결 요청 메세지가 전달 되기 위한 시간이 필요합니다.

전달 요청 메세지가 가고 서버가 TCP 연결을 수락 하면 위쪽의 RTT가 TCP를 연결 하는데 걸리는 시간이 되겠습니다.

> RTT : Round-Trip-Time, 신호가 갔다가 응답이 돌아오는데 까지 걸리는 시간.

그 이후에 HTTP request message가 전달이 됩니다.

그에 응답으로 reponse가 전달 되겠고 필요한 파일을 요구 했으면 그 파일을 전송 하는 데 또 시간이 걸립니다.

그래서 전체적으로 HTTP response time, 내가 request를 보내고 reponse를 받는 데 까지 시간을 따져 보면

1. TCP 연결을 먼저 수립 하기 위해서 1RTT

2. 간단한 request와 response가 오는 데 1RTT

3. + 파일 전송을 요구를 했다고 하면 파일 용량이 크다고 하면 파일을 전송하는 시간 만큼이 같이 필요 합니다.

## HTTP Message

HTTP는 일단 클라이언트 request와 서버 response 두 가지로 이루어지는데 구성되는 형태는 다음과 같습니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1114/MessageFormat.PNG">

> method : 이것은 특정 명령어에 해당 하는 것

어떤 데이터를 갖고 싶으면 GET이나 POST 이런 method가 들어가는 것이고요

> URL : 어떤 오브젝트를 가져 오고 싶은지 작성

> HTTP version : 1.1 버전, 2 버전과 같은 버전을 작성

> header field name : 어떠한 호스트에서 가져 오고 싶은가 와 같이 여러가지 정보들이 담기는 곳

어떤 정보의 종류를 말 하고 그 정보의 값을 얘기하고 이런 식으로 쭉 나열. 

실제 데이터, 파일을 전달해야 한다면 entuty body에 전달이 되겠습니다.

## Http Request Message

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1114/HttpRequestMessage.PNG">

HTTP request message에 예를 보면 다음과 같습니다. 

앞에 method에 해당하는 GET이나 POST, HEAD, PUT, DELETE 이런 명령어를 담을 수 있습니다.

* GET : 뒤에 있는 이 오브젝트(/index.html)를 가져 오겠다는 것.
 
* POST : 웹 페이지 검색 창 같은 것을 보면 데이터를 넣고 엔터를 치면 이 데이터가 날아가듯 데이터를 보내는 것.

* HEAD : 전체 데이터는 필요 없고 필요로 하는 오브젝트의 정보, 실제 데이터의 정보에 해당하는 것만 요청하는 것.

* PUT : HTTP를 통해서 서버에다가 파일을 넣거나, 수정하거나 파일을 지우는 것.

* DELETE : HTTP를 이용해 resource를 지우는 것.

## HTTP Response Message

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1114/HttpResponseMessage.PNG">

위처럼 request를 받게 되면 서버는 거기에 response를 하게 됩니다. 

그래서 여기에는 버전이 있고, 클라이언트의 요청에 대한 처리 결과를 나타내는 것이 버젼 뒤에 나옵니다.
 
###  Reponse code

* 200 OK : 처리가 성공적으로 끝이 났다.

* 301 : 요구한 오브젝트가 다른 곳으로 옮겨졌고 그 오브젝트로 이동하겠다.

* 400 : 요청한 request를 이해 하지 못했다.

* 404 : 요청한 request를 찾지 못했다. 없다.

* 505 : HTTP 버전이 달라서 너와 통신 할 수 없다

## What is REST

REST는 Representational State Transfer으로 

간단히 말해서 서버가 클라이언트의 상태를 유지하지 않겠다는 것 입니다.

우리가 어떤 작업을 처리하는 데 현재 상태가 중요할 때가 있습니다.

> 예를 들면 장기를 둔다던가 체스를 둔다고 하면 <br>
> 어떤 장기 알, 체스 말이 이동 할 수 있는 위치는 현재 상태의 영향을 받습니다.<br>

그런데 HTTP를 통한 서버 클라이언트 모델에서는 

클라이언트의 위치, 현재 상태를 유지 하는 정보를 담고 있으려면 서버에게 너무 큰 오버헤드가 됩니다.

특히 클라이언트 수가 많아진 요즘 같은 세상에서는 서버가 그런 것을 다 할 수가 없어졌습니다.

그래서 서버는 클라이언트의 state를 저장하고 있지 않고,
 
그 대신에 주고 받는 HTTP 메세지 안에 클라이언트에 대한 모든 정보가 다 들어 있도록 하는 것입니다.

그것을 해석 할 수 있는 방법까지 다 들어 있기에

그래서 서버는 굳이 특정 클라이언트에 대한 상태를 저장하고 있지 않아도 

HTTP request message를 받으면 어떻게 처리 해야 될 지,

반대로 클라이언트도 서버의 response message를 받으면 어떤 방식으로 데이터를 가져와야 할 지가 

모두 코드 안에 다 담겨져 있습니다.

이런 architecture를 따르는 서비스를 `RESTful`이라고 이야기 합니다.

REST의 구조가 가져야 하는 요구사항들이 크게 6가지로 정리가 되어 있습니다.
 
### Requirment

1. 클라이언트 서버 구조여야 한다.<br>이것은 실제 데이터와 유저에 대한 정보가 분리되어 있어야 한다는 것.
2. 클라이언트의 현재 상황을 서버에 저장하지 않는다.
3. 서버의 response가 클라이언트쪽에 또는 어떤 중간의 웹 캐시 같은 데에 저장 할 수 있어야 한다.
4. 클라이언트가 접속 할 때 서버에 직접 접속이 된 상태인지 중간의 웹 프록시 서버에 접속 된 것인지 이런 것을 구별 할 수 없어야 한다.<br>둘 사이에 결국 차이가 없어야 한다는 것
5. optional) 자바의 경우에 자바 애플릿이나 자바 스크립트이 해당 특성을 가집니다.<br> 어떤 데이터를 해석하는데 있어서 해석 할 수 있는 방법이 클라이언트 측에 없는 경우<br>해석하고 사용할 수 있는 응용프로그램 까지 전달 해주는 개념
6. 특정한 컴퓨터 architecture에 제약 받지 않아야 한다.<br>모든 인터페이스가 독립적으로서, HTML이나 XML, JSON 같은 표준화된 언어를 사용해서 특정한 컴퓨터 architecture에 제약 받지 않는, 제한 되지 않는 그러한 서비스를 해야 한다는 뜻


---
