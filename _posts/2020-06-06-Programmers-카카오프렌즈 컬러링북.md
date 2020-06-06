---
layout: post
title: "Programmers-카카오프렌즈 컬러링북"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 카카오프렌즈 컬러링북 문제에 관한 설명입니다.<br>

---

### 문제

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 

여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 

그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.

<img src="http://t1.kakaocdn.net/codefestival/apeach.png">

위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.

* 입력 형식
입력은 그림의 크기를 나타내는 m과 n, 그리고 그림을 나타내는 m × n 크기의 2차원 배열 picture로 주어진다. 제한조건은 아래와 같다.

1 <= m, n <= 100

picture의 원소는 0 이상 2^31 - 1 이하의 임의의 값이다.

picture의 원소 중 값이 0인 경우는 색칠하지 않는 영역을 뜻한다.
* 출력 형식
리턴 타입은 원소가 두 개인 정수 배열이다.

그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

```java
import java.util.*;
 
class Solution {
  public static int[] dx= {1, -1, 0, 0};
  public static int[] dy= {0, 0, 1, -1};
 
  public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;
        
        boolean[][] visited = new boolean[m][n];
        int[] answer = new int[2];
             
        Queue<Point> queue = new LinkedList<Point>();      
   
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (picture[i][j] != 0 && visited[i][j] == false) {
                 queue.offer(new Point(i, j));
                    visited[i][j] = true;
                    numberOfArea++;
                    int sizeOfArea = 1;
                    
                    while (!queue.isEmpty()) {
                           Point point = queue.poll();
                           int x = point.x;
                           int y = point.y;
                           
                           for (int k = 0; k < 4; k++) {
                               int nx = x + dx[k];
                               int ny = y + dy[k];
                                       
                               if (0 <= nx && nx < m && 0 <= ny && ny < n) {
                                   if (visited[nx][ny] == false && picture[nx][ny] == picture[x][y]) {
                                       visited[nx][ny] = true;
                                       queue.offer(new Point(nx, ny));
                                       sizeOfArea++;
                                   }
                               }
                               
                           }
                           
                       }
                        
                    if (maxSizeOfOneArea < sizeOfArea) {
                        maxSizeOfOneArea = sizeOfArea;
                        sizeOfArea = 0;
                    }
                    
                }
                   
            }
        }
        
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
      
        return answer;
    }
}

class Point {
    int x;
    int y;
    
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

```

<a href= "https://programmers.co.kr/learn/courses/30/lessons/1829"></a>

---
