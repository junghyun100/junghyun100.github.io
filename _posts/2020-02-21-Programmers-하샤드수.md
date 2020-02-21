---
layout: post
title: "Programmers-하샤드수"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 하샤드 수 문제에 관한 설명입니다.<br>

---

### 문제

양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다.

예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다.

자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.

입출력 예 #1

10의 모든 자릿수의 합은 1입니다. 10은 1로 나누어 떨어지므로 10은 하샤드 수입니다.

입출력 예 #2

12의 모든 자릿수의 합은 3입니다. 12는 3으로 나누어 떨어지므로 12는 하샤드 수입니다.

입출력 예 #3

11의 모든 자릿수의 합은 2입니다. 11은 2로 나누어 떨어지지 않으므로 11는 하샤드 수가 아닙니다.

입출력 예 #4

13의 모든 자릿수의 합은 4입니다. 13은 4로 나누어 떨어지지 않으므로 13은 하샤드 수가 아닙니다.

10=
true

12=
true

11=
false

13=
false




```java
class Solution {
  public boolean solution(int x) {
      int sum = checkOfsum(x);
      return x % sum == 0;
  }
  private int checkOfsum(int xcheck)
  {
      int sum = 0;
      while(xcheck>0)
      {
          sum += (xcheck%10);
          xcheck/=10;
      }
      return sum;
  }
}
```

### 코드 설명

1. checkOfSUm에서는 각 자릿수 마다의 합을 구합니다.

1-1. 각 자릿수의 값을 구하기 위해 %10을 해줍니다. 이후 /10으로 다음 자리 수로 넘어갑니다.

2. return 결과를 바로 반환합니다.

---
