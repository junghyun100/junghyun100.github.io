---
layout: post
title: "Programmers-스킬트리"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 스킬트리 문제에 관한 설명입니다.<br>

---

## 문제

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 스파크 → 라이트닝 볼트 → 썬더일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 

따라서 스파크 → 힐링 → 라이트닝 볼트 → 썬더와 같은 스킬트리는 가능하지만, 썬더 → 스파크나 라이트닝 볼트 → 스파크 → 힐링 → 썬더와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리1를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

## 제한조건

* 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
* 스킬 순서와 스킬트리는 문자열로 표기합니다.
* 예를 들어, C → B → D 라면 CBD로 표기합니다
* 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
* skill_trees는 길이 1 이상 20 이하인 배열입니다.
* skill_trees의 원소는 스킬을 나타내는 문자열입니다.
* skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

|skill|skill_trees|return|
|:---:|:---------:|:----:|
|"CBD"|["BACDE", "CBADF", "AECB", "BDA"]|2|


### 해결 방법

```java
class Solution {
    public int solution(String skill, String[] skill_trees) {
	int answer = skill_trees.length;
	int postIndex = 0, currentIndex = 0;
	for (int i = 0; i < skill_trees.length; i++) {
		postIndex = skill_trees[i].indexOf(skill.charAt(0));
		for (int j = 1; j < skill.length(); j++) {
			currentIndex = skill_trees[i].indexOf(skill.charAt(j));
			if((postIndex > currentIndex && currentIndex != -1) || (postIndex == -1 && currentIndex != -1)) {
				answer--;
				break;
			} 
			postIndex = currentIndex;
		}
	}
	return answer;
}
}
```

## 이번 문제는 문자열 문제였습니다.

## 해결책

1. 처음에 첫번째 스킬트리의 값을 `postIndex`에 넣고,  2번째 스킬부터 `currentIndex`에 넣어 비교합니다.

2. 불가능하다면 `answer`의 갯수를 -1 시키고 for문을 break 시키고 다음 경우를 확인합니다.

3. 가능한 경우라면 `currentIndex`를 `postIndex`에 넣고 반복합니다.

처음에는 문제를 이해하지 못해서 감을 잡지 못해서 시작하는데 어려움이 있었습니다.

## 예

예시를 들어서 설명해 봅시다.

`skill`에는 제가 사용하는 스킬 `CBD`가 있습니다.

`skill_trees`에는 다양한 스킬트리가 있고,

가장 갯수가 많이 겹치는 경우를 세기 위해 `answer`를 사용합시다.

`postIndex`와 `currentIndex`는 해결책 1번에 나와있듯이 사용합니다.

### 분기문 안쪽을 살펴봅시다!

```java
for (int i = 0; i < skill_trees.length; i++) {
		postIndex = skill_trees[i].indexOf(skill.charAt(0));
		for (int j = 1; j < skill.length(); j++) {
			currentIndex = skill_trees[i].indexOf(skill.charAt(j));
			if((postIndex > currentIndex && currentIndex != -1) || (postIndex == -1 && currentIndex != -1)) {
				answer--;
				break;
			} 
			postIndex = currentIndex;
		}
	}
```
스킬트리의 종류 갯수만큼 반복을 시키고,

`postIndex`에는 스킬트리의 `i`번째의 인덱스가 `skill`의 첫번쨰 index와 같은가를 판단합니다.

이후부터는 그 스킬트리의 끝까지 를 `currentIndex`에 넣고 확인을 합니다.

이전 `postIndex`보다 `currentIndex`가 작거나 길이가 끝이났고, IndexOf로 인해 해당 값은 존재한다면? 

해당 스킬트리는 배우지 못하는 경우이므로, `answer`를 줄입니다.

또는 해당 값자체가 존재하지 않는다면 answer를 마찬가지로 줄여줍니다.

그런경우 거기까지만 하고 다음 스킬트리와 비교합니다.

이렇게 나온 `answer`의 갯수는 불가능한 스킬트리가 빠진 모든 갯수가 담겨있습니다.

<a href= "https://school.programmers.co.kr/courses/10313/lessons/63163">스킬트리</a>

---
