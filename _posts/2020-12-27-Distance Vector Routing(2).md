---
layout: post
title: "네트워크 - Distance Vector Routing(2)"
tags: [컴퓨터 네트워크]
comments: true

---

컴퓨터 네트워크를 공부하면서 정리를 한 내용들 입니다.

-참고 K-mooc 부산 대학교 유영환 교수님 : 컴퓨터 네트워크 강의

---

# Note : "Good News Travels Fast"

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1227/Good%20News%20Travels%20Fast.PNG">

<strong>Distance vector algorithm</strong>에서 <strong>good news가 빨리 전달 된다</strong>는 장점이 있습니다.

어떻게 good news는 빨리 전달 되는가 살펴 보겠습니다.

그림을 보시면 `x`, `y`와 `z`가 각각 연결 되어 있고, 

`z`는 `distance vector`를 주고 받으면서 

처음에 `y`에서 `x`로의 값이 1로 바뀌기 전, 

4, 1과 3일 때에는 `z`에서 `x`측으로 가기 위해 `y`를 거쳐가면 5의 비용이 들기 때문에

`x`로 갈 때 3의 비용으로 바로 직접 연결을 했습니다.

그런데 값이 4에서 1로 줄었다면?

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1227/Good%20News%20Travels%20Fast2.PNG">

그러면 `z`는 3의 비용으로 직접 `x`로 전달 하는 것 보다는

`y`로 거쳐 갈 때 1+1=2의 비용으로 갈 수 있다는 것을 알게 됩니다.

그래서 바로 `y`를 거쳐서 전달 하게끔 바뀌게 됩니다.

이렇게 해서 비용이 4에서 1로 줄었다는 것은 금방 알려지게 된다는 것 입니다.

# Note : "Bad News Travels Slow"

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1227/Bad%20News%20Travels%20Slow.PNG">

<strong>Distance vector algorithm</strong>에서 문제가 될 수 있는 것은 <strong>Bad news</strong>입니다. 

현재 그림에선 `x`, `y`, `z`가 연결 되어 있고 비용이 1과 4와 50으로 되어 있었습니다.

그래서 `z`는 원래 `y`로 보낼 때 `y`로 보내고 있었고 

`x`로 보낼 때도 `y`로 거치는 것이 더 비용이 적어 그쪽으로 보내고 있었습니다. 

여기서 `x`에서 `y`로 가는 비용이 4에서 60으로 증가 했습니다.

`y`가 60으로 되었다는 것을 감지 했습니다.

`y`의 입장에서 `z`가 보내온 케이블을 보니

`z`가 5의 비용으로 갈 수 있다고 말해옵니다.

사실 `z`는 5의 비용으로 갈 수 있다는 것은 값이 바뀌기 전의 거치는 비용 5입니다.

`y`는 그 사실을 모르기 때문에 `z`를 통해 전송을 하게 됩니다.

`Dz(x)` 이 값이 5이므로, `C(y,z)+ Dz(x)`는 6이 됩니다.

그리고 테이블을 이렇게 `(y,x)` 비용을 6이라고 바꿔서 `z`한테 전달 합니다.

그럼 `z`는 아까는 `y`가 4의 비용으로 갈 수 있다고 했는데, 

이번에는 6의 비용으로 갈 수 있다고 하니 여전히 50보다 작기 때문에 

값을 7로 갱신 해서 다시 `y`와 주고 받게 됩니다.

결론적으로, `z`는 `y`를 통해서 보내고 `y`는 `z`를 통해서 보내는 것인데 

비용 값이 계속 서로 1씩 증가 한 값을 계속 주고 받게 됩니다.

처음 `y`가 `z`로 보낸 첫 테이블의 값, 처음 6에서 7, 8, 9, 10 왔다 갔다 하면서

하나씩 증가 하다가 값이 50이 되면 `z`에서 `x`측으로 보내는 방식이 더 효율적일 때.

그 때 가서야 `z`는 `y`를 거쳐 보내지 말고 보내야한다는 사실을 알게 됩니다.

이렇게 계속 증가하는 문제를 <strong>count to infinity problem</strong>라고 이야기 합니다.

그리고 보통 `Distance vector`를 주고 받는 주기가 설정 마다 다르지만 

30초 정도로 보통 많이 설정 됩니다.

그럼 6에서 50까지 44회를 주고 받으려면 약 20분 정도 시간이 걸리게 됩니다. 

그래서 이런 문제를 완화하기 위해서 <strong>poisened reverse</strong>라는 제한이 생겼습니다.

예를 들어서 `z`가 `x`로 가는 길이 `y`를 통해서 간다고 하면 

`z`는 `y`에게 자신이 `x`로 가는 비용은 무한대이라고 알려줍니다.

여기서의 문제는 `z`는 `y`를 거쳐가면서 `y`한테 말하기를

자기는 5의 비용으로 갈 수 있다 이야기 했던 점 입니다.

그럼 `y`는 `z`가 5의 비용으로 가는 길이 있다고 하니까

거기에다 1을 더하면 6으로 갈 수 있다고 판단합니다. 

차라리 `z`가 `y`한테 `y`를 거쳐서 보내면서 `y`한테 말 할 때는

`x`로의 길이 없다 또는 무한대다 이렇게 말했다면 `y`는 이 길을 선택하려 하지 않습니다.

그렇다고 해서 이것이 완벽한 해결책은 되지 않습니다. 

직접 이렇게 연결 된 두 node 사이에 일어 났을 때는 해결책이 될 수도 있겠지만

만약에 이 사이에서 loop이 일어났다. 여러 개의 node가 서로 원형으로 loop을 만들었다 할 때는

이런 식으로 해결이 안 되기 때문에 완벽한 해결책이 되지는 못합니다.

---
