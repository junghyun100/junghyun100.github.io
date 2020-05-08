---
layout: post
title: "Programmers-기능개발"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 기능개발 문제에 관한 설명입니다.<br>

---

### 문제

트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다

. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다.

트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.

※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다.

무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다.

이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.


### 제한사항

* bridge_length는 1 이상 10,000 이하입니다.
* weight는 1 이상 10,000 이하입니다.
* truck_weights의 길이는 1 이상 10,000 이하입니다.
* 모든 트럭의 무게는 1 이상 weight 이하입니다.


```java
import java.util.LinkedList;
import java.util.Queue;
 
public class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        Queue<Truck> trucks = new LinkedList<>();
        Queue<Truck> onBridge = new LinkedList<>();
 
        for (int w : truck_weights) {
            Truck truck = new Truck();
            truck.weight = w;
            truck.position = 0;
            trucks.add(truck);
        }
 
        int sec = 0;
 
        while (!trucks.isEmpty() || !onBridge.isEmpty()) {
            sec++;
 
            Truck doneTruck = null;
            int sum = 0;
 
            for (Truck truck : onBridge) {
                sum += truck.weight;
                truck.position++;
 
                if (truck.position > bridge_length) {
                    doneTruck = truck;
                }
            }
 
            if (doneTruck != null) {
                onBridge.remove(doneTruck);
                sum -= doneTruck.weight;
            }
 
            if (!trucks.isEmpty() && (onBridge.size() < bridge_length)) {
                Truck truck = trucks.peek();
 
                if (truck.weight + sum <= weight) {
                    trucks.remove(truck);
                    onBridge.add(truck);
                    truck.position++;
                }
            }
        }
 
        return sec;
    }
 
    private class Truck {
        int weight;
        int position;
    }
}

```
1. Truck class 를 이용합니다.

2.  다리를 건너는 중의 트럭을 넣는 큐와 다 건넌 트럭을 넣는 ArrayList를 하나 만들었습니다.

3. while문을 만들고 ArrayList에 모든 트럭이 들어오면 종료시킵니다.

https://programmers.co.kr/learn/courses/30/lessons/42583

---
