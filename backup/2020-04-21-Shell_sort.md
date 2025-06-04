---
layout: post
title: "Shell Sorting에 관해"
tags: [알고리즘]
comments: true
---
## Shell Sorting(쉘 정렬)
삽입정렬을 보완한 알고리즘이다.

삽입 정렬의 최대 문제점: 요소들이 삽입될 때, 이웃한 위치로만 이동
일정한 간격으로 떨어져있는 자료들끼리 집합을 만들어 각각의 집합 내에서 삽입정렬(Insert Sort)을 수행하는 작업을 반복한다.

### 순서
* 저 정렬해야 할 리스트를 일정한 기준에 따라 분류
* 연속적이지 않은 여러 개의 부분 리스트를 생성
* 각 부분 리스트를 삽입 정렬을 이용하여 정렬
* 모든 부분 리스트가 정렬되면 다시 전체 리스트를 더 적은 개수의 부분 리스트로 만든 후에 알고리즘을 반복
* 위의 과정을 부분 리스트의 개수가 1이 될 때까지 반복

### 장점
속적이지 않은 부분 리스트에서 자료의 교환이 일어나면 더 큰 거리를 이동한다.

따라서 교환되는 요소들이 삽입 정렬보다는 최종 위치에 있을 가능성이 높아진다.


부분 리스트는 어느 정도 정렬이 된 상태이기 때문에 부분 리스트의 개수가 1이 되게 되면

셸 정렬은 기본적으로 삽입 정렬을 수행하는 것이지만 삽입 정렬보다 더욱 빠르게 수행된다.


알고리즘이 간단하여 프로그램으로 쉽게 구현할 수 있다

### 시간복잡도

평균: T(n) = O(n^1.5)

최악의 경우: T(n) = O(n^2)
 

---

```java
import java.util.Arrays;
 
public class ShellSort {
    public static void intervalSort(int a[], int s, int e, int inter) {
        int j;
        for(int i=s+inter;i<=e;i=i+inter) {
            int item = a[i];
            for(j = i-inter;j>=s && item<a[j]; j=j-inter) {
                a[j+inter] = a[j];
            }
            a[j+inter] = item;
        }
    }
    public static void shellSort(int[] a, int size) {
        System.out.println("정렬할 원소:"+Arrays.toString(a));
        System.out.println("-----------------셸 정렬 수행------------------");
        int inter = size/2;
        while (inter >=1){
            for(int i=0;i<inter;i++) {
                intervalSort(a, i, size-1, inter);
            }
            System.out.println("interval = "+inter);
            for(int t=0;t<size;t++) {
                System.out.print(a[t]+" ");
            }
            System.out.println();
            inter = inter/2;
        }
    }
    
    public static void main(String[] args) {
        int[] list = {16, 7, 30, 3, 69, 9, 31, 23};
        int size = list.length;
        shellSort(list, size);
 
    }
 
}
```

---
