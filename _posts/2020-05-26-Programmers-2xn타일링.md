---
layout: post
title: "Programmers-2xn타일링"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 2xn타일링 문제에 관한 설명입니다.<br>

---

### 문제

가로 길이가 2이고 세로의 길이가 1인 직사각형모양의 타일이 있습니다. 이 직사각형 타일을 이용하여 세로의 길이가 2이고 가로의 길이가 n인 바닥을 가득 채우려고 합니다. 타일을 채울 때는 다음과 같이 2가지 방법이 있습니다.

타일을 가로로 배치 하는 경우

타일을 세로로 배치 하는 경우

예를들어서 n이 7인 직사각형은 다음과 같이 채울 수 있습니다.

<img src="https://i.imgur.com/29ANX0f.png">

직사각형의 가로의 길이 n이 매개변수로 주어질 때, 이 직사각형을 채우는 방법의 수를 return 하는 solution 함수를 완성해주세요.
### 제한 조건
*가로의 길이 n은 60,000이하의 자연수 입니다.
* 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요


```java
public class Solution {
    public int solution(int n) {
        int [] tile = new int[n+1];
        tile[0] = 1;
        if(n > 1)
            tile[1]=2;
         for(int i =2 ; i<=n;i++)
             tile[i] = (tile[i-1] + tile[i-2])%1000000007;
        return tile[n-1];
    }
} 

```
동적계획법을 통해 문제에 접근했습니다.

식 n-1 + n-2를 사용해주면서 풀어냈습니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/12900">2xn 타일링</a>

---
