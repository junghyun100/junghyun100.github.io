---
layout: post
title: "Programmers-입국심사"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 입국심사 문제에 관한 설명입니다.

---

## 문제 설명

n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 

각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 

가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 

하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 

모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

* 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.

* 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.

* 심사관은 1명 이상 100,000명 이하입니다.

### 입출력 예 설명

입출력 예 #1

|n|times|return|
|:----:|:-----:|:-----:|
|6|[7, 10]|28|

```java
import java.util.*;
class Solution {
    public long solution(int n, int[] times) {
        Arrays.sort(times); 
        long start = 1; 
        long end = (long)times[times.length-1] * n; 
        long answer = 0;
        long mid;
        long sum;
        while(start <= end){ 
            mid = (start + end) / 2; 
            sum = 0;
            for(int i = 0 ; i < times.length; i++){
                sum += mid/times[i];
            }
            if(sum >= n){ 
                end = mid-1;
                answer = mid;
            }else{
                start = mid+1;
            }
        }
        
        return answer;
    }
}
```

### 이번 문제는 이진탐색 문제 였습니다.

문제 사이트에 들어오기 전에 아래에 문제 카테고리로 

<strong>이진탐색</strong>이라고 적혀있는 걸 보았습니다.

그게 힌트가 크게 되었어서 그렇지 아니었으면 한참 걸렸을 것 같습니다.

처음에 Arrays.sort로 심사관들의 처리속도를 정렬합니다.

빠른 사람이 앞에 올수록 빨리 처리할 수 있으므로 그것을 기준으로 처리 인원을 결정합니다.

start는 1로 설정합니다.(심사받으러 오는 인원은 최소 1명이기 때문)

end는 (long)times[times.length-1] * n; 로 설정합니다.

* 설명 1 : 최악의 경우인 마지막 사람이 혼자서 n명을 다 처리한다는 가정을 했을 때입니다.

* 설명 2 : (long)으로 형변환 해준 이유는 long * int가 int로 갔다가 long으로 변환되기 때문에 값이 이상하게 변환되는 경우가 발생했음.

```java
        while(start <= end){ 
            mid = (start + end) / 2; 
            sum = 0;
            for(int i = 0 ; i < times.length; i++){
                sum += mid/times[i];
            }
            if(n <= sum){ 
                end = mid-1;
                answer = mid;
            }else{// 그 반대임.
                start = mid+1;
            }
        }
```
이분 탐색 부분입니다.

일단 mid는 양 끝점의 절반 부분입니다.

반복문을 통해서 심사관들의 수만큼 mid를 심사관의 처리속도로 나눠줍니다.

그리고 그값을 sum에 저장합니다.

그럼 `mid`라는 시간 이내에 

심사관들이 처리할 수 있는 최대 인원을 모두 합한 값이 `sum`에 들어가게 됩니다. 

그렇게 나왔을 때 `sum`이 최대로 처리할 수 있는 인원 수 이니

`n`보다 크다면 더 빨리 처리하므로 `end`를 줄이고,

반대라면 `start`를 늘려줍니다.

이 과정을 반복하면서 `answer`에는 `mid`값을 넣어줍니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/43238">Programmers-입국심사</a>

---
