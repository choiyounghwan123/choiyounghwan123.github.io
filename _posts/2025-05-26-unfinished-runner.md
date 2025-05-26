---
title: "[프로그래머스] 완주하지 못한 선수"
excerpt: "Counter를 활용한 완주하지 못한 선수 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, Counter, 해시]

permalink: /algorithm/unfinished-runner/

toc: true
toc_sticky: true

date: 2025-05-26
last_modified_at: 2025-05-26
---

# 문제 설명

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하는 문제입니다.

# 제한사항

- 참가자 중에는 동명이인이 있을 수 있습니다.
- 1 ≤ 참가자 수 ≤ 100,000
- completion의 길이는 participant의 길이보다 1 작습니다.

# 입출력 예

```
입력:
participant = ["leo", "kiki", "eden"]
completion = ["eden", "kiki"]

출력: "leo"
```

# 풀이 방법

이 문제는 Counter를 활용하여 해결할 수 있습니다.

1. completion 배열의 각 선수 이름의 등장 횟수를 Counter로 계산합니다.
2. participant 배열을 순회하면서 Counter에서 해당 선수를 제거합니다.
3. Counter에 없는 선수가 완주하지 못한 선수입니다.

# 코드

```python
from collections import Counter

def solution(participant, completion):
    counter = Counter(completion)

    for p in participant:
        if p not in counter:
            return p
        else:
            counter[p] -= 1
            if counter[p] == 0:
                del counter[p]

print(solution(["leo", "kiki", "eden"], ["eden", "kiki"]))  # "leo"
```

# 코드 설명

1. `Counter`를 사용하여 완주한 선수들의 이름과 등장 횟수를 저장합니다.
2. 참가자 배열을 순회하면서:
   - 해당 선수가 Counter에 없으면 완주하지 못한 선수입니다.
   - Counter에 있으면 등장 횟수를 1 감소시키고, 0이 되면 Counter에서 제거합니다.
3. 완주하지 못한 선수의 이름을 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N)
  - Counter 생성에 O(N)
  - 참가자 배열 순회에 O(N)
- 공간 복잡도: O(N)
  - Counter를 저장하는 데 O(N) 공간이 필요합니다.

Counter를 활용하면 효율적으로 완주하지 못한 선수를 찾을 수 있습니다. 