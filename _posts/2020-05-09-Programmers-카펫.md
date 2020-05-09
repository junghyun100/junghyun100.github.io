---
layout: post
title: "Programmers-카펫"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 카펫 문제에 관한 설명입니다.<br>

---

### 문제

Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 빨간색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

image.png

Leo는 집으로 돌아와서 아까 본 카펫의 빨간색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 빨간색 격자의 수 red가 매개변수로 주어질 때 카펫의 가로,

세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

제한사항
* 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
* 빨간색 격자의 수 red는 1 이상 2,000,000 이하인 자연수입니다.
* 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.


```java
import java.util.*;
class Solution {
    public int[] solution(int brown, int red) {
        int[] answer = new int[2];
        int maxSize = 5000;
        for(int N = 3; N <= maxSize; N++){
            for(int M = 3; M <=maxSize; M++)
            {
                if((N*M) == (brown + red)&& red ==(N-2)*(M-2))
                {
                    answer[0] = N;
                    answer[1] = M;
                    break;
                }
            }
        }
        return answer;
    }
}
```
1. maxSize = 5000라고 설정한 이유는 갈색길이가 가로의 길이라고 생각했을 때 빨강색은 넘지 못하기에 최대값으로 설정해놓았습니다.

> 여기서 문제점이라 생각되는 것은 가로 자체가 5000까지 가는가? 에 대해서 해결하지 못했다는 점 입니다. 조금 더 고민해보자

2.  N, M 이 2개의 값을 3부터 시작하는 이유 = 빨강색이 1이상이기 위해서는 최소 가로가 3 세로가 3이상을 만족해야 하기 때문입니다.

3. if((N*M) == (brown + red)&& red ==(N-2)*(M-2))의 조건식은<br>
조건문은 가로 세로의 곱 = 빨강과 갈색의 합과 같고, 빨간색은 가로와 세로의 겉면(2칸씩)을 제외한 값이 되어야 하기 때문입니다.

https://programmers.co.kr/learn/courses/30/lessons/42842

---
