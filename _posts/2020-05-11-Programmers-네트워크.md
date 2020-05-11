---
layout: post
title: "Programmers-네트워크"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 네트워크 문제에 관한 설명입니다.<br>

---

### 문제

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다.

예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 

컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때,

네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

### 제한사항

* 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
* 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
* i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
* computer[i][i]는 항상 1입니다.


```java
public class Solution {
    static boolean link[][];

    public int solution(int n, int[][] computers) {
        int answer = 0;
        link = new boolean[n][n];
        for (int i = 0; i < computers.length; i++) {
            if (!link[i][i]) {
                answer++;
                dfs(computers, i, n);
            }
        }
        return answer;
    }

    public static void dfs(int[][] computers, int index, int n) {
        for (int i = 0; i < n; i++) {
            if (computers[index][i] == 1 && !link[index][i]) {
                link[index][i] = link[i][index] = true;
                dfs(computers, i, n);
            }
        }
    }
}
```
2차원배열과 재귀함수방식으로 풀었습니다.

1. computer[i][i]는 항상 1입니다.

- 따라서 기준점을 computer[i][i]로 잡아서 하나씩 검사하는 방식을 택했습니다.

2. 탐색하면서 탐색 기준 노드가 바뀔 때 마다 answer을 증가시킵니다.

3.연결된 적이 없을 경우, 지금 노드와 연결된 다른 노드를 찾기 위해 재귀함수를 호출합니다.

https://programmers.co.kr/learn/courses/30/lessons/43162

---
