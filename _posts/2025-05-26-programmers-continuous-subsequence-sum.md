---
title: "[프로그래머스] 연속된 부분 수열의 합"
excerpt: "투 포인터와 누적합을 활용한 연속된 부분 수열의 합 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 투 포인터, 누적합]

permalink: /algorithm/continuous-subsequence-sum/

toc: true
toc_sticky: true

date: 2025-05-26
last_modified_at: 2025-05-26
---

# 문제 설명

수열에서 연속된 부분 수열의 합이 k가 되는 (가장 짧은) 구간의 시작과 끝 인덱스를 구하는 문제입니다. 여러 구간이 있을 경우, 가장 앞에 있는 구간을 반환합니다.

- 부분 수열은 반드시 연속되어야 합니다.
- 합이 k가 되는 구간이 여러 개라면, 가장 짧은 구간을 반환합니다.
- 가장 짧은 구간이 여러 개라면, 시작 인덱스가 가장 작은 구간을 반환합니다.

# 제한사항

- 5 ≤ sequence의 길이 ≤ 1,000,000
- 1 ≤ sequence의 원소 ≤ 1,000
- 1 ≤ k ≤ 1,000,000,000

# 입출력 예

```
입력:
sequence = [1, 1, 1, 2, 3, 4, 5]
k = 5

출력: [2, 4]
```

# 풀이 방법

이 문제는 투 포인터(Two Pointer)와 누적합(Prefix Sum)을 활용하여 효율적으로 해결할 수 있습니다. 

1. 누적합 배열을 만들어 각 구간의 합을 빠르게 계산할 수 있도록 합니다.
2. left, right 포인터를 사용해 부분 수열의 합이 k가 되는 구간을 찾습니다.
3. 합이 k보다 작으면 right를 늘리고, 크면 left를 늘립니다.
4. 합이 k와 같으면, 현재 구간이 기존 답보다 짧으면 갱신합니다.

# 코드

```python
def solution(sequence, k):
    left, right = 0, 1
    answer = [0, 1000000000]
    prefix_sum = [0]
    for num in sequence:
        prefix_sum.append(prefix_sum[-1] + num)
    while right < len(prefix_sum):
        if prefix_sum[right] - prefix_sum[left] < k:
            right += 1
        elif prefix_sum[right] - prefix_sum[left] > k:
            left += 1
        else:
            if answer[1] - answer[0] > right - left:
                answer = [left, right]
            right += 1
    return [answer[0], answer[1] - 1]

print(solution([1, 1, 1, 2, 3, 4, 5], 5))  # [2, 4]
```

# 코드 설명

- 누적합(prefix_sum) 배열을 만들어 각 구간의 합을 O(1)에 구할 수 있습니다.
- left, right 포인터로 구간을 조절하며 합이 k가 되는 구간을 찾습니다.
- 조건에 맞는 구간이 여러 개일 때, 더 짧은 구간을 우선적으로 선택합니다.
- 반환 시, 실제 인덱스는 [left, right-1]이므로 answer[1] - 1로 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N)
  - 각 원소를 한 번씩만 확인하므로 선형 시간에 해결할 수 있습니다.
- 공간 복잡도: O(N)
  - 누적합 배열을 저장하는 데 O(N) 공간이 필요합니다.

투 포인터와 누적합을 활용하면 대용량 데이터에서도 효율적으로 부분 수열의 합 문제를 해결할 수 있습니다. 