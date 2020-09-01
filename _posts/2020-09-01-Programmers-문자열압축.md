---
layout: post
title: "Programmers-문자열압축"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 문자열압축 문제에 관한 설명입니다.<br>

2020 KAKAO BLIND RECRUITMENT에 나왔던 문제입니다.

---

## 문제

데이터 처리 전문가가 되고 싶은 어피치는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다.
 
최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 
 
문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 
 
더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
 
간단한 예로 aabbaccc의 경우 2a2ba3c(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 

표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 

예를 들면, abcabcdede와 같은 문자열은 전혀 압축되지 않습니다. 

어피치는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 

더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

예를 들어, ababcdcdababcdcd의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 

2개 단위로 잘라서 압축한다면 2ab2cd2ab2cd로 표현할 수 있습니다. 

다른 방법으로 8개 단위로 잘라서 압축한다면 2ababcdcd로 표현할 수 있으며, 

이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

다른 예로, abcabcdede와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 abcabc2de가 되지만, 

3개 단위로 자른다면 2abcdede가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 

이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 

위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 

가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

### 제한사항
   *  s의 길이는 1 이상 1,000 이하입니다.
   *  s는 알파벳 소문자로만 이루어져 있습니다.

### 입출력 예제  

|              s              |result | 
| :-------------------------: | :---: | 
| "aabbaccc"               	  |   7   | 
| "ababcdcdababcdcd"          |   9   | 
| "abcabcdede"                |   8   |
| "abcabcabcabcdededededede"  |   14  |
| "xababcdcdababcdcd"         |   17  |

### 입출력 예에 대한 설명
####입출력 예 #1

문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.

####입출력 예 #2

문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.

#### 입출력 예 #3

문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.

#### 입출력 예 #4

문자열을 2개 단위로 자르면 abcabcabcabc6de 가 됩니다.

문자열을 3개 단위로 자르면 4abcdededededede 가 됩니다.

문자열을 4개 단위로 자르면 abcabcabcabc3dede 가 됩니다.

문자열을 6개 단위로 자를 경우 2abcabc2dedede가 되며, 이때의 길이가 14로 가장 짧습니다.

#### 입출력 예 #5

문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다.

따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다.

이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.

```java
class Solution {
    public int solution(String s) {
        int answer = s.length();
        
        for(int i = 1; i <= s.length()/2;++i){
            int point = 0;
            int leng = s.length();
            
            for(;point+i <= s.length();){
                String unit = s.substring(point,point+i);
                point +=i;
                
                int count = 0;
                
                for(;point +i <= s.length();){
                    if(unit.equals(s.substring(point,point+i))){
                        ++count;
                        point += i;
                    }
                    else{
                        break;
                    }
                }
                
                if(count > 0){
                    leng -= i * count;
                    
                    if(count < 9)leng += 1;
                    else if(count < 99)leng += 2;
                    else if(count < 999)leng += 3;
                    else leng += 4;
                }
            }
            answer = Math.min(answer, leng);
        }
        
        return answer;
    }
}
 ```

### 이번 문제는 문자열 처리 문제였습니다.
 
 예시를 들어 설명하는 것이 가장 이해가 빠르다고 생각됩니다.
 
 아래의 과정을 천천히 따라갑시다.
 
### 예시를 통한 해결 분석

 `aabbaccc`를 예로 듭니다.
```java
        for(int i = 1; i <= s.length()/2;++i){
            int point = 0;
            int leng = s.length();
``` 
 `i`가 기준이 되는 문자의 길이로 둡니다.
 
 그렇다면 `i`의 길이가 `s.length()/2`보다 길다면 연속된 문자가 나오지 못합니다.
  
 `point`는 현재의 위치를 나타냅니다.
 
#### 분기문 안쪽으로 들어가 봅시다.
 ```java
 
for(;point+i <= s.length();){
                String unit = s.substring(point,point+i);
                point +=i;
                
                int count = 0;
                
                for(;point +i <= s.length();){
                    if(unit.equals(s.substring(point,point+i))){
                        ++count;
                        point += i;
                    }
                    else{
                        break;
                    }
                }
```
 `unit`의 처음으로 들어갈 값은 `a`가 됩니다.
 
 `point`에는 1의 값이 들어가게되고
 
 분기문을 반복하면서 그 유닛 뒤로 같은 길이로 문자를 가지고 와서 
 
 같다면 숫자를 늘려주고,
 
 포지션을 다음칸(`point + i`), 두번째 `a`를 가르키게 됩니다.
 
 이 과정을 반복하면 몇개의 문자가 겹치는지 나오고, 만약 다르다면
 
 분기문을 나옵니다.
 
 이때 저장된 count가 있으니, 조건문을 살펴봅니다.
 
#### 조건문 안쪽으로 들어가 봅시다.
```java
if(count > 0){
    leng -= i * count;

    if(count < 9)leng += 1;
    else if(count < 99)leng += 2;
    else if(count < 999)leng += 3;
    else leng += 4;
}
 ```
 늘어난 카운트의 크기만큼 길이를 줄여서 적습니다.
 
 `aa`라고 한다면 2의 길이에서 1을 뺍니다.
 
 그리고 나서 `count`가 9보다 작으니까 길이를 2a라고 하기 때문에 길이를 1늘립니다.
 
 10보다 크고 99보다 작은 경우엔 2자리기에 +2를 합니다.
 
 나머지 조건도 같습니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/60057">문자열 압축</a>

---
