---
title: "[프로그래머스] 대충 만든 자판 - Python"
excerpt: "프로그래머스 대충 만든 자판 문제 풀이와 해설"

categories:
  - Algorithm
tags:
  - [Python, Algorithm, Programmers, Hash]

permalink: /algorithm/roughly-made-keypad/

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
---

## 문제 설명

대충 만든 자판은 프로그래머스에서 제공하는 코딩 테스트 문제입니다. 이 문제는 주어진 자판 배열과 목표 문자열들을 이용하여 각 목표 문자열을 입력하는데 필요한 최소한의 키 입력 횟수를 구하는 문제입니다.

### 문제 요구사항
- `keymap`: 자판의 배열이 담긴 문자열 배열
- `targets`: 입력하려는 문자열들이 담긴 배열
- 각 문자를 입력하기 위해 필요한 최소한의 키 입력 횟수를 구해야 함
- 만약 목표 문자열을 완성할 수 없다면 -1을 반환

## 문제 풀이

이 문제는 해시맵을 활용하여 효율적으로 풀 수 있습니다. 각 문자가 자판의 어느 위치에 있는지 저장하고, 최소한의 키 입력 횟수를 계산하는 방식으로 접근했습니다.

### 접근 방법
1. 각 문자가 자판의 어느 위치에 있는지 해시맵에 저장
2. 목표 문자열의 각 문자에 대해 최소 키 입력 횟수 계산
3. 불가능한 경우 -1 반환

### 코드 구현

```python
from collections import defaultdict

def solution(keymap, targets):
    # 각 문자의 위치를 저장할 해시맵 생성
    hash_map = defaultdict(list)
    
    # 각 자판의 문자 위치 정보 저장
    for key in keymap:
        for idx, char in enumerate(key):
            hash_map[char].append(idx + 1)
    
    result = []
    
    # 각 목표 문자열에 대해 처리
    for target in targets:
        temp = 0
        for char in target:
            if char in hash_map:
                # 해당 문자의 최소 키 입력 횟수 더하기
                temp += min(hash_map[char])
            else:
                # 문자가 자판에 없는 경우
                temp = -1
                break
        
        result.append(temp)
    
    return result
```

### 코드 설명

1. `defaultdict(list)`를 사용하여 각 문자의 위치 정보를 저장할 해시맵을 생성합니다.
2. 각 자판의 문자와 그 위치를 해시맵에 저장합니다.
3. 목표 문자열을 순회하면서:
   - 각 문자가 자판에 있는지 확인
   - 있다면 해당 문자의 최소 키 입력 횟수를 더함
   - 없다면 -1을 반환
4. 최종 결과를 반환합니다.

### 시간 복잡도
- 자판 정보 저장: O(N * M) (N: 자판 개수, M: 자판의 최대 길이)
- 목표 문자열 처리: O(K * L) (K: 목표 문자열 개수, L: 목표 문자열의 최대 길이)
- 전체 시간 복잡도: O(N * M + K * L)

### 공간 복잡도
- 해시맵 저장: O(N * M)
- 결과 배열: O(K)
- 전체 공간 복잡도: O(N * M + K)

## 테스트 케이스

```python
# 테스트 케이스 1
print(solution(["ABACD", "BCEFD"], ["ABCD","AABB"]))  # [9, 4]

# 테스트 케이스 2
print(solution(["AA"], ["B"]))  # [-1]

# 테스트 케이스 3
print(solution(["AGZ", "BSSS"], ["ASA","BGZ"]))  # [4, 6]
```

## 결론

이 문제는 해시맵을 활용하여 각 문자의 위치 정보를 효율적으로 저장하고 관리하는 것이 핵심이었습니다. 특히 `defaultdict`를 사용하여 코드를 더 간결하고 효율적으로 작성할 수 있었습니다. 실제 코딩 테스트에서도 자주 사용되는 해시맵 자료구조를 활용한 좋은 예제라고 생각합니다.
