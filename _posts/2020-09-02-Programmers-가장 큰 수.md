---
layout: post
title: "Programmers-가장 큰 수"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 가장 큰 수 문제에 관한 설명입니다.

---

## 문제

0 또는 양의 정수가 주어졌을 때, 

정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고,
 
이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 

순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

### 제한사항
   *  numbers의 길이는 1 이상 100,000 이하입니다.
   *  numbers의 원소는 0 이상 1,000 이하입니다.
   *  정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

### 입출력 예제  

|      numbers       |   return | 
| :----------------: | :------: | 
| [6, 10, 2]     	 |  "6210"  | 
| [3, 30, 34, 5, 9]  |"9534330" | 



```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
	public String solution(int[] numbers) {
		String[] strNumbers = new String[numbers.length];
        
		for (int i = 0; i < numbers.length; i++) {
			strNumbers[i] = String.valueOf(numbers[i]);
		}
        
		Arrays.sort(strNumbers, new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				return (Integer.parseInt(o2 + o1) - Integer.parseInt(o1 + o2));
			}
		});

		if(strNumbers[0].equals("0")) {
            return "0";
		}
		
		String answer = "";
		for (String a : strNumbers) {
			answer += a;
		}
        
		return answer;
	}
}
 ```

### 이번 문제는 정렬 문제였습니다.
 
"Integer 형과 String 형으로 자유롭게 변환할 수 있으면서 그 값을 정렬하는 것"을

연습하기에 좋은 문제 였습니다.
 
### 예시를 통한 해결 분석

### 분기문 안쪽을 살펴봅시다.

`[3, 30, 34, 5, 9]`를 예로 듭시다.

```java
		for (int i = 0; i < numbers.length; i++) {
			strNumbers[i] = String.valueOf(numbers[i]);
		}
```

`strNumbers`에는 `[3, 30, 34, 5, 9]`가 String형으로 들어있게 되었습니다.

##### String.valueof(int i)란?

API를 살펴 봅시다.

> * public static String valueOf(int i)<br>
Returns the string representation of the int argument.<br>
The representation is exactly the one returned by the Integer.toString method of one argument. 

간단히 말하자면 int형 값 파라미터로 넣으면 String 표현식으로 변경시켜 줍니다.

### Arrays.sort 안쪽을 살펴봅시다.

```java
		Arrays.sort(strNumbers, new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				return (Integer.parseInt(o2 + o1) - Integer.parseInt(o1 + o2));
			}
		});

```

문제에서 원하는 기준을 만족하기 위해서 Comparator를 사용했습니다.

Comparaotr와 Comparable은 본인이 사용하고자 하는 정렬 방식을 설정할 때 사용 합니다.

`strNumbers`에 있는 값들을 Comparator에서 사용할 때

`(o2 + o1)` 과 `(o1 + o2)`를 비교합니다. 

`6`과 `10`을 `o1`과 `o2`로 가정했을 때 `610` 과 `106`이 됩니다.

정렬하기 위해 -를 비교해야 하는 과정이 필요한데.

이 때, String형으로 - 를 하는 것은 오류가 발생하게 됩니다.

그렇기 때문에 Integer.parseInt를 사용해 Int형식일 때 크기 비교를 통해 정렬 방식을 정했습니다.

저렇게 된다면 

`[9, 5, 34, 3, 30]`으로 정렬 되어있을 것입니다.

### 함정(?)이 있었습니다.

마지막 테스트 케이스를 살펴보았을때 `0`이라는 값들만 배열에 있거나 한다면,

결과는 `"0"`이 반환되어야 합니다.

이것을 그대로 반영한다면 `0`의 갯수에 따라 `"0000000000000000000"`이 반환 될 수도 있기 때문에

조건문을 사용해 줍시다.

```java 
		if(strNumbers[0].equals("0")) {
            return "0";
		}
```

정렬이 된 상태에서도 `0`이 먼저 있다면 뒤는 안봐도 전부 `0`으로 되어있을 겁니다.

그냥 return 시킬 때 `"0"`하나만 넘겨줍시다.



<a href= "https://programmers.co.kr/learn/courses/30/lessons/42746">가장 큰 수</a>

---
