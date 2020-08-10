---
layout: post
title: "Programmers-더 맵게"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 더 맵게 문제에 관한 설명입니다.<br>

---

### 문제

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다.

모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

> 섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.

Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때,

모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

* 제한 사항

scoville의 길이는 2 이상 1,000,000 이하입니다.

K는 0 이상 1,000,000,000 이하입니다.

scoville의 원소는 각각 0 이상 1,000,000 이하입니다.

모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

* 출력 형식

입출력 예 설명

스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.

* 새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
* 가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
* 새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
* 가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

```java
import java.util.PriorityQueue;

public class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;
        PriorityQueue<Integer> queue = new PriorityQueue();

        for (int aScoville : scoville) {
            queue.offer(aScoville);
        }
        while (queue.peek() < K && 1 < queue.size()) {
            queue.offer(queue.poll()+(queue.poll()*2));
            answer++;
        }
        return  K <= queue.peek() ? answer : -1;
    }
}
```

1. 가장 낮은 두 개의 음식을 섞기 위해 우선순위 큐를 사용해서 aScoville배열에서 하나씩 값을 넣습니다.

2. 가장 낮은 두 음식을  섞으면서 "가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)" 를 하기위해 <br>
한번 뽑아낸것은 그대로, 두번쨰로 뽑아낸 것은 * 2 한 것을 다시 우선순위 큐에 넣어줍시다.

3. 이 과정을 반복하면서 마지막에 남은 answer를 K와 비교해 K 이상으로 만들 수 없는 경우에는 -1을 return, 아니라면 answer를 return합시다.

<a href= "https://school.programmers.co.kr/courses/10313/lessons/63176">더 맵게</a>

---
