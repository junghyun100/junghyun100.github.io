---
layout: post
title: "Fibonacci 구현방법 3가지의 차이점은?(시간복잡도등)"
tags: [개념]
comments: true
---
 
해당 Post는 Fibonacci 구현방법 3가지의 차이점을 정리한 파일입니다.

---

# Fibonacci 수열
첫째 및 둘째 항이 1이며 그 뒤의 모든 항은 바로 앞 두 항의 합인 수열이다.
<img src= "https://shoark7.github.io/assets/img//algorithm/fibonacci-logo.png" height="240px" width="400px">
- 반환형을 double로 바꾸는 것을 추천
-> 50 이상의 수열에서는 int형 범위 초과

### 1. 재귀(recursion)

- O(2^n)
- 연속 함수 호출로 인한 stack overflow 문제 발생 가능성 높음
```
int fibonacci(int x){ 
    if(x <= 2){
        return 1; 
    } 
    return fibonacci(x-1) + fibonacci(x-2); }
```

### 2. 동적 프로그래밍(Dynamic Programming)

- O(n)
```
int fibonacci(int n) {
    int arr[MAX_SIZE];
    if(n < 2) return n;
    if(arr[n] > 0) return arr[n];
    else return arr[n] = fibonacci(n-1) + fibonacci(n-2);
}
```
- 연속 함수 호출로 인한 stack overflow 문제 발생 가능성 높음
- 메모이제이션을 통해 동일한 계산이 반복되는 것을 방지
- 계산된 값을 저장할 테이블 존재

### 3. 반복문(for 문)

- O(n)
```
int fibonacci(int n) {
    if(n < 2) return n;
    else {
        int i, tmp, cur = 1, last = 0;
        for (i = 2; i <= n; i++) {
            tmp = cur;
            cur += last;
            last = tmp;
        }
        return cur;
    }
}
```
- 메모이제이션을 통해 동일한 계산이 반복되는 것을 방지
- 계산된 값을 저장할 테이블 존재

