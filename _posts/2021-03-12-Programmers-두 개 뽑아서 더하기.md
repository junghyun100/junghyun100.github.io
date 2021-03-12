---
layout: post
title: "Programmers-두 개 뽑아서 더하기"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 두 개 뽑아서 더하기 문제에 관한 설명입니다.

---

## 문제 설명

정수 배열 numbers가 주어집니다. 

numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 

모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

### 제한 사항

* numbers의 길이는 2 이상 100 이하입니다.
* numbers의 모든 수는 0 이상 100 이하입니다.

### 입출력 예 설명

입출력 예 #1

|numbers|result|
|:----:|:-----:|
|[2,1,3,4,1]|[2,3,4,5,6,7]|
|[5,0,2,7]|[2,5,7,9,12]|

```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        HashSet<Integer> hashSet = new HashSet<Integer>();
        for(int i = 0 ; i < numbers.length; i++){
            for(int j = i+1 ; j < numbers.length; j++){
                    hashSet.add(numbers[i] + numbers[j]);
            }
        }
        List list = new ArrayList(hashSet);
        Collections.sort(list);
        int[] answer = new int[list.size()];
        for(int i = 0 ; i < list.size();i++){
            answer[i] = (int)list.get(i);
        }
        return answer;
    }
}
```

### 이번 문제는 구현문제였습니다.

더하는 수들 중에서 결과값으로 나오는데 중복되는 값은 없었습니다.

그래서 저는 `Set`을 사용하는 것이 더 좋다고 생각했습니다.

`HashSet`을 우선 사용했습니다.

그리고는 이중 반복문을 이용해서 각 값들의 합을 `hashSet`에 넣습니다.

그렇게 되면 자동적으로 중복되는 값이 걸러지게 됩니다.

이제 값들은 다 도출되었으니 정렬을 해야합니다.

정렬을 위해서 `Collections.sort`를 사용하면 된다고 생각했는데,

`hashSet`은 동작하지 않습니다.

그에 대한 이유로는 `Collections.sort`의 메소드 인자값이 `List`타입이기 때문입니다.

그래서 `hashSet`를 `List` 형식으로 바꿔줬습니다.

이제 `List`형식으로 변환이 되었으니 `Collections.sort`에 넣습니다.

정렬된 값들을 하나씩 `answer` 배열에 넣어줬습니다.

`list.get(i)`를 사용해서 하나씩 꺼내다보면 해당 하는 

객체가 `Object형` 이라는라는 사실을 알게됩니다.

그래서 `int형`으로 캐스팅 해줬습니다.

그리고 `answer`을 `return`해줬습니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/68644#">Programmers-두`개 뽑아서 더하기</a>

---
