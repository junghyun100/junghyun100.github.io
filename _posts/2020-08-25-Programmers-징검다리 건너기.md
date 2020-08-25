---
layout: post
title: "Programmers-징검다리 건너기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 징검다리 건너기 문제에 관한 설명입니다.<br>

---

## 문제

[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

카카오 초등학교의 니니즈 친구들이 라이언 선생님과 함께 가을 소풍을 가는 중에
 
 징검다리가 있는 개울을 만나서 건너편으로 건너려고 합니다. 
 
 라이언 선생님은 니니즈 친구들이 무사히 징검다리를 건널 수 있도록 
 
 다음과 같이 규칙을 만들었습니다.

1. 징검다리는 일렬로 놓여 있고 각 징검다리의 디딤돌에는 모두 숫자가 <br>
   적혀 있으며 디딤돌의 숫자는 한 번 밟을 때마다 1씩 줄어듭니다.
2. 디딤돌의 숫자가 0이 되면 더 이상 밟을 수 없으며 <br>
이때는 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
3. 단, 다음으로 밟을 수 있는 디딤돌이 여러 개인 경우 <br>
   무조건 가장 가까운 디딤돌로만 건너뛸 수 있습니다.
4. 니니즈 친구들은 개울의 왼쪽에 있으며, 개울의 오른쪽 건너편에 도착해야 <br>
징검다리를 건넌 것으로 인정합니다.
5. 니니즈 친구들은 한 번에 한 명씩 징검다리를 건너야 하며, 
한 친구가 징검다리를 모두 건넌 후에 그 다음 친구가 건너기 시작합니다.

디딤돌에 적힌 숫자가 순서대로 담긴 배열 stones와 
한 번에 건너뛸 수 있는 디딤돌의 최대 칸수 k가 매개변수로 주어질 때, 
최대 몇 명까지 징검다리를 건널 수 있는지 return 하도록 solution 함수를 완성해주세요.

### 제한 사항
   * 징검다리를 건너야 하는 니니즈 친구들의 수는 무제한 이라고 간주합니다.
   * stones 배열의 크기는 1 이상 200,000 이하입니다.
   * stones 배열 각 원소들의 값은 1 이상 200,000,000 이하인 자연수입니다.
   * k는 1 이상 stones의 길이 이하인 자연수입니다.

### 입출력 예 #1

첫 번째 친구는 다음과 같이 징검다리를 건널 수 있습니다.
<img src ="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4560e242-cf83-4e77-a14c-174f3831499d/step_stones_104.png">

첫 번째 친구가 징검다리를 건넌 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.

두 번째 친구도 아래 그림과 같이 징검다리를 건널 수 있습니다.    

<img src ="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d64f29ac-3e35-4fd3-91fa-4d70e3b6c80a/step_stones_101.png">

두 번째 친구가 징검다리를 건넌 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.

세 번째 친구도 아래 그림과 같이 징검다리를 건널 수 있습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/369bc8a1-7017-4135-a499-505247ab9cfc/step_stones_102.png">

세 번째 친구가 징검다리를 건넌 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.

네 번째 친구가 징검다리를 건너려면, 세 번째 디딤돌에서 일곱 번째 디딤돌로 

네 칸을 건너뛰어야 합니다. 하지만 k = 3 이므로 건너뛸 수 없습니다.

<img src ="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e44e0a83-e637-48ad-858c-4c135c3b078f/step_stones_103.png">

따라서 최대 3명이 디딤돌을 모두 건널 수 있습니다.

```java

public class Solution {

    public int solution(int[] stones, int k) {
        int answer = Integer.MAX_VALUE;
        for (int i = 0; i <= stones.length-k;) {
            int MAX = stones[i];
            int index = 0;
            for (int j = i+1; j < i+k; j++) {
                if (stones[j] >= MAX) {
                    index = j;
                    MAX = stones[j];
                }
            }
            if (index == 0) i++;
            else {
                i = (index+1);
            }
            answer = Math.min(answer,MAX);
        }
        return answer;
    }
}

 ```

### 이번 문제는 시뮬레이션으로 생각합니다.
 
`answer`은 구간 MAX의 값들 중 가장 작은 값이 출력되어야 합니다.

추후 비교를 할 것이기에 `Integer.MAX_VALUE`를 사용해 일단 최대치로 바꿔놓읍시다.

이후 `징검다리 갯수 - K` 만큼 반복하도록 분기문을 만듭시다.

처음 들어가게 되면 그 값을 `MAX`에 넣어줍시다.

그리고 `K` 만큼 반복하는 분기문을 만듭시다.

현재 구간의 징검다리가 구간의 최댓값보다 크거나 같을 때 구간의 최대값을 바꿔줘야겠죠?

K번 반복을 했음에도 구간의 최댓값이 변경되지 않았을 때는 한칸만 이동,

구간의 최댓값이 변경되었을 때는 최댓값에 해당하는 `징검다리 번호+1`로 이동합니다.

구간의 최댓값이 다른 구간과 비교했을 때, 더 작은 구간의 최댓값을 결과값에 설정합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/64062">징검다리 건너기</a>

---
