---
layout: post
title: "Programmers-K번째수"
tags: [Programmers]
comments: true

---

위 문제는 Programmers K번째수에 관한 설명입니다.<br>

---

### 문제

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.

1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.

2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때,

commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항
* array의 길이는 1 이상 100 이하입니다.

* array의 각 원소는 1 이상 100 이하입니다.

* commands의 길이는 1 이상 50 이하입니다.

* commands의 각 원소는 길이가 3입니다.

```java

import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int [commands.length];
        for(int loop = 0 ; loop < commands.length; loop++)
        {
            int count =0;
            int [] arrayTest = new int[commands[loop][1]-commands[loop][0]+1];
            for(int loop2 = commands[loop][0]-1;loop2 <= commands[loop][1]-1;loop2++)
            {
                arrayTest[count] = array[loop2];
                count ++;
            }
            Arrays.sort(arrayTest);
            answer[loop] = arrayTest[commands[loop][2]-1];
        }
        return answer;
    }
}

```

### 코드 설명

방의 길이만큼 answer칸을 만들어주고, commands의 갯수만큼 loop문을 돌립니다.

이때 

commands[loop][0] = 어디서 부터 자를것인가

commands[loop][1] = 어디까지 자를것인가

commands[loop][2] = (잘라낸 수를 정렬 후) 몇번째 숫자를 뽑을 것인가로 표현했고

잘라낸 배열을 arrayTest로 옮긴 후 Arrays.sort하면서 정렬 시킵니다.

그리고 원하는 숫자가 있는 위치를 골라서 answer배열에 넣어줍니다.

https://programmers.co.kr/learn/courses/30/parts/12198

---
