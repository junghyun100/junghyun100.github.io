---
layout: post
title: "Programmers-숫자야구"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 숫자야구 문제에 관한 설명입니다.<br>

---

### 문제

숫자 야구 게임이란 2명이 서로가 생각한 숫자를 맞추는 게임입니다. 게임해보기

각자 서로 다른 1~9까지 3자리 임의의 숫자를 정한 뒤 서로에게 3자리의 숫자를 불러서 결과를 확인합니다. 

그리고 그 결과를 토대로 상대가 정한 숫자를 예상한 뒤 맞힙니다.

* 숫자는 맞지만, 위치가 틀렸을 때는 볼
* 숫자와 위치가 모두 맞을 때는 스트라이크
* 숫자와 위치가 모두 틀렸을 때는 아웃

예를 들어, 아래의 경우가 있으면
```
A : 123
B : 1스트라이크 1볼.
A : 356
B : 1스트라이크 0볼.
A : 327
B : 2스트라이크 0볼.
A : 489
B : 0스트라이크 1볼.
```
이때 가능한 답은 324와 328 두 가지입니다.

질문한 세 자리의 수, 스트라이크의 수, 볼의 수를 담은 2차원 배열 baseball이 매개변수로 주어질 때, 

가능한 답의 개수를 return 하도록 solution 함수를 작성해주세요.

* 제한사항

질문의 수는 1 이상 100 이하의 자연수입니다.

baseball의 각 행은 [세 자리의 수, 스트라이크의 수, 볼의 수] 를 담고 있습니다.

```java

class Solution {
    public int solution(int[][] baseball) {
        int answer = 0;
        
        for(int i = 123; i< 987; i++)
        {
            int C = i%10;
            int B = (i/10)%10;
            int A = i/100;
            if(A ==0 || B==0 || C==0){
                continue;
            }
            if((A==B)||(B==C)||(C==A)){
                continue;
            }
            for(int j = 0; j < baseball.length; j++)
            {
                int strike = 0;
                int ball = 0;
                
                int question = baseball[j][0];
                int question_C = question % 10;
                int question_B = (question / 10)%10;
                int question_A = question/100;
                
                if(question_C ==0 || question_B == 0 || question_A==0){
                    break;
                }
                if(C == question_C) strike++;
                if(B == question_B) strike++;
                if(A == question_A) strike++;
                
                if(strike!= baseball[j][1]) break;
                
                if((C==question_B)||(C==question_A)) ball ++;
                if((B==question_A)||(B==question_C)) ball ++;
                if((A==question_B)||(A==question_C)) ball ++;
                if(ball != baseball[j][2]) break;
                
                if(j== baseball.length-1) answer++;
            }
        }
        return answer;
    }
}

```

<a href= "https://programmers.co.kr/learn/courses/30/lessons/42841">숫자 야구</a>

---
