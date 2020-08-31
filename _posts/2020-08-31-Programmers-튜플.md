---
layout: post
title: "Programmers-튜플"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 튜플 문제에 관한 설명입니다.<br>

---

## 문제

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

* (a1, a2, a3, ..., an)
튜플은 다음과 같은 성질을 가지고 있습니다.

1. 중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
2. 원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다.<br> ex : (1, 2, 3) ≠ (1, 3, 2)
3. 튜플의 원소 개수는 유한합니다.

원소의 개수가 n개이고, 중복되는 원소가 없는 튜플 (a1, a2, a3, ..., an)이 주어질 때(단, a1, a2, ..., an은 자연수),
 
이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는
```
* {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}
```
와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로
```
* {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}
* {{2, 1, 3, 4}, {2}, {2, 1, 3}, {2, 1}}
* {{1, 2, 3}, {2, 1}, {1, 2, 4, 3}, {2}}
```
는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

### 제한사항
   *  s의 길이는 5 이상 1,000,000 이하입니다.
   *  s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
   *  숫자가 0으로 시작하는 경우는 없습니다.
   *  s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
   *  s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
   *  return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.

### 입출력 예제  

|                 s                  |      result      | 
| :--------------------------------: | :--------------: | 
| "{{2},{2,1},{2,1,3},{2,1,3,4}}"	 |   [2, 1, 3, 4]   | 
| "{{1,2,3},{2,1},{1,2,4,3},{2}}"    |   [2, 1, 3, 4]   | 
| "{{20,111},{111}}"                 |   [111, 20]      |
| "{{123}}"                          |   [123]          |
| "{{4,2,3},{3},{2,3,4,1},{2,3}}"    |   [3, 2, 4, 1]   |



```java
import java.util.*;

class Solution {
    public int[] solution(String s) {
        int[] answer;
        String[] strArray;
        ArrayList<Integer> check = new ArrayList<>();
		s=s.replace("{", "");
		s = s.substring(0, s.length() - 2);
		strArray = s.split("},");

        Arrays.sort(strArray, new Comparator<String>(){
	    @Override
         public int compare(String o1, String o2){
         	return o1.length() - o2.length();
           }
        });

        for(String tuple : strArray){
			String[] strArray2 = tuple.split(",");
            for(String num : strArray2){
                if(!check.contains(Integer.parseInt(num))){
                    check.add(Integer.parseInt(num));
                }
            }
        }

       answer = new int[check.size()];
       for(int i=0; i<check.size(); i++)
           answer[i] = check.get(i);

        return answer;
    }
}
 ```

### 이번 문제는 문자열 처리 문제였습니다.
 
 예시를 들어 설명하는 것이 가장 이해가 빠르다고 생각됩니다.
 
 아래의 과정을 천천히 따라갑시다.
 
### 예시를 통한 해결 분석

첫번째 예시인 String `s`에 `"{{2},{2,1},{2,1,3},{2,1,3,4}}"`이 들어있다고 생각해봅시다.

일단 s에서 앞부분의 `{`를 없애봅시다.
```java
s=s.replace("{", ""); 
// 그 결과는 ?  2},2,1},2,1,3},2,1,3,4}}
```
그리고나서 맨뒤의 괄호도 때어내 버립시다.
```java
s = s.substring(0, s.length() - 2);
// 그 결과는 ?  2},2,1},2,1,3},2,1,3,4
```
그럼 결과가 위의 코드에서 보이는 결과처럼 나오게 됩니다.

이때 각각 구분을 하기 위해서 `},`가 연달아서 보인다면? 구분을 하도록 했습니다.

```java
strArray = s.split("},");

// 그 결과는 아래의 코드를 실행시키면 전체적으로 출력이 됩니다.

//for(String str : strArray)
//System.out.println(str);
// 0번째~ 3번째 방까지 출력
//2 
//2,1
//2,1,3
//2,1,3,4

```

그리고 나선 s의 길이에 따라 오름차순을 시켰습니다.

 ```java
Arrays.sort(strArray, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
	    return o1.length() - o2.length();
    }
});

//2 
//2,1
//2,1,3
//2,1,3,4 길이가 순서대로 나열 되어있는 상태여서 그대롭니다.
 ``` 

자 문자열을 활용할 차례가 되었습니다.

```java
	for (String tuple : strArray) {
		String[] strArray2 = tuple.split(",");
		for (String num : strArray2) {
		    if (!list.contains(Integer.parseInt(num))) {
		        list.add(Integer.parseInt(num));
			}
		}
	}
```
`strArray2`에는 `,`를 기준으로 잘라냈으므로, 

숫자형태(String이지만)의 값들이 들어있습니다.

길이가 작은 순서부터 정렬이 되어있으니 숫자형태인 값이 `list`에 포함되어있지 않다면.
 
int형으로 바꾸어서 `list`에 추가합니다.
```java
answer = new int[list.size()];
for (int i = 0; i < list.size(); i++)
	answer[i] = list.get(i);
```
이제 `list`에 있는것을 `answer`에 옮겨 담습니다.

그리고 `answer`를 출력하면 됩니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/64065">튜플</a>

---
