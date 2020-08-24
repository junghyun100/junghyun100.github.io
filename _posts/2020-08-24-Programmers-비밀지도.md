---
layout: post
title: "Programmers-비밀지도"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 비밀지도 문제에 관한 설명입니다.<br>

---

## 문제

네오는 평소 프로도가 비상금을 숨겨놓는 장소를 알려줄 비밀지도를 손에 넣었다. 

그런데 이 비밀지도는 숫자로 암호화되어 있어 위치를 확인하기 위해서는 암호를 해독해야 한다. 

다행히 지도 암호를 해독할 방법을 적어놓은 메모도 함께 발견했다.

지도는 한 변의 길이가 n인 정사각형 배열 형태로, 각 칸은 공백(" ) 또는벽(#") 

두 종류로 이루어져 있다.
전체 지도는 두 장의 지도를 겹쳐서 얻을 수 있다. 

각각 지도 1과 지도 2라고 하자. 지도 1 또는 지도 2 중 어느 하나라도 벽인 부분은 

전체 지도에서도 벽이다. 지도 1과 지도 2에서 모두 공백인 부분은 전체 지도에서도 공백이다.

지도 1과 지도 2는 각각 정수 배열로 암호화되어 있다.

암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백 부분을 0으로 부호화했을 때
 
얻어지는 이진수에 해당하는 값의 배열이다.

<img src="http://t1.kakaocdn.net/welcome2018/secret8.png">;

네오가 프로도의 비상금을 손에 넣을 수 있도록, 

비밀지도의 암호를 해독하는 작업을 도와줄 프로그램을 작성하라.

### 입력 형식
    입력으로 지도의 한 변 크기 n 과 2개의 정수 배열 arr1, arr2가 들어온다.
    
    1 ≦ n ≦ 16
    
    arr1, arr2는 길이 n인 정수 배열로 주어진다.
    
    정수 배열의 각 원소 x를 이진수로 변환했을 때의 길이는 n 이하이다. 
    
    즉, 0 ≦ x ≦ 2n - 1을 만족한다.

### 출력 형식

    원래의 비밀지도를 해독하여 '#', 공백으로 구성된 문자열 배열로 출력하라.

```java

class Solution {
    public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] answer = new String[n];
        for (int i = 0; i < n; i++) {
            String temp = Integer.toBinaryString(arr1[i] | arr2[i]);
            temp = String.format("%" + n + "s", temp);
            temp = temp.replaceAll("1", "#");
            temp = temp.replaceAll("0", " ");
            answer[i] = temp;
        }
        return answer;
    }
}

 ```

### 이번 문제는 이 문제는 비트 연산(Bitwise Operation)을 묻는 문제입니다.

 문제 예시를 봅시다.
 
 지도 `arr1`과 `arr2`에 있는 값들을 이진법으로 바꿔서 표현한다는 힌트가 있습니다.
 
 그리고 2개의 지도가 겹쳤을 때 (or 연산) 나타나는 결과를 벽이면 `#`, 빈공간이면 공백으로 `  `표현합니다.
 
 이것을 위해서 3가지를 사용합시다.
 
### toBinaryString

10진수를 2진수,8진수,16진수로 변환 할 때, Integer 클래스의 함수를 사용하면 쉽게 변환이 가능합니다.

Integer 클래스의 toBinaryString, toOctalString, toHexString 함수를 사용하면 

각각 2진수,8진수 16진수로 변환해줍니다.

```java

String binaryString = Integer.toBinaryString(45); //2진수
String octalString = Integer.toOctalString(45);   //8진수
String hexString = Integer.toHexString(45);       //16진수

System.out.println(binaryString); //101101
System.out.println(octalString);  //55
System.out.println(hexString);    //2d

```
### 비트 연산

비트 연산자는 비트(bit) 단위로 논리 연산을 할 때 사용하는 연산자입니다.

또한, 비트 단위로 전체 비트를 왼쪽이나 오른쪽으로 이동시킬 때도 사용합니다.

그 중  `&`와 `|`를 사용했습니다.

* `&` :	대응되는 비트가 모두 1이면 1을 반환함. (비트 AND 연산)
* `|` :	대응되는 비트 중에서 하나라도 1이면 1을 반환함. (비트 OR 연산)
 
```java
System.out.println(Integer.toBinaryString(20 & 28)); //10100
System.out.println(Integer.toBinaryString(20 | 28)); //11100
```
 
### String format

String클래스의 `format` 메소드는 지정된 위치에 값을 대입해서 문자열을 만들어 내는 용도이고,

여기서 자릿수가 고정된 String 형태를 만들고 싶다면 아래와 같이 사용합니다.

String.format("%(자릿수)s", str)를 이용합니다. s는 String을 뜻합니다.

```java
temp = temp.replaceAll("1", "#");
temp = temp.replaceAll("0", " ");
```
이렇게 고정된 format에서 0과 1로 이루어진 값들을 `#`과 `공백`으로 전환합니다!

<a href= "https://programmers.co.kr/learn/courses/30/lessons/17681">비밀지도</a>

---
