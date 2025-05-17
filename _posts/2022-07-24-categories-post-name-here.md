---
title: "[PCCP] 2번 문제 - 석유시추 풀이"
excerpt: "PCCP 2번 문제인 석유시추 문제의 해결 방법과 코드 설명"

categories:
  - Algorithm
tags:
  - [PCCP, DFS, 그래프]

permalink: /algorithm/pccp-oil-drilling/

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
---

## 문제 설명

이 문제는 2차원 배열로 주어진 석유 시추 지도에서 각 열마다 시추할 수 있는 석유의 최대량을 구하는 문제입니다. 석유는 상하좌우로 연결된 1로 표시된 영역에 존재하며, 각 열에서 시추할 때 해당 열의 모든 석유를 채취할 수 있습니다.

## 접근 방법

1. DFS를 사용하여 각 석유 덩어리(연결된 1들의 집합)를 찾고 번호를 매깁니다.
2. 각 석유 덩어리의 크기를 저장합니다.
3. 각 열마다 시추할 수 있는 석유의 양을 계산하여 최대값을 구합니다.

## 코드 설명

```python
import sys
sys.setrecursionlimit(1000000000)

def solution(land):
    count = 0
    def dfs(i, j):
        nonlocal count
        count += 1
        dx = [0, 0, 1, -1]
        dy = [1, -1, 0, 0]

        for k in range(4):
            x = i+dx[k]
            y = j + dy[k]
            if 0 <= x < len(land[0]) and 0 <= y < len(land) and land[y][x] == 1 and visited[y][x] == 0:
                visited[y][x] = visited[j][i]
                dfs(x,y)
    
    result = 0
    visited = [[0] * len(land[0]) for _ in range(len(land))]
    oils = [0]  # 각 석유 덩어리의 크기를 저장할 배열
    
    # 석유 덩어리 찾기
    for i in range(len(land[0])):
        for j in range(len(land)):
            if land[j][i] == 1 and visited[j][i] == 0:
                visited[j][i] = len(oils)
                dfs(i,j)
                oils.append(count)
                count = 0
    
    # 각 열마다 시추할 수 있는 석유의 양 계산
    for i in range(len(land[0])):
        a = set()
        for j in range(len(land)):
            a.add(visited[j][i])
        a = list(a)
        temp = 0
        for j in a[1:]:
            temp += oils[j]
        result = max(result,temp)
    
    return result
```

## 시간 복잡도

- DFS 탐색: O(N*M) (N: 행의 개수, M: 열의 개수)
- 각 열의 석유량 계산: O(N*M)
- 전체 시간 복잡도: O(N*M)

## 공간 복잡도

- visited 배열: O(N*M)
- oils 배열: O(N*M)
- 전체 공간 복잡도: O(N*M)

## 결론

이 문제는 DFS를 사용하여 연결된 석유 덩어리를 찾고, 각 열마다 시추할 수 있는 석유의 양을 계산하는 방식으로 해결할 수 있습니다. 방문 배열을 사용하여 이미 탐색한 석유 덩어리를 다시 탐색하지 않도록 하여 효율적으로 문제를 해결할 수 있습니다.
