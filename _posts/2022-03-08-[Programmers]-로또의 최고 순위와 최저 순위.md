---
layout: post
title: "[Programmers] 로또의 최고 순위와 최저 순위"
tags: [Programmers]
comments: true

---

위 문제는 "로또의 최고 순위와 최저 순위"에 관한 설명이다.

---

## 문제 설명

로또 6/45(이하 '로또'로 표기)는 1부터 45까지의 숫자 중 6개를 찍어서 맞히는 대표적인 복권입니다. 

아래는 로또의 순위를 정하는 방식입니다.

![표1](../images/22년/0308/표1.png)

로또를 구매한 민우는 당첨 번호 발표일을 학수고대하고 있었습니다. 

하지만, 민우의 동생이 로또에 낙서를 하여, 일부 번호를 알아볼 수 없게 되었습니다. 

당첨 번호 발표 후, 민우는 자신이 구매했던 로또로 당첨이 가능했던 최고 순위와 최저 순위를 알아보고 싶어 졌습니다.

알아볼 수 없는 번호를 0으로 표기하기로 하고, 민우가 구매한 로또 번호 6개가 44, 1, 0, 0, 31 25라고 가정해보겠습니다. 

당첨 번호 6개가 31, 10, 45, 1, 6, 19라면, 당첨 가능한 최고 순위와 최저 순위의 한 예는 아래와 같습니다.

![표2](../images/22년/0308/표2.png)

* 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
* 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
  * 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
* 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
  * 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.

민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 

이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

## 제한사항

* lottos는 길이 6인 정수 배열입니다.
* lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
  * 0은 알아볼 수 없는 숫자를 의미합니다.
  * 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
  * lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
* win_nums은 길이 6인 정수 배열입니다.
* win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
  * win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
  * win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.

## 소스코드

<pre><code class="Java">
class Solution {
    public int[] solution(int[] lottos, int[] win_nums) {
        int correct =0;
        int zero =0;
        int[] rank ={6,6,5,4,3,2,1};

        for(int i=0;i<lottos.length;i++){
            for(int j=0;j<win_nums.length;j++){
                if(lottos[i]==0){
                    zero++;
                    break;
                }
                if(lottos[i]==win_nums[j]){
                    correct++;
                    break;
                }
            }
        }
        int[] answer = new int[2];
        answer[0]=rank[zero+correct];
        answer[1]=rank[correct];

        return answer;
    }
}
</code></pre>

## 문제풀이

문제를 잠깐 봤을 땐 어려워보였는데,

풀다보니 내용 자체는 쉬운편이었다.

![코드1](../images/22년/0308/코드1.png)

변수 correct(일치하는 갯수)와 zero(0으로 잘 보이지않는 갯수),

rank배열(순서대로 낙점 부터 1등까지) 생성한다.

![코드2](../images/22년/0308/코드2.png)

모든 로또에 대해서 우승 번호와 맞춰본다.

0번이 있다면 zero를 늘려주고,

숫자가 일치한다면 correct를 늘려준다.

![코드3](../images/22년/0308/코드3.png)

가장 잘맞췄을때는 0이 전부 맞다는 가정하이기에,

answer의 0번 방에는 zero와 correct를 맞춰준다.

answer의 1번 방에는 순수하게 일치하는 correct값만 맞춰준다.

### 문제링크 : 

<a href="https://programmers.co.kr/learn/courses/30/lessons/77484">[Programmers]로또의 최고 순위와 최저 순위</a> 

---
