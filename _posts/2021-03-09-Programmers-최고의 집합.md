---
layout: post
title: "Programmers-최고의 집합"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 최고의 집합 문제에 관한 설명입니다.

---

## 문제 설명

자연수 n 개로 이루어진 중복 집합(multi set, 편의상 이후에는 "집합"으로 통칭) 중에

다음 두 조건을 만족하는 집합을 최고의 집합이라고 합니다.

1. 각 원소의 합이 S가 되는 수의 집합
2. 위 조건을 만족하면서 각 원소의 곱 이 최대가 되는 집합

예를 들어서 자연수 2개로 이루어진 집합 중 합이 9가 되는 집합은 

다음과 같이 4개가 있습니다.

{ 1, 8 }, { 2, 7 }, { 3, 6 }, { 4, 5 }

그중 각 원소의 곱이 최대인 { 4, 5 }가 최고의 집합입니다.

집합의 원소의 개수 n과 모든 원소들의 합 s가 매개변수로 주어질 때, 

최고의 집합을 return 하는 solution 함수를 완성해주세요.

### 제한 사항

* 최고의 집합은 오름차순으로 정렬된 1차원 배열(list, vector) 로 return 해주세요.

만약 최고의 집합이 존재하지 않는 경우에 크기가 1인 1차원 배열(list, vector) 에

-1 을 채워서 return 해주세요.

자연수의 개수 n은 1 이상 10,000 이하의 자연수입니다.

모든 원소들의 합 s는 1 이상, 100,000,000 이하의 자연수입니다.

### 입출력 예 설명

입출력 예 #1

|n|s|result|
|:----:|:----:|:-----:|
|2|9|[4, 5]|
|2|1|[-1]|
|2|8|[4, 4]|

```java
import java.util.Arrays;

class Solution {
    public int[] solution(int n, int s) {
	int[] answer = new int[n];
	for(int i=0; i<n; i++)
	  answer[i] = s/n; 
	for(int i=0; i<s%n; i++)
	  answer[i] = answer[i] +1;
	Arrays.sort(answer);
	if(n>s)
	  return new int[] {-1};
	return answer;
    }
}
```

### 이번 문제는 수학적이었습니다.

문제의 조건을 살펴봅시다.

곱이 최대인 원소를 리턴해줘야 합니다.

그래서 주어진 s의 합을 n개로 나타낼때, 

일단 s가 n으로 나누어 떨어지는 수로 나누고, 나머지 만큼 값을 더해줍니다.

예시로 s가 15이고, n이 4인 경우를 생각해봅니다.

그렇다면 s/n은 3이 나오게 되므로 answer 배열에는 3의 값들이 저장됩니다.

> [3, 3, 3, 3]
 
이제 나머지 값이 있는가 살펴봅니다.

15%4 = 3의 값이나오게 되므로, 

배열들의 값에 나머지 값의 갯수만큼 +1을 해줍니다.

> [4, 4, 4, 3]

이제 Arrays.sort해줍니다.

> [3, 4, 4, 4]

n의 값이 s보다 큰 경우는 존재하지 않으므로 -1의 값을 채워 넣습니다.

그리고 return 해주면 됩니다.
<a href= "https://programmers.co.kr/learn/courses/30/lessons/12938?language=java">Programmers-최고의 집합</a>

---
