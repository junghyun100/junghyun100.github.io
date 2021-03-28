---
layout: post
title: "Programmers-멀리뛰기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 멀리뛰기 문제에 관한 설명입니다.

---

## 문제 설명

효진이는 멀리 뛰기를 연습하고 있습니다. 

효진이는 한번에 1칸, 또는 2칸을 뛸 수 있습니다. 

칸이 총 4개 있을 때, 효진이는
```
(1칸, 1칸, 1칸, 1칸)
(1칸, 2칸, 1칸)
(1칸, 1칸, 2칸)
(2칸, 1칸, 1칸)
(2칸, 2칸)
```
의 5가지 방법으로 맨 끝 칸에 도달할 수 있습니다. 

멀리뛰기에 사용될 칸의 수 n이 주어질 때, 효진이가 끝에 도달하는 방법이 몇 가지인지 알아내, 

여기에 1234567를 나눈 나머지를 리턴하는 함수, solution을 완성하세요. 

예를 들어 4가 입력된다면, 5를 return하면 됩니다.

### 제한사항

* n은 1 이상, 2000 이하인 정수입니다.

```java
class Solution {
    public long solution(int n) {
        long dp[] = new long[2001];
		dp[1] = 1;
		dp[2] = 2;
		      
		for(int i=3;i<=2000; i++){
		    dp[i] = (dp[i-1] + dp[i-2]) %1234567;
		}
	    return dp[n];
    }
}
```

# 이번 문제는 다이나믹 프로그래밍 문제였습니다.

문제를 풀고나서 알게 된건 이게 피보나치 문제와 같다는 사실입니다.

> 피보나치 수열은 상당히 단순한 단조 증가(monotonically increasing) 수열로, <br>0번째 항은 0, 1번째 항은 1, 그 외 항은 전번, 전전번 항의 합으로 표현됩니다.

그리고 더 할때 마다 1234567이라는 숫자로 나누라는 조건을 적용시킵니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/12914">Programmers-멀리뛰기</a>

---
