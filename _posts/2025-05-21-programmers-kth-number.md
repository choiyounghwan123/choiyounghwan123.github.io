---
title: "[프로그래머스] K번째 수"
excerpt: "배열 자르기와 정렬을 활용한 K번째 수 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 정렬, Python]

permalink: /algorithm/kth-number/

toc: true
toc_sticky: true

date: 2025-05-22
last_modified_at: 2025-05-22
---

# 문제 설명

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하는 문제입니다.

- array의 i번째 숫자부터 j번째 숫자까지 자른 후 정렬합니다.
- 정렬된 배열에서 k번째 숫자를 찾습니다.
- 여러 개의 명령어가 주어질 경우, 각 명령어에 대한 결과를 배열로 반환합니다.

# 제한사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

# 입출력 예

```
입력: 
array = [1, 5, 2, 6, 3, 7, 4]
commands = [[2, 5, 3], [4, 4, 1], [1, 7, 3]]

출력: [5, 6, 3]
```

# 풀이 방법

이 문제는 배열의 특정 구간을 자르고 정렬한 후, k번째 수를 찾는 간단한 문제입니다. Python의 슬라이싱과 정렬 기능을 활용하여 해결할 수 있습니다.

# 코드

```python
def solution(array, commands):
    answer = []
    for command in commands:
        # command[0]: 시작 인덱스
        # command[1]: 끝 인덱스
        # command[2]: 찾을 인덱스
        answer.append(sorted(array[command[0]-1:command[1]])[command[2]-1])
    return answer
```

# 코드 설명

1. 명령어 처리:
   - 각 command는 [i, j, k] 형태로 주어집니다.
   - i: 시작 인덱스 (1-based)
   - j: 끝 인덱스 (1-based)
   - k: 찾을 인덱스 (1-based)

2. 배열 처리 과정:
   - `array[command[0]-1:command[1]]`: i번째부터 j번째까지의 배열을 자릅니다.
   - `sorted()`: 자른 배열을 정렬합니다.
   - `[command[2]-1]`: 정렬된 배열에서 k번째 수를 찾습니다.

3. 결과 저장:
   - 각 명령어에 대한 결과를 answer 배열에 추가합니다.

# 시간 복잡도

- 시간 복잡도: O(n * m * log m)
  - n: commands의 길이
  - m: 각 command에서 자르는 배열의 길이
- 공간 복잡도: O(m)

각 명령어마다 배열을 자르고 정렬하는 작업이 필요하므로, 시간 복잡도는 명령어의 개수와 각 명령어에서 처리하는 배열의 길이에 비례합니다. 