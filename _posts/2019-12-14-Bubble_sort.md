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

---
