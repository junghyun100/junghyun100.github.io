---
layout: post
title: "Bubble Sorting에 관해"
tags: [알고리즘]
comments: true

---

## Bubble Sorting(거품 정렬)
두 인접한 원소를 검사하여 정렬하는 방법<br>
인접한 2개의 레코드를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환<br>

## Bubble Sorting(거품 정렬) 개념

코드가 단순하기 때문에 자주 사용된다.<br>
원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름<br>

---

```c

# include <stdio.h>
# define MAX_SIZE 5

void bubble_sort(int array[], int n);

int main(){
  int i;
  int n = MAX_SIZE;
  int array[n] = {7, 1, 5, 4, 3};

  bubble_sort(array, n);

  for(i=0; i<n; i++){
    printf("%d\n", array[i]);
  }
  
}

void bubble_sort(int array[], int n){
  int i, j, temp;

  for(i=n-1; i>0; i--){

    for(j=0; j<i; j++){
      if(array[j]>array[j+1]){
        temp = array[j];
        array[j] = array[j+1];
        array[j+1] = temp;
      }
    }
  }
}


```
```java
	static void bubble_sort(int arr[], int n) {
		for(int i=n-1; i>0; i--) {
			for(int j=0; j<i; j++) {//조심!!
				if(arr[j] > arr[j+1]) {
					int temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				}
			}
		}
	}
```
## 버블 정렬의 특징
- 장점
 1. 구현이 매우 간단하다.
- 단점
 1. 순서에 맞지 않은 요소를 인접한 요소와 교환한다. (가장 큰 문제점)
 2. 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다.
 3. 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.
- 일반적으로 자료의 교환 작업(swap)이 자료의 이동 작업(move)보다 복잡하기 때문에 버블 정렬은 그 단순성에도 불구하고 거의 쓰이지 않는다.

## 버블 정렬의 시간 복잡도 (분석)
- 비교 횟수
 - 최선, 평균, 최악의 경우 모두 일정하다. n-1, n-2, ... 2, 1 = n(n-1)/2
- 교환 횟수
 - 입력 자료가 역순으로 정렬되어 있는 최악의 경우, 한 번 교환하기 위해서 3번의 교환(SWAP)이 필요하므로 (비교 횟수 * 3) = 3n(n-1)/2
 - 입력 자료가 이미 정렬되어 있는 최상의 경우, 자료의 이동이 발생하지 않는다.
- 시간복잡도는 O(n^2)

출처

1. C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)

2. https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html
---
