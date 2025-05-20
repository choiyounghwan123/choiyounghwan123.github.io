---
title: "[2025 프로그래머스 코드챌린지 1차 예선] 지게차와 크레인"
excerpt: "시뮬레이션을 활용한 지게차와 크레인 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 시뮬레이션, Python]

permalink: /algorithm/forklift-crane/

toc: true
toc_sticky: true

date: 2025-05-20
last_modified_at: 2025-05-20
---

# 문제 설명

창고에서 지게차와 크레인을 사용하여 물건을 이동하는 문제입니다. 지게차는 한 번에 하나의 물건만 이동할 수 있고, 크레인은 여러 개의 동일한 물건을 한 번에 이동할 수 있습니다.

- 창고는 2차원 배열로 표현되며, 각 칸에는 물건의 종류가 표시됩니다.
- 지게차는 주변에 벽(1)이 있는 물건만 이동할 수 있습니다.
- 크레인은 동일한 종류의 물건들을 한 번에 이동할 수 있습니다.
- 물건을 이동한 후에는 해당 위치가 벽으로 변합니다.

# 제한사항

- 1 ≤ storage의 길이 ≤ 100
- 1 ≤ storage의 각 행의 길이 ≤ 100
- 1 ≤ requests의 길이 ≤ 100
- requests의 각 원소는 1개 이상의 문자로 구성됩니다.

# 입출력 예

```
입력: 
storage = ["AZWQY", "CAABX", "BBDDA", "ACACA"]
requests = ["A", "BB", "A"]

출력: 3
```

# 풀이 방법

이 문제는 시뮬레이션을 통해 해결할 수 있습니다. 지게차와 크레인의 동작을 각각 구현하고, 요청에 따라 적절한 동작을 수행하도록 합니다.

# 코드

```python
def fork_list(request, storage):
    dx = [1, -1, 0, 0]
    dy = [0, 0, 1, -1]
    index = list()
    # 주변에 벽이 있는 물건 찾기
    for i in range(1, len(storage) - 1):
        for j in range(1, len(storage[0]) - 1):
            if storage[i][j] == request:
                for k in range(4):
                    x = i + dx[k]
                    y = j + dy[k]
                    if 0 <= x < len(storage) and 0 <= y < len(storage[x]) and storage[x][y] == "1":
                        index.append([i, j])
                        break

    # 찾은 물건 이동
    for i in range(len(index)):
        storage[index[i][0]][index[i][1]] = "0"
        isOutSide(index[i][0], index[i][1], storage)

def crane(request, storage):
    # 크레인으로 물건 이동
    for i in range(len(storage)):
        for j in range(len(storage[i])):
            if storage[i][j] == list(request)[0]:
                storage[i][j] = "0"
                isOutSide(i, j, storage)

def isOutSide(i, j, storage):
    dx = [1, -1, 0, 0]
    dy = [0, 0, 1, -1]
    outSide = False

    # 주변에 벽이 있는지 확인
    for k in range(4):
        x = i + dx[k]
        y = j + dy[k]
        if 0 <= x < len(storage) and 0 <= y < len(storage[x]) and storage[x][y] == "1":
            outSide = True
            storage[i][j] = "1"
            break

    # 주변 물건들도 확인
    if outSide:
        for k in range(4):
            x = i + dx[k]
            y = j + dy[k]
            if 0 <= x < len(storage) and 0 <= y < len(storage[x]) and storage[x][y] == "0":
                isOutSide(x, y, storage)

def solution(storage, requests):
    # 창고 테두리에 벽 추가
    storage = [list("1" + s + "1") for s in storage]
    storage.insert(0, list("1" * len(storage[0])))
    storage.append(list("1" * len(storage[0])))

    # 요청 처리
    for request in requests:
        if len(request) == 1:
            fork_list(request, storage)
        else:
            crane(request, storage)

    # 남은 물건 개수 계산
    result = 0
    for i in range(1, len(storage) - 1):
        for j in range(1, len(storage[i]) - 1):
            if storage[i][j] not in ("0", "1"):
                result += 1
    return result
```

# 코드 설명

1. `fork_list` 함수:
   - 지게차로 이동할 수 있는 물건을 찾습니다.
   - 주변에 벽이 있는 물건만 이동 가능합니다.
   - 이동한 물건의 위치를 벽으로 변경합니다.

2. `crane` 함수:
   - 크레인으로 물건을 이동합니다.
   - 동일한 종류의 물건을 한 번에 이동할 수 있습니다.
   - 이동한 물건의 위치를 벽으로 변경합니다.

3. `isOutSide` 함수:
   - 물건을 이동한 후 주변 상황을 확인합니다.
   - 주변에 벽이 있는 경우 해당 위치를 벽으로 변경합니다.
   - 주변 물건들도 확인하여 필요한 경우 처리합니다.

4. `solution` 함수:
   - 창고 테두리에 벽을 추가합니다.
   - 요청에 따라 지게차 또는 크레인을 사용합니다.
   - 남은 물건의 개수를 계산합니다.

# 시간 복잡도

- 시간 복잡도: O(n * m * k)
  - n: 창고의 세로 길이
  - m: 창고의 가로 길이
  - k: 요청의 개수
- 공간 복잡도: O(n * m)

각 요청에 대해 창고의 모든 위치를 확인하고, 물건을 이동한 후 주변 상황도 확인하므로, 시간 복잡도는 창고의 크기와 요청의 개수에 비례합니다. 