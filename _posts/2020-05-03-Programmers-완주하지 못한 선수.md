---
layout: post
title: "Programmers-완주하지 못한 선수.m"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 완주하지 못한 선수.m 문제에 관한 설명입니다.<br>

---

### 문제

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때,

완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항

* 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
* completion의 길이는 participant의 길이보다 1 작습니다.
* 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
* 참가자 중에는 동명이인이 있을 수 있습니다.



```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public String solution(String[] participant, String[] completion) {
        Map<String, Integer> players = new HashMap<>();
        for(String p : participant)
        {
            players.put(p,players.getOrDefault(p,0)+1);
        }
        for(String p : completion)
        {
            Integer count = players.get(p);
            if(count ==1)
            {
                players.remove(p);
            }
            else{
                players.put(p,count - 1);
            }
        }
        return players.keySet().iterator().next();
    }
}
```

### 코드 설명

HashMap을 사용합니다.

1. Map에 참가자들을 한명씩 넣어줍니다.

2. 동명이인의 경우가 있으니, 해당 이름이 한명만 완주를 했다면 remove를,<br>
 아닌경우에는 Map의 키값에 -1시켜줍니다.
 
3. return 시킵니다.

https://programmers.co.kr/learn/courses/30/parts/12077

---
