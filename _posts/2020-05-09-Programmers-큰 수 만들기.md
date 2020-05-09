---
layout: post
title: "Programmers-큰 수 만들기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 큰 수 만들기 문제에 관한 설명입니다.<br>

---

### 문제

어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다.

number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

제한사항
* number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.
* k는 1 이상 number의 자릿수 미만인 자연수입니다.

```java
class Solution {
    public String solution(String number, int k) {
        int index = 0;
        char max;
        StringBuilder answer = new StringBuilder();
        
        if(number.charAt(0)=='0') return "0";
        for(int i = 0; i< number.length()-k;i++)
        {
            max = '0';
            for(int j = index; j <= k+i;j++)
            {
                if(max < number.charAt(j)){
                    max = number.charAt(j);
                    index= j+1;
                }
            }
            answer.append(max);
        }
        return answer.toString();
    }
}
```
1. 각 자릿수 값들을 하나하나 비교하면서 가장 큰 숫자를 찾아냅니다.

2. 가장 큰 숫자를 맨 앞으로 넣으면서 순차적으로 길이에서 K만큼 뺀 숫자만큼 이 과정을 반복 합니다.

3. 이것을 하기 위해 StringBuilder와 append 를 이용해 하나하나 붙여나갑니다.

https://programmers.co.kr/learn/courses/30/lessons/42883
---
