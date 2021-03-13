---
layout: post
title: "Programmers-쿼드압축 후 개수 세기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 쿼드압축 후 개수 세기 문제에 관한 설명입니다.

---

## 문제 설명

0과 1로 이루어진 2n x 2n 크기의 2차원 정수 배열 arr이 있습니다. 

당신은 이 arr을 쿼드 트리와 같은 방식으로 압축하고자 합니다. 

구체적인 방식은 다음과 같습니다.

당신이 압축하고자 하는 특정 영역을 S라고 정의합니다.

만약 S 내부에 있는 모든 수가 같은 값이라면, S를 해당 수 하나로 압축시킵니다.

그렇지 않다면, S를 정확히 4개의 균일한 정사각형 영역(입출력 예를 참고해주시기 바랍니다.)으로

 쪼갠 뒤, 각 정사각형 영역에 대해 같은 방식의 압축을 시도합니다.
arr이 매개변수로 주어집니다. 

위와 같은 방식으로 arr을 압축했을 때, 

배열에 최종적으로 남는 0의 개수와 1의 개수를 배열에 담아서 return 하도록 

solution 함수를 완성해주세요.

### 제한 사항

* arr의 행의 개수는 1 이상 1024 이하이며, 2의 거듭 제곱수 형태를 하고 있습니다. <br>
즉, arr의 행의 개수는 1, 2, 4, 8, ..., 1024 중 하나입니다.
	* arr의 각 행의 길이는 arr의 행의 개수와 같습니다. 즉, arr은 정사각형 배열입니다.
	* arr의 각 행에 있는 모든 값은 0 또는 1 입니다.

### 입출력 예 설명

입출력 예 #1

|arr|result|
|:----:|:-----:|
|[[1,1,0,0],[1,0,0,0],[1,0,0,1],[1,1,1,1]]|[4,9]|
|[[1,1,1,1,1,1,1,1],[0,1,1,1,1,1,1,1],[0,0,0,0,1,1,1,1],[0,1,0,0,1,1,1,1],[0,0,0,0,0,0,1,1],[0,0,0,0,0,0,0,1],[0,0,0,0,1,0,0,1],[0,0,0,0,1,1,1,1]]|[10,15]|

```java
class Solution {
    private static int zeroCount;
    private static int oneCount;
    public int[] solution(int[][] arr) {
        int[] answer = new int[2];
        zeroCount = 0;
        oneCount = 0;
        dfs(arr, arr.length, 0, 0);
        answer[0] = zeroCount;
        answer[1] = oneCount;
        return answer;
    }
    private static void dfs(int[][] arr, int size, int x, int y){
        if(size == 1){
            if(arr[x][y] == 1){
                oneCount++;
            }
            else{
                zeroCount++;
            }
            return;
        }
        if(compress(arr, size, x, y)){
            return;
        }
        dfs(arr, size / 2, x, y);
        dfs(arr, size / 2, x + size / 2, y);
        dfs(arr, size / 2, x, y + size / 2);
        dfs(arr, size / 2, x + size / 2, y + size / 2);
    }
    public static boolean compress(int[][] arr, int size, int x, int y){
        int temp = arr[x][y];
        for(int i = x; i < x + size; i++){
            for(int j = y; j < y + size; j++){
                if(temp != arr[i][j]){
                    return false;
                }
            }
        }
        if(temp == 0){
            zeroCount += 1;
        }else{
            oneCount += 1; 
        }
        return true;
    } 
}
```

### 이번 문제는 분할정복였습니다.

이전에 백준에서 풀었던 쿼드트리 문제와 유사했습니다.

dfs에 들어가는 매게변수로는 `arr배열(맵)`, `size`, `x좌표`와 `y좌표`가 들어가게 됩니다.

처음 실행하게 된다면 `arr`과 `size(arr의 길이)`, `0`, `0`이 값으로 들어가게 됩니다.

좌측상단에서 전체적인 `arr`에 대해서 분할 정복을 실행하게 됩니다.

```java
    private static void dfs(int[][] arr, int size, int x, int y){
        if(size == 1){
            if(arr[x][y] == 1){
                oneCount++;
            }
            else{
                zeroCount++;
            }
            return;
        }
        if(compress(arr, size, x, y)){
            return;
        }
        dfs(arr, size / 2, x, y);
        dfs(arr, size / 2, x + size / 2, y);
        dfs(arr, size / 2, x, y + size / 2);
        dfs(arr, size / 2, x + size / 2, y + size / 2);
    }
```

dfs부분입니다.

일단 `size`가 1인 경우, 즉 가장 작은 크기의 `size`를 가질 때 해당 값을

0인가 1인가를 확인 후 그 갯수를 올려줍니다.

그 다음으로는 `compress 메소드`를 확인해서 압축이 되는가 안되는가를 확인합니다.

잠시 `compress 메소드`를 살펴봅시다.

```java
    public static boolean compress(int[][] arr, int size, int x, int y){
        int temp = arr[x][y];
        for(int i = x; i < x + size; i++){
            for(int j = y; j < y + size; j++){
                if(temp != arr[i][j]){
                    return false;
                }
            }
        }
        if(temp == 0){
            zeroCount += 1;
        }else{
            oneCount += 1; 
        }
        return true;
    } 
```

처음 확인하고자 넘겨받은 `x좌표`와 `y좌표`를 보고 `temp`에 그 숫자를 저장합니다.

1이나 0의 값이 들어왔을 때, 지금 위치부터 `size`까지의 범위만큼을 둘러봅니다.

만약 다른색상이 있다면? 그것은 압축되는게 아니라 다시 안쪽으로 들어가서 살펴봐야합니다.

그래서 `false`를 리턴하게 되고 아니라면 `temp`의 값에 따라 해당 숫자를 늘려줍니다.

그리고 압축이 되었다는 뜻으로 `true`를 리턴합니다.

```java
        dfs(arr, size / 2, x, y);
        dfs(arr, size / 2, x + size / 2, y);
        dfs(arr, size / 2, x, y + size / 2);
        dfs(arr, size / 2, x + size / 2, y + size / 2);
```

다시 dfs로 돌아왔습니다.

분할 정복을 하는 부분입니다.

`arr`으로 전체적인 맵을 넘겨주고, 사이즈는 반으로 줄입니다.

그리고 `x`, `y`좌표는 각 경우에 따라 

* 가장 좌측 상단
* 상단 중앙
* 중앙 좌측
* 정 중앙

이렇게 시작하도록 합니다.

모든 과정이 끝나게 된다면 `return` 해서 다시 `solution`으로 넘어가게 됩니다.

여태까지 나왔던 모든 0과, 1에 대한 숫자 갯수는 

`zeroCount`와 `oneCount`에서 기록했기 때문에

반환하려는 `answer` 변수의 0번 방과 1번 방에 기록해줍니다.

그리고 `answer`을 `return` 합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/68936">Programmers-쿼드압축 후 개수 세기</a>

---
