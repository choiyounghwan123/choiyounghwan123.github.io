---
title: "[프로그래머스] 미로 탈출"
excerpt: "BFS를 활용한 미로 탈출 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, BFS, Python]

permalink: /algorithm/maze-escape/

toc: true
toc_sticky: true

date: 2025-05-19
last_modified_at: 2025-05-19
---

# 문제 설명

미로 탈출 문제는 다음과 같은 규칙을 가진 미로에서 시작점(S)에서 출구(E)까지 도달하는 최소 시간을 구하는 문제입니다.

- 레버(L)를 당겨야만 출구로 나갈 수 있습니다.
- X는 벽으로, 이동할 수 없습니다.
- O는 이동 가능한 공간입니다.
- 한 칸 이동하는데 1초가 걸립니다.

# 풀이 방법

이 문제는 BFS(너비 우선 탐색)를 사용하여 해결할 수 있습니다. 다음과 같은 단계로 풀이합니다:

1. 시작점(S)에서 레버(L)까지의 최단 거리를 구합니다.
2. 레버(L)에서 출구(E)까지의 최단 거리를 구합니다.
3. 두 거리의 합이 최종 답이 됩니다.

# 코드

```python
from collections import deque

def solution(maps):
    start = None
    for i in range(len(maps)):
        for j in range(len(maps[i])):
            if maps[i][j] == "S":
                start = (i,j)
                break

    queue = deque([start])
    visited = [[-1] * len(maps[i]) for i in range(len(maps))]
    dx = [1,-1,0,0]
    dy = [0,0,1,-1]
    visited[start[0]][start[1]] = 0
    check = 0
    lever = None
    queue_lever = None
    visited_lever = [[-1] * len(maps[i]) for i in range(len(maps))]
    while queue:
        x,y = queue.popleft()
        if maps[x][y] == "E" and check:
            return visited[x][y]
        if maps[x][y] == "L":
            check = 1
            queue_lever = deque([(x,y)])
            visited_lever[x][y] = visited[x][y]
            break
        for i in range(4):
            x_ = x + dx[i]
            y_ = y + dy[i]

            if 0 <= x_ < len(maps) and 0 <= y_ < len(maps[0]) and maps[x_][y_] != 'X' and visited[x_][y_] == -1:
                visited[x_][y_] = visited[x][y] + 1
                queue.append([x_,y_])
    else:
        return -1

    while queue_lever:
        x,y = queue_lever.popleft()
        if maps[x][y] == "E":
            return visited_lever[x][y]
        for i in range(4):
            x_ = x + dx[i]
            y_ = y + dy[i]

            if 0 <= x_ < len(maps) and 0 <= y_ < len(maps[0]) and maps[x_][y_] != 'X' and visited_lever[x_][y_] == -1:
                visited_lever[x_][y_] = visited_lever[x][y] + 1
                queue_lever.append([x_,y_])
    else:
        return -1
```

# 코드 설명

1. 먼저 시작점(S)의 위치를 찾습니다.
2. BFS를 사용하여 시작점에서 레버까지의 최단 거리를 구합니다.
3. 레버를 찾으면, 새로운 BFS를 시작하여 출구까지의 최단 거리를 구합니다.
4. 각 단계에서 방문하지 않은 위치를 방문할 때마다 거리를 1씩 증가시킵니다.
5. 만약 레버나 출구에 도달할 수 없다면 -1을 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(N * M)
  - N: 미로의 세로 길이
  - M: 미로의 가로 길이
- 공간 복잡도: O(N * M)

각 위치를 최대 한 번씩만 방문하므로, 시간 복잡도는 미로의 크기에 비례합니다. 