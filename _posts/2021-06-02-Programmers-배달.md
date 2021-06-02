---
layout: post
title: "Programmers-배달"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 배달 문제에 관한 설명입니다.

---

## 문제 설명

N개의 마을로 이루어진 나라가 있습니다. 

이 나라의 각 마을에는 1부터 N까지의 번호가 각각 하나씩 부여되어 있습니다. 

각 마을은 양방향으로 통행할 수 있는 도로로 연결되어 있는데, 

서로 다른 마을 간에 이동할 때는 이 도로를 지나야 합니다. 

도로를 지날 때 걸리는 시간은 도로별로 다릅니다.

현재 1번 마을에 있는 음식점에서 각 마을로 음식 배달을 하려고 합니다. 

각 마을로부터 음식 주문을 받으려고 하는데, 

N개의 마을 중에서 K 시간 이하로 배달이 가능한 마을에서만 주문을 받으려고 합니다. 

다음은 N = 5, K = 3인 경우의 예시입니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d7779d88-084c-4ffa-ae9f-2a42f97d3bbf/%E1%84%87%E1%85%A2%E1%84%83%E1%85%A1%E1%86%AF_1_uxun8t.png">{: .center-image}

위 그림에서 1번 마을에 있는 음식점은 [1, 2, 4, 5] 번 마을까지는 3 이하의 시간에 배달할 수 있습니다. 

그러나 3번 마을까지는 3시간 이내로 배달할 수 있는 경로가 없으므로 

3번 마을에서는 주문을 받지 않습니다. 

따라서 1번 마을에 있는 음식점이 배달 주문을 받을 수 있는 마을은 4개가 됩니다.

마을의 개수 N, 각 마을을 연결하는 도로의 정보 road, 

음식 배달이 가능한 시간 K가 매개변수로 주어질 때, 

음식 주문을 받을 수 있는 마을의 개수를 return 하도록 solution 함수를 완성해주세요.

### 제한사항

* 마을의 개수 N은 1 이상 50 이하의 자연수입니다.
* road의 길이(도로 정보의 개수)는 1 이상 2,000 이하입니다.
* road의 각 원소는 마을을 연결하고 있는 각 도로의 정보를 나타냅니다.
* road는 길이가 3인 배열이며, 순서대로 (a, b, c)를 나타냅니다.
	* a, b(1 ≤ a, b ≤ N, a != b)는 도로가 연결하는 두 마을의 번호이며, c(1 ≤ c ≤ 10,000, c는 자연수)는 도로를 지나는데 걸리는 시간입니다.
	* 두 마을 a, b를 연결하는 도로는 여러 개가 있을 수 있습니다.
	* 한 도로의 정보가 여러 번 중복해서 주어지지 않습니다.
* K는 음식 배달이 가능한 시간을 나타내며, 1 이상 500,000 이하입니다.
* 임의의 두 마을간에 항상 이동 가능한 경로가 존재합니다.
* 1번 마을에 있는 음식점이 K 이하의 시간에 배달이 가능한 마을의 개수를 return 하면 됩니다.

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
    private static ArrayList<Edge>[] edgeList;
    private static int[] distance;
    public int solution(int N, int[][] road, int K) {
        int answer = 0;
        edgeList = new ArrayList[N + 1];
        distance = new int[N + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);
        for (int i = 1; i <= N; i++) {
            edgeList[i] = new ArrayList<>();
        }
        for (int i = 0; i < road.length; i++) {
            edgeList[road[i][0]].add(new Edge(road[i][1], road[i][2]));
            edgeList[road[i][1]].add(new Edge(road[i][0], road[i][2]));
        }
        distance[1] = 0; 
        dijkstra();
        for (int cost : distance) {
            if (cost <= K) {
                answer++;
            }
        }
        return answer;
    }

    private static void dijkstra() {
        PriorityQueue<Edge> queue = new PriorityQueue<>();
        queue.add(new Edge(1, 0));
        while (!queue.isEmpty()) {
            Edge edge = queue.poll();
            int town = edge.town;
            int deliveryTime = edge.deliveryTime;
            if (distance[town] < deliveryTime) {
                continue;
            }
            for (int i = 0; i < edgeList[town].size(); i++) { 
                int town2 = edgeList[town].get(i).town;
                int deliveryTime2 = edgeList[town].get(i).deliveryTime + deliveryTime;
                if (distance[town2] > deliveryTime2) { 
                    distance[town2] = deliveryTime2;
                    queue.add(new Edge(town2, deliveryTime2));
                }
            }
        }
    }
    
    private static class Edge implements Comparable<Edge> {
        int town; 
        int deliveryTime;        
        public Edge(int town, int deliveryTime) {
            this.town = town;
            this.deliveryTime = deliveryTime;
        }

        @Override
        public int compareTo(Edge o) {
            return deliveryTime - o.deliveryTime;
        }
    }
}
```

# 이번 문제는 다익스트라 알고리즘을 사용해야 합니다.

플로이드 와샬이나 다익스트라를 사용하면 풀 수 있는 문제이다.

플로이드 와샬과 다익스트라에 관해서는 추후에 다시 한번 정리해서 포스팅 할 예정이다.

문제의 과정은 해당 도시와 연결되어 있는 도시들을 탐색하고,

그 중에서 최단경로를 세팅하면서 queue에다가 넣어준다.

queue는 PQ이므로 가중치(시간)을 기준으로 알아서 정렬이 된다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/12978">Programmers-배달</a>

---
