---
title: "[프로그래머스] 무인도 여행"
excerpt: "DFS를 활용한 무인도 식량 수집 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, DFS, 깊이우선탐색]

permalink: /algorithm/desert-island/

toc: true
toc_sticky: true

date: 2024-03-22
last_modified_at: 2024-03-22
---

## 문제 설명

무인도 지도가 주어졌을 때, 각 무인도에서 수집할 수 있는 식량의 합을 구하는 문제입니다. 지도는 'X'로 표시된 바다와 숫자로 표시된 식량이 있는 무인도로 구성되어 있습니다. 상하좌우로 연결된 무인도들은 하나의 무인도로 간주됩니다.

## 접근 방법

1. DFS(깊이 우선 탐색)를 사용하여 연결된 무인도들을 탐색합니다.
2. 방문하지 않은 무인도를 발견하면 해당 무인드의 식량을 수집하고 연결된 모든 무인도를 탐색합니다.
3. 각 무인도 그룹에서 수집한 식량의 합을 배열에 저장하고, 오름차순으로 정렬하여 반환합니다.
4. 수집할 수 있는 식량이 없는 경우 [-1]을 반환합니다.

## 코드 설명

```python
import sys
sys.setrecursionlimit(100000)  # 재귀 제한 해제

def solution(maps):
    def dfs(i, j):
        nonlocal count
        # 상하좌우 이동을 위한 방향 배열
        dx = [1, -1, 0, 0]
        dy = [0, 0, 1, -1]
        
        # 4방향 탐색
        for k in range(4):
            x = i + dx[k]
            y = j + dy[k]
            # 유효한 범위이고, 방문하지 않았으며, 바다가 아닌 경우
            if 0 <= x < len(maps) and 0 <= y < len(maps[0]) and visited[x][y] == 0 and maps[x][y] != 'X':
                count += int(maps[x][y])  # 식량 수집
                visited[x][y] = 1  # 방문 표시
                dfs(x, y)  # 재귀적으로 탐색

    # 방문 배열 초기화
    visited = [[0] * len(maps[0]) for _ in range(len(maps))]
    answer = []
    
    # 모든 위치 탐색
    for i in range(len(maps)):
        for j in range(len(maps[i])):
            # 방문하지 않은 무인도를 발견한 경우
            if maps[i][j] != "X" and visited[i][j] == 0:
                count = 0
                count += int(maps[i][j])  # 시작 무인도의 식량 수집
                visited[i][j] = 1  # 방문 표시
                dfs(i, j)  # DFS 탐색 시작
                answer.append(count)  # 수집한 식량 합 저장
                
    return sorted(answer) if answer else [-1]  # 결과 정렬하여 반환
```

## 예시 설명

입력: maps = ["X591X", "X1X5X", "X231X", "1XXX1"]

처리 과정:
1. 첫 번째 무인도 그룹 (왼쪽 상단):
   - 5 + 9 + 1 = 15

2. 두 번째 무인도 그룹 (오른쪽 상단):
   - 5

3. 세 번째 무인도 그룹 (중앙):
   - 2 + 3 + 1 = 6

4. 네 번째 무인도 그룹 (왼쪽 하단):
   - 1

5. 다섯 번째 무인도 그룹 (오른쪽 하단):
   - 1

결과: [1, 1, 5, 6, 15]

## 시간 복잡도

- 지도의 모든 칸을 한 번씩 방문: O(n * m)
- 각 칸에서 DFS 수행: O(n * m)
- 전체 시간 복잡도: O(n * m), n과 m은 지도의 세로와 가로 길이

## 공간 복잡도

- 방문 배열: O(n * m)
- 재귀 호출 스택: O(n * m)
- 전체 공간 복잡도: O(n * m)

## 결론

이 문제는 DFS를 활용하여 연결된 무인도들을 효율적으로 탐색하고 식량을 수집할 수 있습니다. 방문 배열을 사용하여 중복 방문을 방지하고, 재귀 호출을 통해 연결된 모든 무인도를 탐색합니다. 시간 복잡도는 O(n * m)으로, 주어진 지도의 크기에 대해 효율적으로 동작합니다. 