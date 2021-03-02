---
layout: post
title: "Programmers-삼각 달팽이"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 삼각 달팽이 문제에 관한 설명입니다.

---

## 문제 설명

정수 n이 매개변수로 주어집니다. 

다음 그림과 같이 밑변의 길이와 높이가 n인 삼각형에서 

맨 위 꼭짓점부터 반시계 방향으로 달팽이 채우기를 진행한 후, 

첫 행부터 마지막 행까지 모두 순서대로 합친 

새로운 배열을 return 하도록 solution 함수를 완성해주세요.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e1e53b93-dcdf-446f-b47f-e8ec1292a5e0/examples.png">

### 제한 사항

* n은 1 이상 1,000 이하입니다.

### 입출력 예 설명

입출력 예 #1

|n|result|
|:--------:|-------------|
|4|[1,2,9,3,10,8,4,5,6,7]|
|5|[1,2,12,3,13,11,4,14,15,10,5,6,7,8,9]|
|6|[1,2,15,3,16,14,4,17,21,13,5,18,19,20,12,6,7,8,9,10,11]|

```java

class Solution {
    static int[] answer;
    static int[][] map;
    public int[] solution(int n) {
        answer = new int[(n*(n+1))/2];
        map = new int[n][n];
        inputValues(n);
        transfer(n);
        return answer;
    }
    private static void inputValues(int n){
        int x = -1;
        int y = 0;
        int input = 1;
        for(int i = 0; i < n; i++){
            for(int j = i ; j < n; j++){
                if(i % 3 == 0){
                    x++;
                } else if(i%3 == 1){
                    y++;
                } else if(i%3 == 2){
                    x--;
                    y--;
                }
                map[x][y] = input++ ;
            }
        }
    }
    private static void transfer(int n){
        int input = 0;
        for(int i = 0 ; i < n; i++){
            for(int j = 0 ; j < n; j++){
                if(map[i][j]==0)
                    break;
                answer[input++] = map[i][j];
            }
        }
    }
}
```

### 이번 문제를 풀기 위해 규칙을 찾아야 했습니다.

<img src="/images/2021년/0302/설명.PNG">

원래 그림이 가운데로 정렬된 삼각형이었는데,

위 그림은 왼쪽으로 몰린 사진입니다.

이렇게 했을 때 배열에 대한 느낌을 잘 표현할 수 있었습니다.

사각형으로 돌아가면서 푸는 달팽이 문제가 있었는데 

그것에서 연장된 것이나 다름없었습니다.

##  inputValues(int n)
```java
    private static void inputValues(int n){
        int x = -1;
        int y = 0;
        int input = 1;
        for(int i = 0; i < n; i++){
            for(int j = i ; j < n; j++){
                if(i % 3 == 0){
                    x++;
                } else if(i%3 == 1){
                    y++;
                } else if(i%3 == 2){
                    x--;
                    y--;
                }
                map[x][y] = input++ ;
            }
        }
    }
```

`3가지의 경우`를 고려해야했습니다.

* ↓ : 처음에 1은 왼쪽 위부터 시작해서 아래로 쭉내려옵니다.

* → : 끝까지 닿았다면 방향을 오른쪽으로 쭉 이동합니다.

* ↖ : 오른쪽의 끝에 닿았다면 왼쪽 윗방향으로 이동합니다.

이것을 `i`가 0부터 시작을 해서 `j`가 끝까지 닿았을때 `i`가 늘어나는 것으로

`i를 3`으로 나눈다면 나머지가 0일때 아랫방향(첫 시작 포함)

나머지가 1일때 우측방향,

나머지가 2일때 좌상방향으로 진행합니다.

아랫방향으로 갈때는 `x`에 대한 값만 하나씩 늘려주면 됩니다.

우측방향으로 갈때는 `y`에 대한 값을 하나씩 늘려주면 됩니다.

좌상방향으로 이동할 때는 위로 가기 위해 `x`를 하나 줄이고, 

좌측으로 가기 위해 `y`를 하나 줄입니다. 

`input`은 1부터 시작해서 하나씩 넣어주고 증가시킵니다.

## transfer(int n)

```java
    private static void transfer(int n){
        int input = 0;
        for(int i = 0 ; i < n; i++){
            for(int j = 0 ; j < n; j++){
                if(map[i][j]==0)
                    break;
                answer[input++] = map[i][j];
            }
        }
    }
```
값을 옮기는 매서드입니다.

현재 `map`에는 값들이 2차원 배열로 되어있으나

정답으로 `return`시킬 `answer`는 1차원 배열입니다.

따라서 하나씩 값을 옮겨줍니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/68645">삼각 달팽이</a>

---
