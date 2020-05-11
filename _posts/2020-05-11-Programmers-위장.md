---
layout: post
title: "Programmers-기능개발"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 기능개발 문제에 관한 설명입니다.<br>

---

### 문제

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 

다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

* clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
* 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
* 같은 이름을 가진 의상은 존재하지 않습니다.
* clothes의 모든 원소는 문자열로 이루어져 있습니다.
* 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
* 스파이는 하루에 최소 한 개의 의상은 입습니다.


```java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < clothes.length; i++)
            map.put(clothes[i][1], map.getOrDefault(clothes[i][1], 0) + 1);
        for (int value : map.values())
            answer *= (value + 1);
        return answer - 1;
    }
}

```
Map을 이용했습니다.

1.의상의 종류를 key, 동일한 의상의 종류의 수를 value로 map에 담습니다.

2.해시를 순회하며 저장된 (키, 값) 쌍으로 된 엔트리를 가져옵니다.

3.값에 + 1을 더한 후, answer와 곱해줍니다.

4.순회를 마치면 answer에 1을 빼줍니다.

https://programmers.co.kr/learn/courses/30/lessons/42578

---
