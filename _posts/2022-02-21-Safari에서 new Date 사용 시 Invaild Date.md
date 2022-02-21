---
layout: post
title: "Safari에서 new Date 사용 시 Invaild Date"
tags: [개발 TIP, Javascript]
comments: true

---

프로젝트를 진행하다가 발생한 오류이다.

Safari에서 new Date를 사용했을 때 Invaild Date가 나왔을때에 관련한 내용

---

## 상황 설명

![에러출력](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/0e55433434974b5c8563742d3f4d376c78bc35f0/images/22%EB%85%84/0221/%EC%97%90%EB%9F%AC%EB%A1%9C%EA%B7%B8.png)

상황은 다음과 같다.

프로젝트를 진행하는 과정에서 프론트쪽으로 리턴 받은 날짜를 

new Date() 객체 안에 인자로 넣었더니, 

인터넷 익스플로러에서는 정상적으로 동작을 하던 코드가, 

Safari에서는 <b>"undefined is not an object"</b>라는 TypeError를 출력해냈다.

현재 상황으로는 받고있는 리턴날짜의 형식대로 new Date()에 입력했을 때 

다음과 같이 출력이 되었다.

`리턴받는 날짜의 데이터 형식은 'yyyy-m-dd'의 형태로 받고 있는 상황`

![Invaild Date](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/0e55433434974b5c8563742d3f4d376c78bc35f0/images/22%EB%85%84/0221/%EB%B9%84%EC%A0%95%EC%83%81.png)

스택 오버플로우에도 관련된 문제가 있어서 내용을 살펴보니 사유는 다음것으로 보인다.

```
My similar issue was caused by Safari not knowing how to read the timezone in a RFC 822 time zone format.
```

이유인 즉 Safari가 RFC 822 표준 시간대 형식 중 DateType이 ('yyyy-m-dd')인 경우를 

지원하지 않기 때문입니다.

따라서 진행중인 포멧의 경우를 변경해야 합니다.

여러가지 방식들이 존재하고 그 중에서는 다음과 같은 방식으로 진행했다.

![변경된 날짜 형식](https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/0e55433434974b5c8563742d3f4d376c78bc35f0/images/22%EB%85%84/0221/%EC%A0%95%EC%83%81.png)

'yyyy/MM/dd'의 형식으로 변경하면서 해결했습니다.

라이브러리를 이용하고자 한다면 <b>moment.js</b>를 이용하는 방법도 있다.

### 참고링크 : 

<a href="https://stackoverflow.com/questions/6427204/date-parsing-in-javascript-is-different-between-safari-and-chrome">1. 스택오버플로우 관련링크</a> 

<a href="https://github.com/moment/moment">moment.js</a>

---
