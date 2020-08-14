---
layout: post
title: "Programmers-섬 연결하기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 섬 연결하기 문제에 관한 설명입니다.<br>

---

## 문제

n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 

최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 

예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

### 제한 사항

* 섬의 개수 n은 1 이상 100 이하입니다.
* costs의 길이는 ((n-1) * n) / 2이하입니다.
* 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
* 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
* 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
* 연결할 수 없는 섬은 주어지지 않습니다.

### 입출력 예 설명

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.

<img src="https://grepp-programmers.s3.amazonaws.com/files/production/13e2952057/f2746a8c-527c-4451-9a73-42129911fe17.png" weight = 300px; height = 300px;>

```java

import java.util.*;
class MyComparator implements Comparator<int[]>{
	@Override
	public int compare(int[] o1, int[] o2){
		return o1[2]-o2[2]; 
	}
}

class Solution {
    static int[] parent;
    public int solution(int n, int[][] costs) {
		int answer = 0;
		make(n);
    Arrays.sort(costs, new MyComparator());
    
		for( int[] cost : costs ) {	
			int from = cost[0];
			int to = cost[1];
			int weight = cost[2];
			
			if( isConnect(from,to) ) continue;
			else {
				answer+=weight;
				union(from,to);
			}
		}
		return answer;	
    }
    private static void make(int n)
    {
        parent = new int[n];
		for(int i=0; i<n; i++) {
			parent[i]=i;
		}
    }
    
    private static int Find(int index) {
		if(parent[index]==index) return index;
		return Find(parent[index]);
	}
    private static void union(int from, int to) {
		from = Find(from);
		to = Find(to);
		if( from < to ) parent[from] = to;
		else parent[to] = from;
	}

	private static boolean isConnect(int from, int to) {
		from = Find(from);
		to = Find(to);		
		return from==to;
	}
}
```

해당 문제를 풀기 위해 Kruskal Algorithm을 사용했습니다.

Kruskal Algorithm은 최소 비용 신장 부분 그래프를 찾는 알고리즘으로 이것을 위해서 Union-Find를 활용할 수 있어야 합니다.

## Disjoint Set이란

서로 중복되지 않는 부분 집합들 로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조입니다.

즉, 공통 원소가 없는, 즉 “상호 배타적” 인 서로소 집합 자료구조입니다.

이것을 표현하기 위해서 사용되는 것이 Union-Find입니다.

## Union-Find는 크게 3가지로 구성됩니다.

### 1. make-set(n)
parent배열의 인덱스번째 방에 인덱스 값을 넣어 유일한 원소로 구성된 새로운 집합을 만듭니다.

### 2.find(x)
x가 속한 집합의 대표값(루트 노드 값)을 반환합니다. 즉, x가 어떤 집합에 속해 있는지 찾는 연산

### 3.union(x, y)
x가 속한 집합과 y가 속한 집합을 합치는 연산으로 이 과정에서 Find가 활용됩니다.

### 문제해결 이어서
이후 Comparator를 이용해 가중치를 기준으로 오름차순 정렬을 합니다.

그리고 낮은 가중치부터 정점을 연결하면서 그래프를 만들어 주고,

이미 연결된 정점은 continue를 통해 넘어가고 연결할 수 있다면? 연결하면서 answer를 늘려줍시다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/42884">감시카메라</a>

---
