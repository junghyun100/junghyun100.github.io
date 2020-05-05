---
layout: post
title: "Programmers-기능개발"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 기능개발 문제에 관한 설명입니다.<br>

---

### 문제

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다.

각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고,

이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 

각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 

return 하도록 solution 함수를 완성하세요.

### 제한사항

* 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
* 작업 진도는 100 미만의 자연수입니다.
* 작업 속도는 100 이하의 자연수입니다.
* 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 

예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

```java
import java.util.ArrayList;

class Solution{    
    public int[] solution(int[] progresses, int[] speeds) {
        ArrayList<Integer> answerList = new ArrayList<>();
        ArrayList<Integer> arrList = new ArrayList<>();

        for (int i = 0; i < progresses.length; i++) {
            int count = 0;
            while(true){
                progresses[i] += speeds[i];
                count++;
                
                if(progresses[i] >= 100){
                    arrList.add(count);
                    break;
                }    
            }
        }

        int count = 1;
        int j=0;
        while(true){
            if(arrList.get(j) >= arrList.get(j+1)){
                arrList.remove(j+1);
                count++;
            }
            else{
                j++;
                answerList.add(count);
                count = 1;
            }

            if(arrList.size() -1 == answerList.size()){
                answerList.add(count);
                break;
            }
        }

        int[] answer = new int[answerList.size()];
        for (int i = 0; i < answerList.size(); i++) {
            answer[i] = answerList.get(i);
        }
        return answer;
    }
}
```
1. 답을 담을 answerList와 기능마다 걸리는 일 수를 계산해서 담을 arrList를 선언합니다.

2. while 문에서 앞의 수가 더 크다면 작은 수를 지우고 cnt를 증가시킵니다.

뒤의 수가 더 크다면 정답 배열에 cnt를 추가하고, cnt를 초기화합니다.

3. 일이 걸리는 시간을 담은 arrayList의 크기가 정답의 arrayList의 크기보다 1이 작기 전까지 진행합니다.

https://programmers.co.kr/learn/courses/30/lessons/42586

---
