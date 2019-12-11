---
layout: post
title: "Selection Sorting에 관해"
tags: [알고리즘]
comments: true
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
