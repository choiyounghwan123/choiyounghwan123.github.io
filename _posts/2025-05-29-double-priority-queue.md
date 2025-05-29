---
title: "[프로그래머스] 이중우선순위큐"
excerpt: "힙(Heap)을 활용한 이중우선순위큐 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 힙, 우선순위큐]

permalink: /algorithm/double-priority-queue/

toc: true
toc_sticky: true

date: 2025-05-29
last_modified_at: 2025-05-29
---

# 문제 설명

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

- I 숫자: 큐에 주어진 숫자를 삽입합니다.
- D 1: 큐에서 최댓값을 삭제합니다.
- D -1: 큐에서 최솟값을 삭제합니다.

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

# 제한사항

- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 "I 숫자", "D 1", "D -1" 중 하나입니다.
- 숫자는 1 이상 100,000 이하인 정수입니다.
- "I 숫자"는 큐에 주어진 숫자를 삽입합니다.
- "D 1"은 큐에서 최댓값을 삭제합니다.
- "D -1"은 큐에서 최솟값을 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

# 입출력 예

```
입력: ["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"]
출력: [0, 0]
```

# 풀이 방법

이 문제는 최대 힙(Max Heap)과 최소 힙(Min Heap)을 함께 사용하여 해결할 수 있습니다.

1. 최대 힙과 최소 힙을 각각 구현합니다.
2. 삽입 연산(I)의 경우 두 힙 모두에 값을 추가합니다.
3. 최댓값 삭제(D 1)의 경우 최대 힙에서 값을 제거하고 최소 힙을 재구성합니다.
4. 최솟값 삭제(D -1)의 경우 최소 힙에서 값을 제거하고 최대 힙을 재구성합니다.
5. 모든 연산이 끝난 후 최댓값과 최솟값을 반환합니다.

# 코드

```python
import heapq

def solution(operations):
    answer = []
    max_heap, min_heap = [], []  # 최대 힙과 최소 힙

    for operation in operations:
        operand, num = operation.split()
        
        if operand == "I":  # 삽입 연산
            heapq.heappush(max_heap, -int(num))  # 최대 힙은 음수로 저장
            heapq.heappush(min_heap, int(num))
            
        elif max_heap and operand == "D" and num == "1":  # 최댓값 삭제
            heapq.heappop(max_heap)
            # 최소 힙 재구성
            min_heap = []
            for num in max_heap:
                heapq.heappush(min_heap, -num)
                
        elif min_heap and operand == "D" and num == "-1":  # 최솟값 삭제
            heapq.heappop(min_heap)
            # 최대 힙 재구성
            max_heap = []
            for num in min_heap:
                heapq.heappush(max_heap, -num)

    # 결과 반환
    return [max(min_heap) if min_heap else 0, min(min_heap) if min_heap else 0]

# 테스트
print(solution(["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"]))
```

# 코드 설명

1. `max_heap`: 최대 힙으로, 음수 값을 저장하여 최댓값을 쉽게 추출할 수 있게 합니다.
2. `min_heap`: 최소 힙으로, 양수 값을 저장하여 최솟값을 쉽게 추출할 수 있게 합니다.

주요 로직:
- 삽입 연산(I): 두 힙 모두에 값을 추가합니다. 최대 힙에는 음수로 저장합니다.
- 최댓값 삭제(D 1): 최대 힙에서 값을 제거하고, 최소 힙을 재구성합니다.
- 최솟값 삭제(D -1): 최소 힙에서 값을 제거하고, 최대 힙을 재구성합니다.
- 결과 반환: 최소 힙이 비어있으면 [0, 0]을, 그렇지 않으면 [최댓값, 최솟값]을 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N log N)
  - N은 연산의 개수
  - 힙 연산(삽입, 삭제)은 O(log N)의 시간이 소요됩니다.
  - 힙 재구성 시 O(N log N)의 시간이 소요됩니다.
- 공간 복잡도: O(N)
  - 두 힙에 저장되는 요소의 개수만큼의 공간이 필요합니다.

이 방법을 사용하면 이중 우선순위 큐의 모든 연산을 효율적으로 처리할 수 있습니다. 최대 힙과 최소 힙을 함께 사용함으로써 최댓값과 최솟값을 모두 O(log N)의 시간 복잡도로 처리할 수 있습니다. 