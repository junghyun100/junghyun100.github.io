---
layout: post
title: "Programmers-가장 긴 펠린드롬"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 가장 긴 펠린드롬문제에 관한 설명입니다.

---

## 문제 설명

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.

문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 

return 하는 solution 함수를 완성해 주세요.

예를들면, 문자열 s가 "abcdcba"이면 7을 return하고 "abacde"이면 3을 return합니다.

### 제한사항

* 문자열 s의 길이 : 2,500 이하의 자연수
* 문자열 s는 알파벳 소문자로만 구성

```java
class Solution
{
    public int solution(String s)
    {
        int answer = 1; 
        int length = s.length(); 
        for(int i = 2; i <= length;i++){ 
            for(int j = 0 ;j <= length-i ; j++){ 
                boolean check = true; 
                int front = j; 
                int rear = j+i-1; 
                while(front <= rear){ 
                    if(s.charAt(front) == s.charAt(rear)){ 
                        front++;
                        rear--; 
                    }else{ 
                        check=false; 
                        break; 
                    } 
                } 
                if(check){ 
                    answer = i; 
                    break; 
                } 
            } 
        } 
        return answer;
    }
}
```

# 이번 문제는 브루트 포스로 푼 문제였습니다.

일단 answer 는 1을 가집니다.

문자열의 길이는 2500이하의 `자연수`이므로 ""의 값이 들어가는 경우가 없습니다.

또 그 경우 앞 뒤를 뒤집어도 같은 문자열의 펠린드롬은 한글자의 길이인 1이 최소값입니다.

브루트 포스로 펠린드롬 글자수의 길이가 2 부터 s 문자열의 길이까지 확인해 봅니다.

그 다음 반복문은 현재 i번쨰 글자부터 확인을 하는데 끝 글자까지 확인을 해야하니

length - i 까지 반복문을 진행하게 됩니다.

이제 check를 위한 boolean 변수와, 보기 쉽게 front와 rear(끝)의 순서번째를 기억하는 변수를 사용합니다.

while문에서는 만약 front 번째 글자가 rear번째 글자와 같다면 펠린드롬인걸 충족해서

그 다음 글자를 확인할 수 있게 넘어갑니다.

만약 다르다면 펠린드롬이 아니기에 다음 글자 확인으로 넘어갑니다.

while문을 나왔을 때 check가 true일 때, answer는 i(글자 수)가 들어가고 break를 선언합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/12904?language=java">Programmers-가장 긴 펠린드롬</a>

---
