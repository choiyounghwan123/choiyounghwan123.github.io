---
title: "[2025 프로그래머스 코드챌린지 2차 예선] 서버 증설 횟수"
excerpt: "시뮬레이션을 활용한 서버 증설 횟수 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 시뮬레이션, Python]

permalink: /algorithm/server-expansion/

toc: true
toc_sticky: true

date: 2025-05-21
last_modified_at: 2025-05-21
---

# 문제 설명

서버 증설 횟수를 계산하는 문제입니다. 각 시간대별로 필요한 서버 용량이 주어지고, 서버를 증설할 때마다 일정 시간 동안 유지됩니다. 필요한 최소 서버 증설 횟수를 구해야 합니다.

- 각 시간대별로 필요한 서버 용량이 주어집니다.
- 서버를 증설할 때마다 k시간 동안 유지됩니다.
- 각 서버는 m만큼의 용량을 가집니다.
- 필요한 용량이 현재 서버 용량보다 크면 서버를 증설해야 합니다.

# 제한사항

- 1 ≤ len(players) ≤ 100,000
- 1 ≤ m ≤ 100
- 1 ≤ k ≤ 100
- 0 ≤ players[i] ≤ 100,000

# 입출력 예

```
입력: 
players = [0, 2, 3, 3, 1, 2, 0, 0, 0, 0, 4, 2, 0, 6, 0, 4, 2, 13, 3, 5, 10, 0, 1, 5]
m = 3
k = 5

출력: 8
```

# 풀이 방법

이 문제는 시뮬레이션을 통해 해결할 수 있습니다. 각 시간대별로 필요한 서버 용량을 확인하고, 현재 서버 용량이 부족할 경우 서버를 증설합니다. 서버 증설은 k시간 동안 유지되며, 이 시간이 지나면 서버 용량이 감소합니다.

# 코드

```python
def solution(players, m, k):
    current_server = 0  # 현재 서버 용량
    timer = []         # 각 증설된 서버의 남은 시간
    a = []            # 각 증설된 서버의 용량
    result = 0        # 총 증설 횟수
    
    for i in range(len(players)):
        # 타이머 감소 및 만료된 서버 제거
        if timer:
            idx = []
            for j in range(len(timer)):
                timer[j] -= 1
                if timer[j] == 0:
                    idx.append(j)
                    current_server -= a[j]
            for index in idx:
                timer.pop(index)
                a.pop(index)
        
        # 필요한 경우 서버 증설
        if players[i] >= (current_server + 1) * m:
            a.append(players[i] // m - current_server)
            result += a[-1]
            current_server += a[-1]
            timer.append(k)
            
    return result
```

# 코드 설명

1. 변수 초기화:
   - `current_server`: 현재 서버 용량
   - `timer`: 각 증설된 서버의 남은 시간
   - `a`: 각 증설된 서버의 용량
   - `result`: 총 증설 횟수

2. 각 시간대별 처리:
   - 타이머가 있는 경우, 각 타이머를 1씩 감소시킵니다.
   - 타이머가 0이 된 서버는 제거하고 현재 서버 용량에서 감소시킵니다.
   - 현재 서버 용량이 부족한 경우, 필요한 만큼 서버를 증설합니다.
   - 증설된 서버는 k시간 동안 유지됩니다.

3. 결과 반환:
   - 총 증설 횟수를 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n * k)
  - n: 시간대의 개수
  - k: 서버 증설 유지 시간
- 공간 복잡도: O(n)

각 시간대마다 타이머를 확인하고 업데이트하는 작업이 필요하므로, 시간 복잡도는 시간대의 개수와 서버 증설 유지 시간의 곱에 비례합니다. 