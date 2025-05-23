---
title: "[프로그래머스] 의상"
excerpt: "해시맵을 활용한 의상 조합 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 해시]

permalink: /algorithm/clothes/

toc: true
toc_sticky: true

date: 2025-05-23
last_modified_at: 2025-05-23
---

# 문제 설명

스파이가 가진 의상들이 주어졌을 때, 서로 다른 옷의 조합의 수를 구하는 문제입니다.

- 각 의상은 [의상의 이름, 의상의 종류] 형태로 주어집니다.
- 같은 종류의 의상은 동시에 착용할 수 없습니다.
- 최소 한 개의 의상은 입어야 합니다.

# 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- 1 ≤ clothes의 길이 ≤ 30
- 1 ≤ 의상의 이름의 길이 ≤ 20
- 1 ≤ 의상의 종류의 길이 ≤ 20

# 입출력 예

```
입력: 
clothes = [["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]

출력: 5
```

# 풀이 방법

1. 각 의상의 종류별로 개수를 해시맵에 저장합니다.
2. 각 종류별로 (의상의 개수 + 1)을 곱합니다.
   - +1은 해당 종류의 의상을 입지 않는 경우를 포함합니다.
3. 최종 결과에서 1을 뺍니다.
   - 1을 빼는 이유는 모든 종류의 의상을 입지 않는 경우를 제외하기 위함입니다.

# 코드

```python
from collections import defaultdict

def solution(clothes):
    hash_map = defaultdict(int)
    for clothe in clothes:
        hash_map[clothe[1]] += 1
    result = 1
    for key, item in hash_map.items():
        result *= (item + 1)
    return result - 1
```

# 코드 설명

1. `defaultdict`를 사용하여 의상의 종류별 개수를 저장합니다.
2. 각 의상에 대해:
   - 의상의 종류를 키로 사용하여 개수를 증가시킵니다.
3. 각 종류별로:
   - (의상의 개수 + 1)을 곱하여 모든 가능한 조합의 수를 계산합니다.
4. 최종 결과에서 1을 빼서 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n)
  - n: clothes 배열의 길이
- 공간 복잡도: O(k)
  - k: 의상의 종류의 수

각 의상에 대해 한 번씩 순회하면서 해시맵을 업데이트하므로, 시간 복잡도는 O(n)입니다. 