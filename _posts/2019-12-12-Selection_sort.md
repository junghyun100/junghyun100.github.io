---
layout: post
title: "Selection Sorting에 관해"
tags: [알고리즘]
comments: true
---
## Selection Sorting(선택 정렬)
선택 정렬(選擇整列, selection sort)은 제자리 정렬 알고리즘의 하나

순서
<ol><li>주어진 리스트 중에 최소값을 찾는다.</li>
<li>그 값을 맨 앞에 위치한 값과 교체한다.</li>
<li>맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.</li></ol>

-출처 위키백과

간단히 말하자면 앞부분 부터 작은 순서(또는 큰순서)로 정해서 채워나간다.

---

```c
# include <stdio.h>
# define MAX_SIZE 5

void selection_sort(int array[], int n);

int main(){
  int i;
  int n = MAX_SIZE;
  int array[n] = {7, 3, 6, 9, 5};

  selection_sort(array, n);

  for(i=0; i<n; i++){
    printf("%d\n", array[i]);
  }
}

// 선택 정렬
void selection_sort(int array[], int n){
  int i, j, min, temp;

  for(i=0; i<n-1; i++){
    min = i;

    for(j=i+1; j<n; j++){
      if(array[j]<array[min])
        min = j;
    }

    if(i != min){
    	temp = array[i];
    	array[i]=array[min];
		array[min]=temp;
    }
  }
}
```
```java
	static void selection_sort(int arr[], int n) {
		for(int i=0; i<n-1; i++) {
			int select = i;
			for(int j=i+1; j<n; j++) {
				int least = j;
				if(arr[select] > arr[least]) {
					int temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;
				}
			}
		}
	}
```
## 선택 정렬의 특징
- 장점
 1. 자료의 이동 횟수가 미리 결정된다는 점
- 단점
 1. 값이 같은 레코드가 있는 경우 상대적인 위치가 변경될 수 있어서 안정성을 만족하지 못한다는 점

## 선택 정렬의 시간 복잡도 (분석)
- 비교 횟수
 1. 외부 루프는 n-1번 실행
 2. 내부 루프는 n-1, n-2, ... , 2, 1번 실행
- 교환 횟수
 1. 외부 루프의 실행 횟수와 같으며 한번 교환하기 위해 3번의 이동이 필요하므로 전체 이동 횟수는 3(n-1)이 된다.
- 시간복잡도
 1. (n-1) + (n-2) + ... + 2 + 1 = n(n-1)/2 = O(n^2)
 
 출처
 
1. C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)

2. https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html
---
