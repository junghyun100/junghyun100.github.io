---
layout: post
title: "Programmers - 방문 길이"
tags: [Programmers]
comments: true
---

위 문제는 Programmers 방문 길이 문제에 관한 설명입니다.

---

## 문제 설명

게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

* U: 위쪽으로 한 칸 가기

* D: 아래쪽으로 한 칸 가기

* R: 오른쪽으로 한 칸 가기

* L: 왼쪽으로 한 칸 가기

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ace0e7bc-9092-4b95-9bfb-3a55a2aa780e/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B51_qpp9l3.png">{: .center-image}

예를 들어, "ULURRDLLU"로 명령했다면

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/668c7458-e184-472d-9d32-f5d2acca759a/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B52_lezmdo.png">{: .center-image}

1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/08558e36-d667-4160-bfec-b754c78a7d85/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B53_sootjd.png">{: .center-image}

8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/a52af28e-5835-438b-9f40-5467ebf9bf03/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B54_hlpiej.png">{: .center-image}

이때, 우리는 게임 캐릭터가 지나간 길 중 캐릭터가 처음 걸어본 길의 길이를 구하려고 합니다. 

예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 

캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. 

(8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

### 제한사항

- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.

```java
class Solution {
    public static int[] dx = {0, 0, 1, -1};
    public static int[] dy = {-1, 1, 0, 0};
    public static boolean[][][][] visit = new boolean[11][11][11][11];

    public int solution(String dirs) {
        int x = 5;
        int y = 5;
        
        int nextX = 5;
        int nextY = 5;
        
        int answer = 0;
        
        int directionIndex = 0;
        for(int i = 0 ; i < dirs.length(); i++){
            if(dirs.charAt(i) == 'U')
                directionIndex = 0;
            else if(dirs.charAt(i) == 'D')  
                directionIndex = 1;
            else if(dirs.charAt(i) == 'R')  
                directionIndex = 2;
            else if(dirs.charAt(i) == 'L')  
                directionIndex = 3;
            
            nextX += dx[directionIndex];
            nextY += dy[directionIndex];
            if(nextX < 0 || nextY < 0 || nextX > 10 || nextY > 10){
                nextX -= dx[directionIndex];
                nextY -= dy[directionIndex];
                continue;
            }
            if(!visit[x][y][nextX][nextY] && !visit[nextX][nextY][x][y]){
                visit[x][y][nextX][nextY] = true;
                visit[nextX][nextY][x][y] = true;
                answer ++;
            }
            x = nextX;
            y = nextY;
        }
        return answer;
    }
}
```

# 이번 문제는 시뮬레이션 문제였습니다.

```java
    public static int[] dx = {0, 0, 1, -1};
    public static int[] dy = {-1, 1, 0, 0};
    public static boolean[][][][] visit = new boolean[11][11][11][11];

    ....

        int x = 5;
        int y = 5;
        
        int nextX = 5;
        int nextY = 5;
```

다음과 같이 짠 이유는 `U`, `D`, `R`, `L`의 순서로 문자를 받았을 때,

dx와 dy를 통해 이동하는 방식을 하기 위해서이고,

 -5 ~ 5의 범위가 음수로 인해서 표현하기 어렵기 때문에 0 ~ 10으로 설정했습니다.

0 ~ 10까지의 범위를 위해 11사이즈로 정해줍니다.

```java
        for(int i = 0 ; i < dirs.length(); i++){
            if(dirs.charAt(i) == 'U')
                directionIndex = 0;
            else if(dirs.charAt(i) == 'D')  
                directionIndex = 1;
            else if(dirs.charAt(i) == 'R')  
                directionIndex = 2;
            else if(dirs.charAt(i) == 'L')  
                directionIndex = 3;
            
            nextX += dx[directionIndex];
            nextY += dy[directionIndex];
            if(nextX < 0 || nextY < 0 || nextX > 10 || nextY > 10){
                nextX -= dx[directionIndex];
                nextY -= dy[directionIndex];
                continue;
            }
            if(!visit[x][y][nextX][nextY] && !visit[nextX][nextY][x][y]){
                visit[x][y][nextX][nextY] = true;
                visit[nextX][nextY][x][y] = true;
                answer ++;
            }
            x = nextX;
            y = nextY;
        }
```

게임 캐릭터를 방향에 맞게 일단 움직입니다.

nextX에는 다음 방향으로 이동한 상태로 두고,

이것이 범위를 벗어난다면 되돌려 줍니다.

만약 되돌아 간 적이 없다면 이동해도 가능한 위치이므로,

방문한 적이 없다면, 그곳을 방문하고, 길이를 늘려준 후,

그 위치로 이동해줍니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/49994">Programmers-방문 길이</a>

---
