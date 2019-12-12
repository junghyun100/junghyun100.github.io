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
### 1. 삽입 정렬은 두 번째 자료부터 시작하여 그 앞(왼쪽)의 자료들과 비교하여 <br>
삽입할 위치를 지정한 후 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘<br>
### 2. 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료, 네 번째 자료는 세 번째, 두 번째, 첫 번째 자료와 비교한 후 자료가 삽입될 위치를 찾는다.<br>
자료가 삽입될 위치를 찾았다면 그 위치에 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시킨다.
처음 Key 값은 두 번째 자료부터 시작한다.

---

```c
# include <stdio.h>
# define MAX_SIZE 5

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

---
