---
title: "[프로그래머스] 신고 결과 받기"
excerpt: "해시 맵을 활용한 신고 결과 처리 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 해시맵, Hash Map]

permalink: /algorithm/report-results/

toc: true
toc_sticky: true

date: 2024-03-20
last_modified_at: 2024-03-20
---

## 문제 설명

신고 시스템에서 각 유저가 신고한 유저의 목록과 정지 기준이 되는 신고 횟수 k가 주어졌을 때, 각 유저별로 처리 결과 메일을 받게 되는 횟수를 구하는 문제입니다. 한 유저가 같은 유저를 여러 번 신고해도 1회로 처리됩니다.

## 접근 방법

1. defaultdict를 사용하여 신고당한 유저를 키로, 신고한 유저들의 집합을 값으로 하는 해시 맵을 생성합니다.
2. 신고 목록을 순회하며 해시 맵을 구성합니다.
3. 각 신고당한 유저에 대해 신고 횟수가 k 이상인 경우, 해당 유저를 신고한 모든 유저의 메일 수신 횟수를 증가시킵니다.

## 코드 설명

```python
from collections import defaultdict

def solution(id_list, report, k):
    # 신고당한 유저를 키로, 신고한 유저들의 집합을 값으로 하는 해시 맵 생성
    hash_map = defaultdict(set)
    # 각 유저별 메일 수신 횟수를 저장할 배열 초기화
    answer = [0] * len(id_list)
    
    # 신고 목록을 순회하며 해시 맵 구성
    for i in range(len(report)):
        reporter, reportedUser = report[i].split(" ")
        hash_map[reportedUser].add(reporter)
    
    # 각 신고당한 유저에 대해 처리
    for key, item in hash_map.items():
        item = list(item)
        # 신고 횟수가 k 이상인 경우
        if len(item) >= k:
            # 해당 유저를 신고한 모든 유저의 메일 수신 횟수 증가
            for i in range(len(item)):
                answer[id_list.index(item[i])] += 1
                
    return answer
```

## 예시 설명

입력: 
- id_list = ["muzi", "frodo", "apeach", "neo"]
- report = ["muzi frodo","apeach frodo","frodo neo","muzi neo","apeach muzi"]
- k = 2

처리 과정:
1. 해시 맵 구성:
   - frodo: {muzi, apeach}
   - neo: {frodo, muzi}
   - muzi: {apeach}

2. 신고 횟수 확인:
   - frodo: 2회 신고 → muzi, apeach에게 메일 발송
   - neo: 2회 신고 → frodo, muzi에게 메일 발송
   - muzi: 1회 신고 → 메일 발송 없음

3. 결과: [2, 1, 1, 0]
   - muzi: 2회 (frodo, neo 신고 처리)
   - frodo: 1회 (neo 신고 처리)
   - apeach: 1회 (frodo 신고 처리)
   - neo: 0회

## 시간 복잡도

- 해시 맵 구성: O(n), n은 report 배열의 길이
- 신고 처리: O(m * n), m은 id_list의 길이
- 전체 시간 복잡도: O(m * n)

## 공간 복잡도

- 해시 맵: O(n)
- 결과 배열: O(m)
- 전체 공간 복잡도: O(n + m)

## 결론

이 문제는 해시 맵을 활용하여 효율적으로 해결할 수 있습니다. defaultdict와 set을 사용하여 중복 신고를 자동으로 처리하고, 신고 횟수를 쉽게 계산할 수 있습니다. 시간 복잡도는 O(m * n)으로, 주어진 입력 크기에 대해 효율적으로 동작합니다. 