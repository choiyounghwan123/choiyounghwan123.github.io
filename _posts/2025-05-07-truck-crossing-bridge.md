---
title: "[프로그래머스] 다리를 지나는 트럭"
excerpt: "큐를 활용한 다리를 지나는 트럭 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 큐, 시뮬레이션]

permalink: /algorithm/truck-crossing-bridge/

toc: true
toc_sticky: true

date: 2025-05-07
last_modified_at: 2025-05-07
---

# 문제 설명

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

# 제한사항

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

# 입출력 예

```
입력:
bridge_length = 100
weight = 100
truck_weights = [10, 10, 10, 10, 10, 10, 10, 10, 10, 10]

출력: 110
```

# 풀이 방법

이 문제는 큐를 활용하여 시뮬레이션하는 방식으로 해결할 수 있습니다.

1. 다리 위의 트럭을 관리하는 큐와 무게를 관리하는 큐를 생성합니다.
2. 매 초마다:
   - 다리 위의 모든 트럭을 한 칸씩 이동시킵니다.
   - 다리를 완전히 건넌 트럭을 제거합니다.
   - 새로운 트럭이 다리에 올라갈 수 있는지 확인하고 올립니다.
3. 모든 트럭이 다리를 건널 때까지 반복합니다.

# 코드

```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    queue = deque([])      # 다리 위의 트럭 위치
    weight1 = deque([])    # 다리 위의 트럭 무게
    result = 1             # 경과 시간
    
    while truck_weights or queue:  # 대기 중인 트럭이나 다리 위의 트럭이 있는 동안
        # 새로운 트럭이 다리에 올라갈 수 있는지 확인
        if truck_weights and sum(weight1) + truck_weights[0] <= weight:
            queue.append(bridge_length)  # 트럭 위치 추가
            weight1.append(truck_weights.pop(0))  # 트럭 무게 추가
        
        # 다리 위의 모든 트럭 이동
        for i in range(len(queue)):
            queue[i] -= 1
        
        # 다리를 건넌 트럭 제거
        if queue and queue[0] == 0:
            queue.popleft()
            weight1.popleft()
        
        result += 1  # 시간 증가
        
    return result

print(solution(100, 100, [10, 10, 10, 10, 10, 10, 10, 10, 10, 10]))  # 110
```

# 코드 설명

1. `queue`: 다리 위의 트럭 위치를 저장하는 큐입니다.
2. `weight1`: 다리 위의 트럭 무게를 저장하는 큐입니다.
3. 매 초마다:
   - 새로운 트럭이 다리에 올라갈 수 있는지 확인합니다.
   - 다리 위의 모든 트럭을 한 칸씩 이동시킵니다.
   - 다리를 완전히 건넌 트럭을 제거합니다.
   - 시간을 1초 증가시킵니다.
4. 모든 트럭이 다리를 건널 때까지 반복합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N * L)
  - N은 트럭의 수, L은 다리의 길이
  - 각 트럭이 다리를 건너는 동안 매 초마다 모든 트럭을 이동시켜야 합니다.
- 공간 복잡도: O(N)
  - 다리 위의 트럭 정보를 저장하는 데 O(N) 공간이 필요합니다.

이 방법을 사용하면 효율적으로 트럭이 다리를 건너는 시간을 계산할 수 있습니다. 