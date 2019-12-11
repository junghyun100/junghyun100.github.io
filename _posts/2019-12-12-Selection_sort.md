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

---
