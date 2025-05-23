---
title: "[프로그래머스] 프로세스"
excerpt: "큐를 활용한 프로세스 실행 순서 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 큐]

permalink: /algorithm/process/

toc: true
toc_sticky: true

date: 2025-05-24
last_modified_at: 2025-05-24
---

# 문제 설명

운영체제의 프로세스 스케줄링을 시뮬레이션하는 문제입니다. 각 프로세스는 우선순위를 가지고 있으며, 우선순위가 높은 프로세스가 먼저 실행됩니다.

- 프로세스의 우선순위가 담긴 배열과 특정 프로세스의 위치가 주어집니다.
- 우선순위가 높은 프로세스가 먼저 실행됩니다.
- 우선순위가 같은 경우, 먼저 대기한 프로세스가 먼저 실행됩니다.
- 특정 프로세스가 몇 번째로 실행되는지 구해야 합니다.

# 제한사항

- priorities의 길이는 1 이상 100 이하입니다.
- priorities의 원소는 1 이상 9 이하의 정수입니다.
- location은 0 이상 (priorities의 길이 - 1) 이하의 정수입니다.

# 입출력 예

```
입력: 
priorities = [2, 1, 3, 2]
location = 2

출력: 1
```

# 풀이 방법

이 문제는 큐를 활용하여 해결할 수 있습니다. 프로세스의 인덱스와 우선순위를 큐에 저장하고, 우선순위가 가장 높은 프로세스가 나올 때까지 큐를 회전시킵니다. 우선순위가 가장 높은 프로세스가 나오면 실행하고, 특정 프로세스가 실행될 때까지 반복합니다.

# 코드

```python
from collections import deque

def solution(priorities, location):
    queue = deque([i for i in range(len(priorities))])
    priorities = deque(priorities)
    answer = 0
    while queue:
        if priorities[0] < max(priorities):
            priorities.append(priorities.popleft())
            queue.append(queue.popleft())
            continue
        priorities.popleft()
        answer += 1
        if queue.popleft() == location:
            return answer

print(solution([2, 1, 3, 2], 2))
```

# 코드 설명

1. 프로세스의 인덱스와 우선순위를 큐에 저장합니다.
2. 큐가 비어있지 않은 동안:
   - 현재 프로세스의 우선순위가 최대 우선순위보다 작으면, 큐를 회전시킵니다.
   - 현재 프로세스의 우선순위가 최대 우선순위와 같으면, 프로세스를 실행하고 실행 순서를 증가시킵니다.
   - 실행된 프로세스가 특정 프로세스라면, 실행 순서를 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n * m)
  - n: priorities의 길이
  - m: 최대 우선순위를 찾는 시간
- 공간 복잡도: O(n)
  - n: 큐에 저장되는 프로세스의 수

각 프로세스에 대해 최대 우선순위를 찾는 작업을 수행하므로, 시간 복잡도는 O(n * m)입니다. 큐에 프로세스를 저장하므로 공간 복잡도는 O(n)입니다. 