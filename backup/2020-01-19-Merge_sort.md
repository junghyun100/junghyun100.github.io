---
layout: post
title: "Merge Sorting에 관해"
tags: [알고리즘]
comments: true
---

## Merge Sorting(퀵 정렬)
여러 개의 정렬된 자료의 집합을 결합해 하나의 집합으로 만드는 정렬 방법이다.<br>
이 과정을 위해 정렬된 자료 리스트를 분할, 정렬(정복), 결합과정을 반복해야한다.<br>

* <strong>분할(Divide)</strong> : 자료들을 같은 크기의 2개의 부분 배열로 분할한다.<br>
* <strong>정복(Conquer)</strong> : 부분 배열을 정렬한다.<br>
부분 배열의 크기가 충분히 작지 않으면 순환 호출을 시켜 다시 분할 시킨다.<br>
* <strong>결합(Combine)</strong> : 정렬된 부분집합들을 하나로 결합한다.<br>

> 2-way병합 과 n-way병합<br>
 2개의 정렬된 집합을 하나의 집합으로 만드는 병합 방법을 2-way 병합이라고 하고,<br>
 n개의 정렬된 집합을 결합하여 하나의 집합으로 만드는 방법을 n-way 병합이라고 한다.<br>

<img src= "https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/%EB%B3%91%ED%95%A9%EC%A0%95%EB%A0%AC.PNG" width = "600px" height = "380px" >
 
---

```java

import java.util.Scanner;

public class mergeSort {
	static int result[] = new int[8];

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int array[] = { 10, 5, 15, 6, 9, 2, 18, 7 };
		for (int i = 0; i < array.length; i++) {
			System.out.print(array[i] + " ");
			System.out.print("");
		}
		System.out.println();
		divide(array, 0, array.length - 1);
	}

	public static void divide(int array[], int m, int n) {
		int middle;
		if (m < n) {
			middle = (m + n) / 2;
			divide(array, m, middle);
			divide(array, middle + 1, n);

			merge(array, m, middle, n);
		}
	}

	public static void merge(int array[], int m, int middle, int n) {
		int i = m;
		int j = middle + 1;
		int k = m;

		while (i <= middle && j <= n) {
			if (array[i] <= array[j]) {
				result[k] = array[i];
				i++;
			} else {
				result[k] = array[j];
				j++;
			}
			k++;
		}
		if (i > middle) {
			for (int t = j; t <= n; t++, k++) {
				result[k] = array[t];
			}
		} else {
			for (int t = i; t <= middle; t++, k++) {
				result[k] = array[t];
			}
		}

		for (int t = m; t <= n; t++) {
			array[t] = result[t];
		}
		System.out.println("병합 과정..");
		for(int l = 0; l<array.length;l++)
		{
			System.out.print(array[l]+" ");
		}
		System.out.println("");

	}

}



```

---
