---
title: "[Baekjoon] 9663. N-Queen : Python"
excerpt: "백트래킹을 활용한 N-Queen 문제 풀이"

categories:
  - Algorithm
tags:
  - 백준
  - 백트래킹
  - DFS
  - Python

permalink: /algorithm/baekjoon-9663/

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
---

## 🦥 문제 설명
N-Queen 문제는 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제입니다. 퀸은 가로, 세로, 대각선으로 이동할 수 있으므로, 서로 다른 퀸이 같은 행, 열, 대각선에 위치하면 안 됩니다.

## 💡 접근 방법
1. 백트래킹을 사용하여 가능한 모든 경우의 수를 탐색합니다.
2. 각 행에 하나의 퀸만 놓을 수 있으므로, 1차원 배열로 체스판을 표현할 수 있습니다.
3. 현재 위치에 퀸을 놓을 수 있는지 확인하는 `is_promising` 함수를 구현합니다.
4. DFS를 사용하여 모든 가능한 경우를 탐색합니다.

## 🛠️ 구현
```python
N = int(input())
board = [0] * N  # board[i]는 i번째 행의 퀸이 위치한 열을 의미
ans = 0

def is_promising(n:int)->bool:
    # 현재 위치에 퀸을 놓을 수 있는지 확인
    for i in range(n):
        # 같은 열에 퀸이 있거나 대각선에 퀸이 있는 경우
        if board[i] == board[n] or abs(board[n] - board[i]) == abs(n-i):
            return False
    return True

def dfs(start):
    global ans
    if start == N:  # 모든 행에 퀸을 놓은 경우
        ans += 1
        return
    for i in range(N):
        board[start] = i  # start번째 행의 i번째 열에 퀸을 놓음
        if is_promising(start):  # 현재 위치가 유효한지 확인
            dfs(start+1)  # 다음 행으로 이동

dfs(0)
print(ans)
```

## ⏰ 시간 복잡도
- 각 행마다 N개의 열을 확인하고, 각 위치마다 이전 퀸들의 위치를 확인하므로
- 시간 복잡도: O(N!)

## 📝 정리
N-Queen 문제는 백트래킹의 대표적인 문제입니다. 1차원 배열을 사용하여 체스판을 표현하고, DFS를 통해 가능한 모든 경우를 탐색하는 방식으로 해결할 수 있습니다. 각 위치에 퀸을 놓을 수 있는지 확인하는 `is_promising` 함수가 핵심입니다.
```
