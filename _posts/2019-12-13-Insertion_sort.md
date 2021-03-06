---
layout: post
title: "Insertion Sorting에 관해"
tags: [알고리즘]
comments: true
---

## Insertion Sorting(삽입 정렬)
자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여,<br>
자신의 위치를 찾아 삽입하는 정렬 알고리즘<br>

## Insertion Sorting(삽입 정렬) 개념
<ol><li> 삽입 정렬은 두 번째 자료부터 시작하여 그 앞(왼쪽)의 자료들과 비교하여 <br>
삽입할 위치를 지정한 후 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘<br></li>
<li> 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료, 네 번째 자료는 세 번째, 두 번째, 첫 번째 자료와 비교한 후 자료가 삽입될 위치를 찾는다.<br>  </li>
<li>자료가 삽입될 위치를 찾았다면 그 위치에 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시킨다.<br>
<strong>처음 Key 값은 두 번째 자료부터 시작한다.</strong></li></ol>

---
C로 
```c

# include <stdio.h>
# define MAX_SIZE 5

void insertion_sort(int array[], int n);

void main(){
  int i;
  int n = MAX_SIZE;
  int array[n] = {8, 5, 6, 2, 4};

  insertion_sort(array, n);

  for(i=0; i<n; i++)
  {
    printf("%d\n", array[i]);
  }
}
// 삽입 정렬
void insertion_sort(int array[], int n)
{
  int i, j, key;

  for(i=1; i<n; i++)
  {
    key = array[i]; 
      for(j=i-1; j>=0 && array[j]>key ; j--)
     {
        array[j+1] = array[j]; 
     }
    array[j+1] = key;
  }
}


```
자바로 구현
```java
	static void insertion_sort(int arr[], int n) {
		for(int i=1; i<n; i++) {
			int insertValue = arr[i];
			int insertIndex = i;
			for(int j=i-1; j>=0 && arr[j] > insertValue; j--) {
				arr[j+1] = arr[j];
				insertIndex = j;
			}
			arr[insertIndex] = insertValue;
		}
	}
```
## 삽입 정렬의 특징
- 장점
 1. 안정적인 정렬 방법이다.
 2. 레코드 수가 적을 경우 알고리즘 자체가 매우 간단해서 다른 복잡한 정렬 알고리즘보다 유리할 수 있다.
- 단점
 1. 비교적 많은 레코드들의 이동을 포함한다.
 2. 레코드 수가 많고 레코드 크기가 클 경우에 적합하지 않다.

## 삽입 정렬의 시간 복잡도 (분석)
- 삽입 정렬의 복잡도는 입력 자료의 구성에 따라 달라진다.
- 최선의 경우 (이미 정렬되어 있는 경우)
 - 비교 횟수
  > 외부 루프는 n-1번
  > 각 단계에서 1번의 비교로 1번
- 최악의 경우 (입력 자료가 역순일 경우)
 - 각 단계에서 앞에 놓인 자료들은 전부 한 칸씩 뒤로 이동해야 한다.
 - 비교 횟수
  > 외부 루프 안의 각 반복마다 i번의 비교가 수행되므로 1 + 2 + ... + n-1 = n(n-1)/2 이다.
  > 외부 루프의 각 단게마다 i + 2번 이동하므로 n(n-1)/2 + 2(n-1) = (n^2 + 3n -3)/2이다.
- 총 시간 복잡도는 O(n^2)이다.

출처

1. C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)

2. https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html
---
