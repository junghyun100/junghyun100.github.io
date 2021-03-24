---
layout: post
title: "Programmers-캐시"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 캐시 문제에 관한 설명입니다.

---

## 문제 설명

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 

해당 도시와 관련된 맛집 게시물들을 

데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.

이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 

서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 

제이지가 작성한 부분 중 데이터베이스에서 

게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.

어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고,

제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 

캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 

캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식

* 캐시 크기(`cacheSize`)와 도시이름 배열(`cities`)을 입력받는다.
* `cacheSize`는 정수이며, 범위는 0 ≦ cacheSize ≦ 30 이다.
* `cities`는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
* 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식

* 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

### 조건

* 캐시 교체 알고리즘은 `LRU`(Least Recently Used)를 사용한다.
* `cache hit`일 경우 실행시간은 `1`이다.
* `cache miss`일 경우 실행시간은 `5`이다.

```java
import java.util.*;

class Solution {
    public int solution(int cacheSize, String[] cities) {
         int answer = 0;
        LinkedList<String> cache = new LinkedList<>();
        for(String cityTemp : cities) {
            String city = cityTemp.toUpperCase();
            if(cache.contains(city)) {
                cache.remove(city);
                cache.add(city);
                answer++;
            } else if(cache.size() < cacheSize) {
                cache.add(city);
                answer += 5;
            } else {
                if(cacheSize > 0) {
                    cache.remove(0);
                    cache.add(city);
                }
                answer += 5;
            }
        }
        
        return answer;
    }
}
```

# 이번 문제는 LRU 알고리즘을 알아야합니다.

## LRU란?

> Least Recently Used : 페이지 교체 시 가장 오래 전에 참조가 이루어진 페이지를 내쫒는 방식

<img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter9/9_15_LRU_PageReplacement.jpg">{: .center-image}

`cities`를 하나씩 순회합니다.

도시들의 이름은 대소문자를 가리지 않기 때문에 `UpperCase` 혹은 `LowerCase`를 해줬습니다.

만약 캐시에 도시가 포함되어있다면 캐시에서 없애주고,

최근에 사용했다는 뜻으로 다시 집어넣어줍니다.

그리고 그 시간값인 `answer`를 늘려줍니다.

아직 `캐시의 사이즈`가 `정해진 캐시 사이즈`보다 작다면

캣치미스가 일어나므로 안에 넣어줌과 동시에 `5`초 늘려줍니다.
 
만약 캐시에 포함되지 않는다면

기본적으로 미스가 일어나므로 `5`초 늘려주고,

사이즈가 0보다 크다면 `0번 방`을 지워주고 `city`값을 넣어줍니다.


<a href= "https://programmers.co.kr/learn/courses/30/lessons/17680">Programmers-캐시</a>

---
