---
layout: post
title: "Programmers-프린터"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 프린터 문제에 관한 설명입니다.<br>

---

### 문제

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 

그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 

이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 

이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

* 1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
* 2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
* 3. 그렇지 않으면 J를 인쇄합니다.

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 

현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

### 제한 조건

현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.

인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.

location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0,

두 번째에 있으면 1로 표현합니다.

```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 0;
        PriorityQueue<Integer> PriorityQ = new PriorityQueue<>(Collections.reverseOrder());
        //기존의 priority queue는 최소값 기준으로 우선순위가 되어 정렬이되는데 collections 리버스를 하면 최대값 기준으로 우선순위가 됨
        for(int i : priorities)//각 값들을 입력하는데 이때 리버스가 적용된 priority queue이므로 최대값 기준으로 들어가게됨 
            {
                PriorityQ.offer(i);
            }
        while(!PriorityQ.isEmpty())//입력이 다되고 나서 빌 때까지 반복을 합니다.
        {
            for(int i =0; i< priorities.length;i++)
            {
                if(PriorityQ.peek()==priorities[i]){//맨 끝에값이 우선순위와 같다면?
                    PriorityQ.poll();//꺼내고나서 answer의 값을올려주면서 
                    answer++;
                    if(location ==i){// 이떄 우리가 찾는 location과 i번째의 값이 같다면 더이상 할 필요가 없으므로 break;
                        PriorityQ.clear();
                        break;
                    }
                }
            }
        }
        return answer;
    }
}
```

설명은 주석을 참고할 것

<a href= "https://programmers.co.kr/learn/courses/30/lessons/42587">프린터</a>

---
