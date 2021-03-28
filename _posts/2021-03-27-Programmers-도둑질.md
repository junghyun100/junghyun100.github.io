---
layout: post
title: "Programmers-도둑질"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 도둑질 문제에 관한 설명입니다.

---

## 문제 설명

도둑이 어느 마을을 털 계획을 하고 있습니다. 

이 마을의 모든 집들은 아래 그림과 같이 동그랗게 배치되어 있습니다.

<img src="https://grepp-programmers.s3.amazonaws.com/files/ybm/e7dd4f51c3/a228c73d-1cbe-4d59-bb5d-833fd18d3382.png">

각 집들은 서로 인접한 집들과 방범장치가 연결되어 있기 때문에 

인접한 두 집을 털면 경보가 울립니다.

각 집에 있는 돈이 담긴 배열 money가 주어질 때,

도둑이 훔칠 수 있는 돈의 최댓값을 return 하도록 solution 함수를 작성하세요.

### 제한사항

* 이 마을에 있는 집은 3개 이상 1,000,000개 이하입니다.
* money 배열의 각 원소는 0 이상 1,000 이하인 정수입니다.

```java
class Solution {
    public int solution(int[] money) {
        int[][] dp = new int[2][money.length];
        dp[0][0]=money[0];
        dp[0][1]=money[0];
        dp[1][1]=money[1];
        for(int i=2;i<money.length;i++){
            dp[0][i]=Math.max(dp[0][i-1],dp[0][i-2]+money[i]);
            dp[1][i]=Math.max(dp[1][i-1],dp[1][i-2]+money[i]);
        }

        return Math.max(dp[0][money.length-2],dp[1][money.length-1]);
    }
}
```

# 이번 문제는 다이나믹 프로그래밍 문제였습니다.

인접한 두 집을 동시에 털면 방범 장치가 울리기 때문에 

첫 번째 집을 털껀지 아닌지에 대해서 따로 생각해 줘야합니다.

그래서 `dp배열`을 2차원으로 설정해 주었습니다.

`dp[0][n]` 은 첫 번째 집을 털었을 때 부터 마지막 집을 털지 않았을때의 최댓값을,

`dp[1][n]` 은 두 번째 집을 털었을 때 부터 마지막 집을 털었을 때의 최댓값을

저장하게 됩니다.

초깃 값으로 `0`번째 에 있는 방을 설정해주고,

`dp[1][1]`에 값을 넣어줍니다.

이후 2번 방 부터 시작을해서 이전 까지의 도둑질한 금액의 최댓값과, 

2번째 전의 방과 지금 도둑질한 금액의 합을 비교해서 큰 값을 넣습니다.

그리고 두 가지 케이스의 결과중 큰 값을 `return` 합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/42897?language=java">Programmers-도둑질</a>

---
