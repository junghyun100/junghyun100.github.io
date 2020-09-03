---
layout: post
title: "Programmers-타겟 넘버"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 타겟 넘버 문제에 관한 설명입니다.

---

## 문제

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다.

예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

```
사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 

타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

### 제한사항
   *  주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
   *  각 숫자는 1 이상 50 이하인 자연수입니다.
   *  타겟 넘버는 1 이상 1000 이하인 자연수입니다.

### 입출력 예제  

|      numbers      | target |   return | 
| :----------------:| :----: | :------: | 
| [1, 1, 1, 1, 1]	  |   3    |     5    | 



```java
public class Solution {
	static int answer = 0;
	public int solution(int[] numbers, int target) {
		recur(target, numbers, 0);
		return answer;
	}

	public void recur(int target, int[] numbers, int index) {
		if (index == numbers.length) {
			int sum = 0;
			for (int i = 0; i < numbers.length; i++) {
				sum += numbers[i];
			}
			if (sum == target)
				answer++;
			return;
		} else {
			numbers[index] *= 1;
			recur(target, numbers, index + 1);
			numbers[index] *= -1;
			recur(target, numbers, index + 1);
		}
	}
}
 ```

### 이번 문제는 재귀를 잘 활용할 줄 아는지 확인하는 문제(문제상단 *DFS/BFS라고 표시 되어있음)였습니다.

재귀 함수를 이용해 순열의 느낌으로 사용하고 안하고를 반복하다가 원하는 `target`을 만났을 때 반환하는 형식으로 했습니다.

#### 재귀문 안쪽을 봅시다.

`target`과 `numbers`, `index`값을 넘겨 줍니다.

실행 순서에 맞춰서 else 문 부터 보는게 좋다 생각합니다.

들어오자마자 `index`는 `0`으로 되어있기에 `numbers`의 `0`번째 방의 값을 처음에 `numbers[0] *= 1`을 해주고 다음 재귀를 사용합니다.

이게 쭉 반복되다가 보면 `index`가 `numbers`의 길이가 되기에 `sum`에 모든 `numbers`들의 값을 더합니다.

그렇게 되면 아래와 같은 값이 나오게 됩니다. 

```
1+1+1+1+1 = 5
```
이러면 값은 `5`로 `target`인 `3`과는 맞지 않기에 return 시키고 맨 뒤부터 하나씩 `numbers[0] *= -1`를 적용시키게 됩니다.

```
1+1+1+1-1 = 4
...(생략)
1+1-1+1-1 = 3
...(생략)
-1-1+1+1-1 = -1
```

이렇게 하나씩 적용해가면서 원하는 `target`을 찾을 떄마다  `answer`을 하나씩 늘려줍시다.

모든 경우를 다 돌았을 때 나오는 `answer`를 반환시킨다면 정답이라는 표시가 나올겁니다!

<a href= "https://programmers.co.kr/learn/courses/30/lessons/43165">타겟 넘버</a>

---
