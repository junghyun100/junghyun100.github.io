---
layout: post
title: "Programmers-단어 변환"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 단어 변환 문제에 관한 설명입니다.<br>

---

## 문제

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

> 1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
> 2. words에 있는 단어로만 변환할 수 있습니다.

예를 들어 begin이 hit, target가 cog, words가 [hot,dot,dog,lot,log,cog]라면 hit -> hot -> dot -> dog -> cog와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

* 각 단어는 알파벳 소문자로만 이루어져 있습니다.
* 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
* words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
* begin과 target은 같지 않습니다.
* 변환할 수 없는 경우에는 0를 return 합니다.

```java
package com.ssafy.ws;

import java.util.LinkedList;
import java.util.Queue;

class Solution {

    public static boolean Change(String word, String convertWord) {
        int count = 0;
        for (int i = 0; i < word.length(); i++) {
            if (word.charAt(i) != convertWord.charAt(i)) {
                count++;
            }
            if (count > 1) {
                return false;
            }
        }
        return true;
    }

    public int solution(String begin, String target, String[] words) {
        int answer = 0;
        boolean[] visited = new boolean[words.length];
        Queue<Word> queue = new LinkedList<>();
        queue.offer(new Word(begin, 0));
        while (!queue.isEmpty()) {
            Word current = queue.poll();
            if (current.word.equals(target)) {
                answer = current.count;
                break;
            }
            for (int i = 0; i < words.length; i++) {
                if (!visited[i] && Change(current.word, words[i])) {
                    queue.offer(new Word(words[i], current.count + 1));
                    visited[i] = true;
                }
            }
        }
        return answer;
    }

    static class Word {
        String word; 
        int count;

        Word(String word, int cnt) {
            this.word = word;
            this.count = cnt;
        }
    }
}
```

1. 큐에 시작 단어를 넣습니다.

2. 단어들을 불러오고 방문했던 단어가 아니라면, 현재 단어와 한글자씩 비교하고 변환이 가능한 경우 해당 글자를 큐에 추가하고 방문처리 해줍니다.

변경이 일어날때 마다 Word.count를 증가시켜 횟수를 늘린 채, 큐에 추가해줍니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/43163?language=java">단어 변환</a>

---
