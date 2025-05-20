---
title: "[2025 프로그래머스 코드챌린지 1차 예선] 비밀 코드 해독"
excerpt: "조합과 시뮬레이션을 활용한 비밀 코드 해독 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 조합, 시뮬레이션, Python]

permalink: /algorithm/secret-code/

toc: true
toc_sticky: true

date: 2025-05-20
last_modified_at: 2025-05-20
---

# 문제 설명

비밀 코드를 해독하는 문제입니다. 주어진 숫자들 중에서 5개의 숫자를 선택하여 비밀 코드를 만들 수 있습니다. 각 숫자 조합에 대해 주어진 조건을 만족하는 경우의 수를 구해야 합니다.

- 1부터 n까지의 숫자 중에서 5개의 숫자를 선택합니다.
- 각 숫자 조합에 대해 주어진 조건을 만족하는지 확인합니다.
- 조건은 각 숫자 조합이 주어진 질문에 대해 몇 개의 숫자가 일치하는지를 나타냅니다.

# 제한사항

- 5 ≤ n ≤ 20
- 1 ≤ q의 길이 ≤ 10
- q의 각 행은 5개의 숫자로 구성됩니다.
- ans의 길이는 q의 길이와 같습니다.
- 0 ≤ ans의 각 원소 ≤ 5

# 입출력 예

```
입력: 
n = 10
q = [[1, 2, 3, 4, 5], [6, 7, 8, 9, 10], [3, 7, 8, 9, 10], [2, 5, 7, 9, 10], [3, 4, 5, 6, 7]]
ans = [2, 3, 4, 3, 3]

출력: 1
```

# 풀이 방법

이 문제는 조합과 시뮬레이션을 활용하여 해결할 수 있습니다. 먼저 가능한 숫자들을 필터링한 후, 남은 숫자들로 가능한 모든 5개 숫자 조합을 만들어 조건을 만족하는지 확인합니다.

# 코드

```python
from itertools import combinations

def solution(n, q, ans):
    # 가능한 모든 숫자 생성
    num = [i for i in range(1, n + 1)]
    # 답이 0인 인덱스 찾기
    index = list(filter(lambda i: ans[i] == 0, range(len(ans))))

    # 답이 0인 질문에 포함된 숫자들은 제외
    for i in index:
        for j in q[i]:
            if j in num:
                num.remove(j)
    
    result = 0
    # 남은 숫자들로 가능한 모든 5개 숫자 조합 생성
    for i in combinations(num, 5):
        temp = []
        # 각 질문에 대해 일치하는 숫자의 개수 계산
        for j in range(len(q)):
            count = 0
            for k in range(5):
                if i[k] in q[j]:
                    count += 1
            temp.append(count)

        # 모든 조건을 만족하는지 확인
        for i in range(len(ans)):
            if temp[i] != ans[i]:
                break
        else:
            result += 1
    return result
```

# 코드 설명

1. 가능한 모든 숫자(1부터 n까지)를 생성합니다.
2. 답이 0인 질문에 포함된 숫자들은 제외합니다.
3. 남은 숫자들로 가능한 모든 5개 숫자 조합을 생성합니다.
4. 각 조합에 대해:
   - 각 질문에 대해 일치하는 숫자의 개수를 계산합니다.
   - 모든 조건을 만족하는지 확인합니다.
5. 조건을 만족하는 조합의 개수를 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(C(n,5) * q)
  - C(n,5): n개 중 5개를 선택하는 조합의 수
  - q: 질문의 개수
- 공간 복잡도: O(n)

가능한 모든 5개 숫자 조합을 확인하고, 각 조합에 대해 모든 질문을 검사하므로, 시간 복잡도는 조합의 수와 질문의 개수에 비례합니다. 