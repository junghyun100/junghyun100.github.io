---
layout: post
title: "Quick Sorting에 관해"
tags: [알고리즘]
comments: true
---

## Quick Sorting(퀵 정렬)
퀵정렬은 임의의 pivot 값을 기준으로 pivot 의 좌측에는 pivot 보다 작은값을 두고,<br>
우측에는 pivot 보다 큰 값을 두고자 하는 알고리즘.<br>

## Quick Sorting(퀵 정렬) 절차
퀵 정렬은 분할 정복(divide and conquer) 방법을 통해 리스트를 정렬한다. <br>

<ol><li>리스트 가운데서 하나의 원소를 고른다. 이렇게 고른 원소를 <strong>피벗</strong>이라고 한다.</li>
<li> 피벗 앞에는 피벗보다 값이 작은 모든 원소들이 오고,<br> 피벗 뒤에는 피벗보다 값이 큰 모든 원소들이 오도록 피벗을 기준으로 리스트를 둘로 나눈다.<br>  </li>
<li>이렇게 리스트를 둘로 나누는 것을 분할이라고 한다. 분할을 마친 뒤에 피벗은 더 이상 움직이지 않는다.<br>
<li>분할된 두 개의 작은 리스트에 대해 재귀(Recursion)적으로 이 과정을 반복한다.<br> 재귀는 리스트의 크기가 0이나 1이 될 때까지 반복된다.</li></ol>

 


---

```java

public class Quick {
    
    public void sort(int[] data, int l, int r){
        int left = l; //array[처음];
        int right = r;//array[끝번호];
        int pivot = array[(l+r)/2]; // pivot 가운데 설정 (최악의 경우 방지)
        
        do{
            while(array[left] < pivot) 
              left++;
            while(array[right] > pivot) 
              right--;
            if(left <= right){    
                int temp = array[left];
                array[left] = array[right];
                array[right] = temp;
                left++;
                right--;
            }
        }while (left <= right);
        
        if(l < right) 
          sort(array, l, right);
        if(r > left) 
          sort(array, left, r);
    }
    
    public static void main(String[] args) {
        
        int array[] = {20, 25, 12, 38, 7, 60};
        
        Quick quick = new Quick();
        quick.sort(array, 0, array.length - 1);
        for(int i=0; i<array.length; i++){
            System.out.println(array[i]);
        }
    }
}


```

---
