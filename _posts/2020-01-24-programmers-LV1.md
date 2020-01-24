---
layout: post
title: "programmers LV.1"
tags: [프로그래머스]
comments: true

---

오늘의 포스팅은 프로그래머스에서 LV1 test를 풀 때
나온 문제에 대한 알고리즘 풀이입니다.

---

#### 첫번째 문제 설명

정수를 담고 있는 배열 arr의 평균값을 return하는 함수, solution을 완성해보세요.

#### 제한사항
<ul>
<li>arr은 길이 1 이상, 100 이하인 배열입니다.</li>
<li>arr의 원소는 -10,000 이상 10,000 이하인 정수입니다.</li>
</ul>

---

```java
class Solution {
  public double solution(int[] arr) {
      double answer = 0;
      for(int i = 0;i<arr.length;i++)
          answer+=arr[i];
      answer/=arr.length;
      return answer;
  }
}
      
```
음.. 완전 기초네요!

#### 두번째 문제 설명
두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.
예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

#### 제한 조건
<ul>
<li>a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.</li>
<li>a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.</li>
<li>a와 b의 대소관계는 정해져있지 않습니다.</li>
</ul>

---

```java

class Solution {
  public long solution(int a, int b) {
      long answer = 0;
      int temp=0;
      if(a==b)//같을 때는 상관없으니 answer에 a를 넣고 리턴시킵니다.
          answer = a;
      else // 다를 때 a와 b가 어느쪽이 높은지 확인후 for문을 돌릴 생각이므로
      {
          if(a>b)//a-> b쪽으로 for문 돌릴생각으로 비교해서 swap했습니다.
          { 
             temp = a;
             a = b;
             b = temp;
          }
          for(int i =a ;i<=b;i++)
          {
              answer+=i;
          }
      }
      return answer;
  }
}
      
```

이것도 쉬웠습니다.

LV1이라 그런가 만점이기도 했고..

그냥 언어를 배워본적 있는가? 정도의 수준인 것 같습니다.

---
