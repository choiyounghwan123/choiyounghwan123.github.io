---
title: "[프로그래머스] 더 맵게"
excerpt: "힙(Heap)을 활용한 스코빌 지수 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 힙]

permalink: /algorithm/more-spicy/

toc: true
toc_sticky: true

date: 2025-05-27
last_modified_at: 2025-05-27
---

# 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 × 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

# 제한사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

# 입출력 예

```
입력: [1, 2, 3, 9, 10, 12], K = 7
출력: 2
```

# 풀이 방법

이 문제는 최소 힙(Min Heap)을 사용하여 효율적으로 해결할 수 있습니다.

1. 주어진 배열을 최소 힙으로 변환합니다.
2. 힙의 최소값이 K보다 작은 동안:
   - 가장 작은 두 값을 꺼내어 새로운 스코빌 지수를 계산합니다.
   - 계산된 값을 다시 힙에 넣습니다.
   - 섞은 횟수를 증가시킵니다.
3. 모든 음식의 스코빌 지수가 K 이상이 되면 섞은 횟수를 반환합니다.
4. 불가능한 경우 -1을 반환합니다.

# 코드

```python
import heapq

def solution(scoville, K):
    answer = 0
    
    heapq.heapify(scoville)
    try:
        while scoville[0] < K:
            a = heapq.heappop(scoville)
            b = heapq.heappop(scoville)
            heapq.heappush(scoville, a + (b*2))
            answer += 1
        return answer
    except:
        return -1

print(solution([1, 2, 3, 9, 10, 12], 7))  # 2
```

# 코드 설명

1. `heapq.heapify(scoville)`: 주어진 배열을 최소 힙으로 변환합니다.
2. `while scoville[0] < K`: 힙의 최소값이 K보다 작은 동안 반복합니다.
3. `heapq.heappop(scoville)`: 힙에서 가장 작은 값을 꺼냅니다.
4. `heapq.heappush(scoville, a + (b*2))`: 계산된 새로운 스코빌 지수를 힙에 넣습니다.
5. `try-except`: 힙에서 값을 꺼낼 수 없는 경우(불가능한 경우) -1을 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N log N)
  - N은 음식의 개수
  - 힙 연산(삽입, 삭제)은 O(log N)의 시간이 소요됩니다.
  - 최악의 경우 모든 음식을 섞어야 하므로 O(N log N)이 됩니다.
- 공간 복잡도: O(N)
  - 힙에 저장되는 요소의 개수만큼의 공간이 필요합니다.

이 방법을 사용하면 효율적으로 모든 음식의 스코빌 지수를 K 이상으로 만들 수 있습니다. 