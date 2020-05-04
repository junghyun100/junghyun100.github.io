---
layout: post
title: "Programmers-모의고사"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 모의고사 문제에 관한 설명입니다.<br>

---

### 문제

수포자는 수학을 포기한 사람의 준말입니다.

수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다.

수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때,

가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

* 시험은 최대 10,000 문제로 구성되어있습니다.
* 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
* 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.


```java
import java.util.*;

public class Solution {
    public int[] solution(int[] answers) {

        int answerPattern1[] = { 1, 2, 3, 4, 5 };// 반복되는 찍는 방식
        int answerPattern2[] = { 2, 1, 2, 3, 2, 4, 2, 5 };
        int answerPattern3[] = { 3, 3, 1, 1, 2, 2, 4, 4, 5, 5 };

        int answerCount1 = getAnswerCount(answers, answerPattern1);
        int answerCount2 = getAnswerCount(answers, answerPattern2);
        int answerCount3 = getAnswerCount(answers, answerPattern3);

        return getAnswerRank(answerCount1, answerCount2, answerCount3);
    }

    private int getAnswerCount(int answers[], int answerPattern[]) {
        int count = 0;
        for (int i = 0; i < answers.length; i++) {
            if (answers[i] == answerPattern[i % answerPattern.length]) // 여러문제일 때 고려해서
                count++;
        }
        return count;
    }

    private int[] getAnswerRank(int answerCount1, int answerCount2, int answerCount3) {
        ArrayList<Integer> answerRank = new ArrayList<>();
        int maxCount = Math.max(answerCount1, answerCount2);// max인 값 저장하기
        maxCount = Math.max(maxCount, answerCount3);

        if (maxCount == answerCount1) // max와 같은 점수면 add 시켜서 저장하기
            answerRank.add(1);
        if (maxCount == answerCount2)
            answerRank.add(2);
        if (maxCount == answerCount3)
            answerRank.add(3);

        int resultArray[] = new int[answerRank.size()];// resultArray에 답 옮기기
        for (int i = 0; i < resultArray.length; i++) {
            resultArray[i] = answerRank.get(i);
        }
        return resultArray;
    }
}
```
### 처음 풀었던 방식

### 코드 설명

1. getAnswerCount에서 답의 갯수를 샙니다.

2. getAnswerRank는 getAnswerCount에서 알게된 갯수를 기준으로 rank를 매깁니다.
 
3. return 시킵니다.

```java
import java.util.*;
public class Solution {
    class Student {
        private int[] answerPattern;
        int answerCount =0;

        public int[] getAnswerPattern() {
            return answerPattern;
        }

        public void setAnswerPattern(int[] answerPattern) {
            this.answerPattern = answerPattern;
        }

        public int getAnswerCount() {
            return answerCount;
        }

        public void setAnswerCount(int answerCount) {
            this.answerCount = answerCount;
        }
    }
    public int[] solution(int[] answers) {
        int answerPattern1[] = { 1, 2, 3, 4, 5 };// 반복되는 찍는 방식
        int answerPattern2[] = { 2, 1, 2, 3, 2, 4, 2, 5 };
        int answerPattern3[] = { 3, 3, 1, 1, 2, 2, 4, 4, 5, 5 };

        Student student1 = new Student();
        Student student2 = new Student();
        Student student3 = new Student();

        student1.setAnswerPattern(answerPattern1);
        student2.setAnswerPattern(answerPattern2);
        student3.setAnswerPattern(answerPattern3);


        student1.setAnswerCount(countAnswer(answers, student1.getAnswerPattern())) ;
        student2.setAnswerCount(countAnswer(answers, student2.getAnswerPattern())) ;
        student3.setAnswerCount(countAnswer(answers, student3.getAnswerPattern())) ;
        return rankAnswer(student1.getAnswerCount(), student2.getAnswerCount(), student3.getAnswerCount());
    }

    private int countAnswer(int answers[], int answerPattern[]) {
        int count = 0;
        for (int i = 0; i < answers.length; i++) {
            if (answers[i] == answerPattern[i % answerPattern.length]) // 여러문제일 때 고려해서
                count++;
        }
        return count;
    }

    private int[] rankAnswer(int answerCount1, int answerCount2, int answerCount3) {
        ArrayList<Integer> answerRank = new ArrayList<>();
        int maxCount = Math.max(answerCount1, answerCount2);// max인 값 저장하기
        maxCount = Math.max(maxCount, answerCount3);

        if (maxCount == answerCount1) // max와 같은 점수면 add 시켜서 저장하기
            answerRank.add(1);
        if (maxCount == answerCount2)
            answerRank.add(2);
        if (maxCount == answerCount3)
            answerRank.add(3);

        int resultArray[] = new int[answerRank.size()];// resultArray에 답 옮기기
        for (int i = 0; i < resultArray.length; i++) {
            resultArray[i] = answerRank.get(i);
        }
        return resultArray;
    }
}
```
객체를 활용해서 다시 풀었을 때.

과정은 위와 같지만 객체의 개념을 사용해서 풀었을 때입니다.

https://programmers.co.kr/learn/courses/30/parts/12230

---
