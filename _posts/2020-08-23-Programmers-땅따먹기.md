---
layout: post
title: "Programmers-땅따먹기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 땅따먹기 문제에 관한 설명입니다.<br>

---

## 문제

땅따먹기 게임을 하려고 합니다. 땅따먹기 게임의 땅(land)은 총 N행 4열로 이루어져 있고,
 
모든 칸에는 점수가 쓰여 있습니다. 
 
1행부터 땅을 밟으며 한 행씩 내려올 때, 각 행의 4칸 중 한 칸만 밟으면서 내려와야 합니다.
 
단, 땅따먹기 게임에는 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없는 특수 규칙이 있습니다.

예를 들면,

| 1 | 2 | 3 | 5 |

| 5 | 6 | 7 | 8 |

| 4 | 3 | 2 | 1 |

로 땅이 주어졌다면, 1행에서 네번째 칸 (5)를 밟았으면, 2행의 네번째 칸 (8)은 밟을 수 없습니다.

마지막 행까지 모두 내려왔을 때, 얻을 수 있는 점수의 최대값을 return하는 solution 함수를 완성해 주세요. 

위 예의 경우, 1행의 네번째 칸 (5), 2행의 세번째 칸 (7), 3행의 첫번째 칸 (4) 땅을 밟아 16점이 최고점이 되므로 16을 return 하면 됩니다.

### 제한 사항

* 행의 개수 N : 100,000 이하의 자연수
* 열의 개수는 4개이고, 땅(land)은 2차원 배열로 주어집니다.
* 점수 : 100 이하의 자연수

### 입출력 예 설명

|             land                |   answer  |
| :-----------------------------: | :-------: |
| [[1,2,3,5],[5,6,7,8],[4,3,2,1]] |     16    |

```java

class Solution {
	int solution(int[][] land) {
		int length = land.length - 1;
		int temp0, temp1, temp2, temp3;
		int[] MAX = { land[length][0], land[length][1], land[length][2], land[length][3] };

		for (int i = land.length - 2; i >= 0; i--) {
			temp0 = Math.max(Math.max(MAX[1], MAX[2]), MAX[3]) + land[i][0];
			temp1 = Math.max(Math.max(MAX[0], MAX[2]), MAX[3]) + land[i][1];
			temp2 = Math.max(Math.max(MAX[1], MAX[0]), MAX[3]) + land[i][2];
			temp3 = Math.max(Math.max(MAX[1], MAX[2]), MAX[0]) + land[i][3];

			MAX[0] = temp0;
			MAX[1] = temp1;
			MAX[2] = temp2;
			MAX[3] = temp3;
		}

		int answer = 0;
		for (int i = 0; i < 4; i++) {
			if (answer < MAX[i]) {
				answer = MAX[i];
			}
		}

		return answer;
	}

}       

 ```

### 이번 문제는 다이나믹 프로그래밍을 사용했습니다.

| 1 | 2 | 3 | 5 |       
| :---: | :---: | :---: | :---: |            
| 5 | 6 | 7 | 8 |   
| 4 | 3 | 2 | 1 |

맨 아랫 줄 부터 시작해서 값을 올라가도록 하겠습니다.

아래의 소스코드도 같이 참고합시다.

```java
temp0 = Math.max(Math.max(MAX[1], MAX[2]), MAX[3]) + land[i][0]; 
temp1 = Math.max(Math.max(MAX[0], MAX[2]), MAX[3]) + land[i][1]; 
temp2 = Math.max(Math.max(MAX[1], MAX[0]), MAX[3]) + land[i][2]; 
temp3 = Math.max(Math.max(MAX[1], MAX[2]), MAX[0]) + land[i][3]; 
```                            

`temp0,1,2,3`은 각 행마다 같은 열을 연속해서 밟을 수 없는 특수 규칙 때문에

나머지 3개의 줄의 값들을 보고 가장 큰 값을 더해서 올라가야합니다.

`Math.max`를 이용하면 다른 행의 3가지 경우의 수 중 가장 큰 수를 골라서 더 할 수 있습니다.

```java
MAX[0] = temp0;
MAX[1] = temp1;
MAX[2] = temp2; 
MAX[3] = temp3; 
```            

이후에 각 행에서 시작된 `MAX`을 쭉 타고 올라가기 위해 더했던 `temp`값을 `MAX`로 갱신해줍시다.

해당 `for`문은 맨 아랫줄에서 하나씩 올라와 첫번째 줄까지 올라오기 때문에

반복문이 끝나게 되면 `MAX[]`에는 각 행에서 시작해 더해진 최댓값들이 모여있습니다.

그 중 가장 큰 값을 `return` 시키면 정답처리가 됩니다!

<a href= "https://programmers.co.kr/learn/courses/30/lessons/12913">땅따먹기</a>

---
