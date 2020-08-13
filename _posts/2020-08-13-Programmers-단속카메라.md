---
layout: post
title: "Programmers-단속카메라"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 단속카메라 문제에 관한 설명입니다.<br>

---

## 문제

고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 

모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.

### 제한 사항

* 차량의 대수는 1대 이상 10,000대 이하입니다.
* routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점,routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
* 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
* 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

```java
import java.util.Arrays;
import java.util.Comparator;
 
class Solution {
    public int solution(int[][] routes) {
        int answer = 0;
        Arrays.sort(routes, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] - o2[0];
            }
        });
        
        int MAX = Integer.MIN_VALUE;
        int MIN = Integer.MAX_VALUE;
 
        for (int i = 0; i < routes.length; i++) {
            if (routes[i][1] < MIN || MAX < routes[i][0]) {
                answer++;
                MIN = routes[i][0];
                MAX = routes[i][1];
            } else {
                if(MIN < routes[i][0])
                    MIN = routes[i][0];
                if(routes[i][1] < MAX)
                    MAX = routes[i][1];
            }
        }
        return answer;
    }
}

```

1. 차가 들어온 순간을 기준으로 Comparaotr를 이용해 정렬합니다. (뒤에 익명 클래스를 활용해 바로!) 

2. 처음차량이 반복문의 else로 들어가 시작 시간을 MIN값, 끝 시간을 MAX로 정해서 반복문을 시작해 봅시다.

3. 그런데 다른 차량의 끝값이 여태까지의 MIN값(가장 먼저 시작한 시작)값 보다 이르거나, 다른 차량의 끝값이 MAX값(가장 먼저 시작한 시작)값보다 크다면?

4. 현재 설치된 카메라 갯수보다 1개를 더 설치해야 감시가 가능합니다.


<a href= "https://programmers.co.kr/learn/courses/30/lessons/42884">감시카메라</a>

---
