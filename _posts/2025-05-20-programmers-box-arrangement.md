---
title: "[2025 프로그래머스 코드챌린지 2차 예선] 택배 상자 꺼내기"
excerpt: "시뮬레이션을 활용한 택배 상자 꺼내기 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 시뮬레이션, Python]

permalink: /algorithm/box-arrangement/

toc: true
toc_sticky: true

date: 2025-05-21
last_modified_at: 2025-05-21
---

# 문제 설명

택배 상자를 특정 규칙에 따라 배치하고 꺼내는 문제입니다. 상자는 지그재그 패턴으로 배치되며, 특정 번호의 상자를 찾기 위해 제거해야 하는 상자의 개수를 구해야 합니다.

- 상자는 w개의 열에 지그재그 패턴으로 배치됩니다.
- 짝수 행은 왼쪽에서 오른쪽으로, 홀수 행은 오른쪽에서 왼쪽으로 배치됩니다.
- 특정 번호의 상자를 찾기 위해서는 해당 상자 위에 있는 모든 상자를 제거해야 합니다.

# 제한사항

- 1 ≤ n ≤ 100,000
- 1 ≤ w ≤ 100
- 1 ≤ num ≤ n

# 입출력 예

```
입력: 
n = 22
w = 6
num = 8

출력: 3
```

# 풀이 방법

이 문제는 시뮬레이션을 통해 해결할 수 있습니다. 상자를 지그재그 패턴으로 배치한 후, 특정 번호의 상자를 찾기 위해 제거해야 하는 상자의 개수를 계산합니다.

# 코드

```python
def solution(n, w, num):
    # w개의 열을 가진 상자 배열 생성
    boxs = [[] for _ in range(w)]
    i, j, k = 0, 0, 1

    # 상자 배치
    while k <= n:
        if k % w == 0:
            j += 1
            boxs[i].append(k)
            k += 1
            continue
        boxs[i].append(k)
        k += 1
        if j % 2 == 0:
            i += 1
        else:
            i -= 1

    # 특정 번호의 상자 찾기
    result = 1
    for box in boxs:
        if num in box:
            while box[-1] != num:
                box.pop(-1)
                result += 1
            break
    return result
```

# 코드 설명

1. 상자 배치:
   - w개의 열을 가진 빈 배열을 생성합니다.
   - 1부터 n까지의 번호를 지그재그 패턴으로 배치합니다.
   - 짝수 행은 왼쪽에서 오른쪽으로, 홀수 행은 오른쪽에서 왼쪽으로 배치합니다.

2. 특정 번호의 상자 찾기:
   - 각 열에서 특정 번호의 상자를 찾습니다.
   - 찾은 열에서 해당 상자 위에 있는 모든 상자를 제거합니다.
   - 제거한 상자의 개수에 1을 더한 값을 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n)
  - n: 상자의 개수
- 공간 복잡도: O(w)

상자를 한 번씩만 배치하고, 특정 번호의 상자를 찾기 위해 각 열을 한 번씩만 확인하므로, 시간 복잡도는 상자의 개수에 비례합니다. 