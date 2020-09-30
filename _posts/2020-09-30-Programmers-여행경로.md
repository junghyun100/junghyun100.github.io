---
layout: post
title: "Programmers-여행경로"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 여행경로 문제에 관한 설명입니다.<br>

---

## 문제

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 ICN 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때,
 
방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

## 제한조건

* 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
* 주어진 공항 수는 3개 이상 10,000개 이하입니다.
* tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
* 주어진 항공권은 모두 사용해야 합니다.
* 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
* 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

## 입출력 예

|tickets|return|
|:---------:|:----:|
|[[ICN, JFK], [HND, IAD], [JFK, HND]]|[ICN, JFK, HND, IAD]|
|[[ICN, SFO], [ICN, ATL], [SFO, ATL], [ATL, ICN], [ATL,SFO]]|[ICN, ATL, ICN, SFO, ATL, SFO]|


### 해결 방법

```java
import java.util.ArrayList;
import java.util.Collections;

class Solution {
    
    static boolean visit[];
	static ArrayList<String> list = new ArrayList<>();
	static String str = "";
    
    public String[] solution(String[][] tickets) {
        visit = new boolean[tickets.length];
		for(int i = 0 ; i < tickets.length; i++) {
			String start = tickets[i][0];
			String end = tickets[i][1];
			
			if(start.equals("ICN")) {
				visit[i] = true;
				str = start + ",";
				recur(tickets, end, 1);
				visit[i] = false;
			}
		}
		Collections.sort(list);
		String[] answer = list.get(0).split(",");
		return answer;
    }
	
	public static void recur(String[][] tickets, String end, int count) {
		str += end + ",";
		if(count==tickets.length) {
			list.add(str);
			return;
		}		
		for(int i = 0 ; i < tickets.length ; i++) {
			String start2 = tickets[i][0];
			String end2 = tickets[i][1];
			
			if(end.equals(start2) && !visit[i]) {
				visit[i] = true;
				recur(tickets, end2, count+1);
				visit[i] = false;
				str = str.substring(0, str.length()-4);
			}
		}
	}
}
```

## 이번 문제는 BFS/DFS 문제였습니다.

## 고려사항

1. 주어진 항공권은 모두 사용해야 합니다.

2. 항상 ICN 공항에서 출발합니다.

3. 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.(★)

아무래도 3번 조건이 제일 까다로웠습니다. 

## 하나씩 살펴봅시다.

```java
		for(int i = 0 ; i < tickets.length; i++) {
			String start = tickets[i][0];
			String end = tickets[i][1];
			
			if(start.equals("ICN")) {
				visit[i] = true;
				str = start + ",";
				recur(tickets, end, 1);
				visit[i] = false;
			}
		}
```
분기문에는 ticket의 길이만큼을 반복합니다. 이때 시작점, 끝점을 하나씩 받아보는데

ICN 공항에서 출발하기에 시작점이 `ICN`이라면 그쪽에서 시작하는 visit 처리 후,

경로를 저장할 `str`변수에 `ICN,` 을 저장합니다.

이제 재귀함수 쪽으로 들어가봅시다.

### 재귀 함수

```java
public static void recur(String[][] tickets, String end, int count) {
		str += end + ",";
		if(count==tickets.length) {
			list.add(str);
			return;
		}		
		for(int i = 0 ; i < tickets.length ; i++) {
			String start2 = tickets[i][0];
			String end2 = tickets[i][1];
			
			if(end.equals(start2) && !visit[i]) {
				visit[i] = true;
				recur(tickets, end2, count+1);
				visit[i] = false;
				str = str.substring(0, str.length()-4);
			}
		}
	}
```
들어가자 마자 `end + ","`가 `str`에 추가 되는데

이것은 시작점이 `ICN`에서 출발했을 때 도착지점인 `end`가 바로 다음의 시작점이기 때문입니다.

첫번째 예시를 들어서 보면 들어오자 마자의 상황에선 `str` 변수에는 `ICN, JFK,`가 들어갑니다. 

이후 기저부에는 ticket의 길이와 카운트가 같은가? = 티켓을 다 썻는가? 를 확인합니다.

같다면 `list`에 모든 경로를 다 탐색한 `str`이 추가됩니다.

### 내부의 분기문을 살펴봅시다.

티켓의 길이를 확인하면서 도착지점이 같은곳이며 방문한 적이 없는 곳이 있다면 그 곳을 들릅시다.

이러면서 재귀함수를 다시 들어갑니다. 재귀함수에서 돌아왔을 때 그 곳을 방문하지 않았던 것처럼 처리합니다.

`str`변수에 substring함수를 사용해서 `-4`만큼 빼주는 이유는 공항의 이름은 대문자 3글자이고 추가로 붙은 `,`를 빼주기 위해입니다.

이것 역시 방문하지 않았던 것 처럼 표시하기 위해서죠.

```java
		Collections.sort(list);
		String[] answer = list.get(0).split(",");
		return answer;
```

Collections.sort(`list`)가 이번 문제에서 생각해내는게 제일 까다로웠습니다.

> 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.(★)

3번째 조건을 만족하기 위해서 입니다.

2번째 예시를 확인하면 [ICN, SFO, ATL, ICN, ATL, SFO] 순으로 방문할 수도 있지만,
 
[ICN, ATL, ICN, SFO, ATL, SFO] 가 알파벳 순으로 앞섭니다.

`list`의 0번째 값은 `ICN,SFO,ATL,ICN,ATL,SFO,`

`list`의 1번째 갑은 `ICN,ATL,ICN,SFO,ATL,SFO,`가 들어있는 상태에서

sort 하게 되면 우리가 원하는 1번째 값이 0번으로 들어오게 됩니다.

이제 String 배열인 `answer`에 `list`의 정렬된 0번째 값을 ,로 구별해서 넣어줍니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/43164">여행경로</a>

---
