---
layout: post
title: "Stable sort와 Unstable sort"
tags: [알고리즘]
comments: true
---

이번 Post는 Stable sort와 Unstable sort에 관한 내용임.

---

## 안정적 특성<br>
Stable sort와 Unstable sort을 구분하게 해주는 안정적 특성에 대해서 먼저 알아야 함.<br>
<strong>"정렬되지 않은 상태에서 같은 키값을 가진 원소의 순서가 정렬 후에도 유지되는 성질"</strong><br>
<p>
포커 게임를 예시로 들었을 때 번호를 기준으로 오름차순으로 정렬을 하고자 한다면?<br>
Stable sort와 Unstable sort의 결과값은 다르게 나타난다.
</p>

```c
초기 : 다7 하4 클2 스4 (다: 다이아, 하: 하트, 클: 클로버, 스: 스페이스)
```

### Stable sort<br>
정렬 후에도 원래의 순서가 유지되도록 함.<br>
### Unstable sort<br>
정렬 후에도 원래의 순서가 유지되는것을 보장할 수 없음.<br>

```c
결과 : 클2 하4 스4 다7(Stable sort)
결과 : 클2 스4 하4 다7(Unstable sort)
```
<p>
위 결과 2가지를 비교한다면 하트4와 스페이드4의 순서가 유지되거나 변경된다.<br>
알고리즘들에 따라서 안정 정렬, 불안정 정렬로 나뉘는데<br>
<br>
간단히 정리해서, 알고리즘 중에서는 <strong>퀵과 선택</strong>이 불안정정렬이고 나머지는 안정 정렬입니다.<br>
</p>
참고: <a hef="https://godgod732.tistory.com/10">https://godgod732.tistory.com/10</a> [졸지말고..]

---
