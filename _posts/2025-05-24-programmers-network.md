---
title: "[프로그래머스] 네트워크"
excerpt: "DFS를 활용한 네트워크(연결 요소 개수) 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, DFS, 그래프]

permalink: /algorithm/network/

toc: true
toc_sticky: true

date: 2025-05-24
last_modified_at: 2025-05-24
---

# 문제 설명

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 말합니다. 어떤 컴퓨터가 다른 컴퓨터와 직접적으로 혹은 간접적으로 연결되어 있다면, 같은 네트워크 상에 있다고 봅니다.

- 컴퓨터의 개수 n과 연결 정보가 담긴 2차원 배열 computers가 주어집니다.
- 연결 정보는 computers[i][j]가 1이면 i번 컴퓨터와 j번 컴퓨터가 연결되어 있음을 의미합니다.
- 네트워크의 개수(연결된 컴퓨터 집합의 개수)를 구해야 합니다.

# 제한사항

- n은 1 이상 200 이하인 자연수입니다.
- computers의 각 원소는 0 또는 1입니다.
- computers[i][i]는 항상 1입니다.

# 입출력 예

```
입력:
n = 3
computers = [[1, 1, 0], [1, 1, 0], [0, 0, 1]]

출력: 2
```

# 풀이 방법

이 문제는 그래프의 연결 요소(connected components) 개수를 구하는 전형적인 문제입니다. DFS(깊이 우선 탐색)를 활용하여 각 컴퓨터를 방문하며, 방문하지 않은 컴퓨터에서 DFS를 시작할 때마다 네트워크 개수를 1씩 증가시킵니다.

# 코드

```python
def dfs(i, computer, visited):
    for j in range(len(computer)):
        if computer[i][j] == 1 and not visited[j]:
            visited[j] = 1
            dfs(j, computer, visited)

def solution(n, computers):
    visited = [0] * n
    result = 0

    for i in range(n):
        if not visited[i]:
            visited[i] = 1
            dfs(i, computers, visited)
            result += 1

    return result

print(solution(3, [[1, 1, 0], [1, 1, 0], [0, 0, 1]]))
```

# 코드 설명

1. `dfs` 함수는 현재 컴퓨터에서 연결된 모든 컴퓨터를 재귀적으로 방문합니다.
2. `solution` 함수에서 각 컴퓨터를 순회하며, 방문하지 않은 컴퓨터가 있으면 DFS를 시작하고 네트워크 개수를 1 증가시킵니다.
3. 모든 컴퓨터를 방문할 때까지 반복합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(n^2)
  - 각 컴퓨터마다 모든 컴퓨터와의 연결을 확인하므로 n^2
- 공간 복잡도: O(n)
  - 방문 배열(visited) 및 재귀 호출 스택

DFS를 통해 모든 연결 요소를 탐색하므로, 효율적으로 네트워크의 개수를 구할 수 있습니다. 