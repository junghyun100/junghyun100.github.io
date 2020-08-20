---
layout: post
title: "Programmers-전화번호 목록"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 전화번호 목록 문제에 관한 설명입니다.<br>

---

## 문제

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.

전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

* 구조대 : 119
* 박준영 : 97 674 223
* 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 

어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

* phone_book의 길이는 1 이상 1,000,000 이하입니다.
* 각 전화번호의 길이는 1 이상 20 이하입니다

### 입출력 예 설명

입출력 예 #1

앞에서 설명한 예와 같습니다.

입출력 예 #2

한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3

첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.


### 효율성에서 걸러진 코드.
```java

class Solution {
    public boolean solution(String[] phone_book) {
boolean answer = true;
		for (int i = 0; i < phone_book.length - 1; i++) {
			for (int j = i + 1; j < phone_book.length; j++) {
				if (phone_book[i].length() > phone_book[j].length()) {
					String temp = phone_book[j];
					phone_book[j] = phone_book[i];
					phone_book[i] = temp;
				}
			}

		}
		for (int i = 0; i < phone_book.length - 1; i++) {
			for (int j = i + 1; j < phone_book.length; j++) {
				String str = phone_book[i];
				if (phone_book[j].substring(0, str.length()).equals(str))
					answer = false;
			}
		}
		return answer;
    }
}

```

문제를 풀면서 효율성을 고려하지 않아서 시간초과가 나왔습니다.

반복문의 사용이 잦았기에 그렇습니다.

그래서 다른 풀이법을 사용했습니다.

### 해당 문제를 풀기 위해 startsWith를 사용했습니다.

자바 api 사이트에서 가져온 내용을 봅시다.

### startsWith

Tests if this string starts with the specified prefix.

대상 문자열이 특정 문자 또는 문자열로 시작하는지 체크하는 함수입니다.

> Returns :<br>
true if the character sequence represented by the argument is a prefix of <br>
the character sequence represented by this string false otherwise. <br>
Note also that true will be returned if the argument is an empty string or <br>
is equal to this String object as determined by the equals(Object) method.<br>

인수로 대표되는 문자열이 의 접두어인 경우 true

그렇지 않은경우는 false로 반환이 되겠습니다.

```java
import java.util.Arrays;

class Solution {
	public boolean solution(String[] phone_book) {
		boolean answer = true;
		Arrays.sort(phone_book);
		for (int i = 0; i < phone_book.length - 1; i++) {
			if (phone_book[i + 1].startsWith(phone_book[i])) {
				answer = false;
				break;
			}
		}
		return answer;
	}
}
```

다음 차례인 값이랑 현재값의 앞자리를 비교했을 때 겹친다면 true가 반환이되어 answer는 false가 됩니다.

그리고 break문을 지나 return이 반환 됩니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/42577">전화번호 목록</a>

---
