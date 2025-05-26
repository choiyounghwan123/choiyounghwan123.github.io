---
title: "[프로그래머스] 귤 고르기"
excerpt: "해시 맵을 활용한 귤 고르기 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 해시 맵, 정렬]

permalink: /algorithm/tangerine-picking/

toc: true
toc_sticky: true

date: 2025-05-26
last_modified_at: 2025-05-26
---

# 문제 설명

경화는 과수원에서 귤을 수확했습니다. 경화는 수확한 귤 중 'k'개를 골라 상자 하나에 담아 판매하려고 합니다. 그런데 귤의 크기가 일정하지 않아 보기에 좋지 않다고 생각한 경화는 귤을 크기별로 분류했을 때 서로 다른 종류의 수를 최소화하고 싶습니다.

# 제한사항

- 1 ≤ k ≤ tangerine의 길이 ≤ 100,000
- 1 ≤ tangerine의 원소 ≤ 10,000,000

# 입출력 예

```
입력:
k = 6
tangerine = [1, 3, 2, 5, 4, 5, 2, 3]

출력: 3
```

# 풀이 방법

이 문제는 해시 맵을 활용하여 해결할 수 있습니다.

1. 각 귤의 크기별 개수를 해시 맵에 저장합니다.
2. 개수를 내림차순으로 정렬합니다.
3. 가장 많은 개수를 가진 귤부터 선택하여 k개를 채웁니다.
4. 필요한 귤의 종류 수를 반환합니다.

# 코드

```python
from collections import defaultdict

def solution(k, tangerine):
    hash_map = defaultdict(int)
    arr = []
    
    # 각 귤의 크기별 개수 계산
    for t in tangerine:
        hash_map[t] += 1
    
    # 개수를 배열로 변환
    for key, item in hash_map.items():
        arr.append(item)
    
    # 개수를 내림차순으로 정렬
    arr.sort(reverse=True)
    
    result = 0
    temp = 0
    
    # k개를 채울 때까지 반복
    for element in arr:
        if temp + element <= k:
            temp += element
            result += 1
            if temp == k:
                break
        else:
            result += 1
            temp += element
            break
            
    return result

print(solution(6, [1, 3, 2, 5, 4, 5, 2, 3]))  # 3
```

# 코드 설명

1. `defaultdict`를 사용하여 각 귤의 크기별 개수를 저장합니다.
2. 해시 맵의 값들을 배열로 변환하고 내림차순으로 정렬합니다.
3. 가장 많은 개수를 가진 귤부터 선택하여 k개를 채웁니다.
4. 필요한 귤의 종류 수를 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N log N)
  - 귤의 개수를 세는 데 O(N)
  - 정렬에 O(N log N)
- 공간 복잡도: O(N)
  - 해시 맵과 배열을 저장하는 데 O(N) 공간이 필요합니다.

해시 맵을 활용하면 효율적으로 귤 고르기 문제를 해결할 수 있습니다. 