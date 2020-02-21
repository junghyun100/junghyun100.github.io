---
layout: post
title: "Programmers-문자열 내마음대로 정렬"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 문자열 내마음대로 정렬 문제에 관한 설명입니다.<br>

---

### 문제

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다.

예를 들어 strings가 [sun, bed, car]이고 n이 1이면 각 단어의 인덱스 1의 문자 u, e, a로 strings를 정렬합니다.

제한 조건

strings는 길이 1 이상, 50이하인 배열입니다.

strings의 원소는 소문자 알파벳으로 이루어져 있습니다.

strings의 원소는 길이 1 이상, 100이하인 문자열입니다.

모든 strings의 원소의 길이는 n보다 큽니다.

인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

입출력 예 1

sun, bed, car의 1번째 인덱스 값은 각각 u, e, a 입니다.

이를 기준으로 strings를 정렬하면 [car, bed, sun] 입니다.

입출력 예 2

abce와 abcd, cdx의 2번째 인덱스 값은 c, c, x입니다.

따라서 정렬 후에는 cdx가 가장 뒤에 위치합니다.

abce와 abcd는 사전순으로 정렬하면 abcd가 우선하므로, 답은 [abcd, abce, cdx] 입니다.



```java

import java.util.Arrays;

class Solution {
    public String[] solution(String[] strings, int n) {
        String[] answer = new String[strings.length];
        for (int i = 0; i < strings.length; i++)
            answer[i] = strings[i].charAt(n) + strings[i];
        Arrays.sort(answer);
        for (int i = 0; i < strings.length; i++)
            answer[i] = answer[i].substring(1);
        return answer;
    }
}

```

### 코드 설명

1.원하는 n글자를 맨앞, 그리고 원본 문자열 자체를 통째로 붙입니다.

2. Arrays.sort를 이용해 한번에 정렬 오름차순 정렬시킵니다.

(이후 문자열 비교도 자동으로 정렬됨.)

3.맨앞 글자 때어냄으로 원본 문자열이 정렬된 상태로 만듭니다.

---
