---
layout: post
title: "Programmers-징검다리"
tags: [Programmers]
comments: true
---

위 문제는 Programmers 징검다리 문제에 관한 설명입니다.

---

## 문제 설명

출발지점부터 distance만큼 떨어진 곳에 도착지점이 있습니다.

그리고 그사이에는 바위들이 놓여있습니다. 바위 중 몇 개를 제거하려고 합니다.

예를 들어, 도착지점이 25만큼 떨어져 있고,

바위가 [2, 14, 11, 21, 17] 지점에 놓여있을 때 바위 2개를 제거하면

출발지점, 도착지점, 바위 간의 거리가 아래와 같습니다.

| 제거한 바위의 위치 | 각 바위 사이의 거리 | 거리의 최솟값 |
| :----------------: | :-----------------: | :-----------: |
|      [21, 17]      |    [2, 9, 3, 11]    |       2       |
|      [2, 21]       |    [11, 3, 3, 8]    |       3       |
|      [2, 11]       |    [14, 3, 4, 4]    |       3       |
|      [11, 21]      |    [2, 12, 3, 8]    |       2       |
|      [2, 14]       |    [11, 6, 4, 4]    |       4       |

위에서 구한 거리의 최솟값 중에 가장 큰 값은 4입니다.

출발지점부터 도착지점까지의 거리 `distance`,

바위들이 있는 위치를 담은 배열 `rocks`,

제거할 바위의 수 `n`이 매개변수로 주어질 때,

바위를 n개 제거한 뒤 각 지점 사이의 거리의 최솟값 중에 가장 큰 값을

return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 도착지점까지의 거리 distance는 1 이상 1,000,000,000 이하입니다.
- 바위는 1개 이상 50,000개 이하가 있습니다.
- n 은 1 이상 바위의 개수 이하입니다.

```java
import java.util.*;
class Solution {
    public int solution(int distance, int[] rocks, int n) {
         int answer = 0;
         int left=1;
         int mid = 0;
         int right=distance;
         int remove=0;
         int lastRock=0;
         Arrays.sort(rocks);
         while(left <= right){
             mid = (left + right) / 2;
             for(int i = 0 ;i < rocks.length ; i++){
                 if(mid > rocks[i] - lastRock) remove++;
                 else lastRock = rocks[i];
             }
             if(distance - lastRock < mid) remove++;
             if(remove > n) right = mid - 1;
             else{
                 answer = Math.max(answer, mid);
                 left = mid + 1;
             }
             remove = 0;
             lastRock = 0;
         }
         return answer;
    }
}
```

# 이번 문제는 이분 탐색 문제였습니다.

필요한 변수들을 만들어주고, 이분탐색을 하기 위해 정렬을 해줬습니다.

바위 사이의 최소 거리를 mid로 정해둡니다.

만약 비교를 하다가 이것보다 작은걸 발견하면 없애줍니다.

발견하지 못한다면 `lastRock` 마지막인 바위값을 저장합니다.

마지막 바위와 도착지까지의 거리도 `mid`와 비교해야하므로

제거한 바위 갯수 `remove`가 `n`보다 크다면

거리 `mid`가 크다는 뜻이므로 줄이고, 반대라면 늘립니다.

`answer`에는 `remove` < = n일때 최댓값을 저장합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/43236?language=java">Programmers-징검다리</a>

---
