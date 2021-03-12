---
layout: post
title: "Programmers-3진법 뒤집기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 3진법 뒤집기 문제에 관한 설명입니다.

---

## 문제 설명

자연수 n이 매개변수로 주어집니다. 

n을 3진법 상에서 앞뒤로 뒤집은 후, 

이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.

### 제한 사항

* n은 1 이상 100,000,000 이하인 자연수입니다.

### 입출력 예 설명

입출력 예 #1

|n|result|
|:----:|:-----:|
|45|7|
|125|229|

```java
class Solution {
    public int solution(int n) {
        String str = "";
        int digit = 0;
        int answer = 0;
        while(n/3 != 0){
            str += n % 3;
            n /= 3;
        }
        str += String.valueOf(n);
        
        for(int i = str.length()-1 ; 0 <= i ; i--){          
            answer+=((str.charAt(i) - '0') * (int)Math.pow(3,digit));         
            digit++;
        }
        
        return answer;
    }
}
```

### 이번 문제는 구현문제였습니다.

|n(10진법)|n(3진법)|앞뒤반전(3진법)|10진법으로 표현|
|:----:|:-----:|:-----:|:-----:|
|45|1200|0021|7|

답을 도출하는 과정은 위와 같습니다.

코드를 하나씩 따라가봅시다.

while문을 거치게 되면 `3진법`으로 변환에서의 나머지값이 나오게됩니다.

따라서 while문을 거친 `str`에는 `45`의 예시에선 ` 0 0 2` 가 나오게되고,

str+=String.valueOf(n)을 거치게 되면 `int`값을 `string`으로 변환합니다.

그렇게 `n`에 있는 값 `1`을 추가로 넣어주면 `0 0 2 1`이 들어갑니다.

이제 뒤에있는 값부터 하나씩 꺼내주면서 `digit(자릿 수)`만큼 `3`을 제곱승해서

계산합니다.
 
<a href= "https://programmers.co.kr/learn/courses/30/lessons/68935">Programmers-3진법 뒤집기</a>

---
