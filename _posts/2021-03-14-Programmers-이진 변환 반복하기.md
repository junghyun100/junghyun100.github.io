---
layout: post
title: "Programmers-이진 변환 반복하기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 이진 변환 반복하기 문제에 관한 설명입니다.

---

## 문제 설명

0과 1로 이루어진 어떤 문자열 x에 대한 이진 변환을 다음과 같이 정의합니다.

x의 모든 0을 제거합니다.

x의 길이를 c라고 하면, x를 "c를 2진법으로 표현한 문자열"로 바꿉니다.

예를 들어, x = "0111010"이라면,

x에 이진 변환을 가하면 `x = "0111010" -> "1111" -> "100"` 이 됩니다.

0과 1로 이루어진 문자열 s가 매개변수로 주어집니다. 

s가 "1"이 될 때까지 계속해서 s에 이진 변환을 가했을 때, 

이진 변환의 횟수와 변환 과정에서 제거된 모든 0의 개수를 각각 배열에 담아 return 하도록 solution 함수를 완성해주세요.

### 제한 사항

* s의 길이는 1 이상 150,000 이하입니다.
* s에는 '1'이 최소 하나 이상 포함되어 있습니다.

### 입출력 예 설명

입출력 예 #1

|s|result|
|:----:|:-----:|
|"110010101001"|[3,8]|
|"01110"|[3,3]|

```java
class Solution {
    public int[] solution(String s) {
        int[] answer = new int[2];
        
        int turn = 0;
        int countZero = 0;
        
        while(!s.equals("1")){
            int countOne = 0;
            
            for(int i = 0; i < s.length(); i++){
                if(s.charAt(i) == '1')
                    countOne+=1;
            }
            countZero += (s.length()-countOne);
            turn++;
            s = Integer.toBinaryString(countOne);
        }
        answer[0] = turn;
        answer[1] = countZero;
        return answer;
    }
}
```

### 이번 문제는 진법 문제 였습니다.

`answer` 변수에는 몇번의 변환이 있었는가(`turn`)와 

제거된 0의 갯수(`countZero`)가 담겨야합니다.

### while문 내부

`while문`은 조건식으로 `s`가 "1"이 될 때까지 진법변환이 되어야한다고 넣어줍니다.

이제 `s`에서 1의 갯수를 구합니다.

하나씩 꺼내서 확인해주는 방식을 사용했습니다.

다 빠져나왔을때 `countZero`는 `s`의 길이에서 `countOne`의 값을 뺀 값을 더 해줍니다.

그래야 0의 갯수가 추가적으로 더해지기 때문입니다.

이제 진수 변환횟수를 늘려주고,

0이 제거된 1의 갯수를 `Binary`값의 `String`으로 저장하기 위해 `toBinaryString()`을 사용합니다.

이 과정을 반복합니다.

### 결과 반환

끝나고 나왔을 때, `turn`에는 몇번의 진수변환이 있었는가가 저장되고,

`countZero`에는 여태까지 없앴던 0의 갯수가 저장됩니다.

이제 그걸 0번 방과 1번 방에 저장합니다.

그리고 `answer`값을 반환합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/70129?language=java">Programmers-이진 변환 반복하기</a>

---
