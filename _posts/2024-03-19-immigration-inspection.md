---
title: "[프로그래머스] 입국심사"
excerpt: "이분 탐색을 활용한 입국심사 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 이분탐색, Binary Search]

permalink: /algorithm/immigration-inspection/

toc: true
toc_sticky: true

date: 2024-03-19
last_modified_at: 2024-03-19
---

## 문제 설명

입국심사를 기다리는 사람 수 n과 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 주어졌을 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 구하는 문제입니다.

## 접근 방법

1. 이분 탐색을 사용하여 최소 시간을 찾습니다.
2. 특정 시간(mid)이 주어졌을 때, 각 심사관이 처리할 수 있는 사람의 수를 계산합니다.
3. 처리할 수 있는 총 사람의 수가 n보다 크거나 같으면 시간을 줄이고, 작으면 시간을 늘립니다.

## 코드 설명

```python
def solution(n, times):
    left, right = 1, 1000000000  # 최소 시간과 최대 시간 설정
    
    while left <= right:
        mid = (left + right) // 2  # 중간 시간
        temp = 0  # 현재 시간으로 처리할 수 있는 총 사람 수
        
        # 각 심사관이 처리할 수 있는 사람 수 계산
        for time in times:
            temp += mid // time
            
        # 이분 탐색 조건
        if temp >= n:  # 처리할 수 있는 사람이 n보다 많거나 같으면
            right = mid - 1  # 시간을 줄임
        else:  # 처리할 수 있는 사람이 n보다 적으면
            left = mid + 1  # 시간을 늘림
            
    return left  # 최소 시간 반환
```

## 시간 복잡도

- 이분 탐색: O(log(max_time))
- 각 단계에서의 처리: O(m) (m: 심사관 수)
- 전체 시간 복잡도: O(m * log(max_time))

## 공간 복잡도

- 추가 공간 사용 없음
- 공간 복잡도: O(1)

## 결론

이 문제는 이분 탐색을 활용하여 효율적으로 해결할 수 있습니다. 주어진 시간 내에 처리할 수 있는 사람의 수를 계산하고, 이를 기준으로 이분 탐색을 수행하여 최소 시간을 찾아냅니다. 이분 탐색을 사용함으로써 O(m * log(max_time))의 시간 복잡도로 문제를 해결할 수 있습니다. 