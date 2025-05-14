---
title: "[프로그래머스] 덧칠하기 - Python"
excerpt: "그리디 알고리즘을 활용한 프로그래머스 덧칠하기 문제 풀이"

categories:
  - Algorithm
tags:
  - 프로그래머스
  - 그리디
  - Python

permalink: /algorithm/programmers-paint/

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
comments: true
---

## 🦥 문제 설명
어느 벽에 칠해야 할 구역들이 주어집니다. 한 번에 롤러로 칠할 수 있는 길이 `m`이 주어질 때, 모든 구역을 칠하기 위해 롤러를 최소 몇 번 사용해야 하는지 구하는 문제입니다.

- `n`: 벽의 길이
- `m`: 롤러의 길이
- `section`: 칠해야 하는 구역의 시작점 리스트

## 💡 접근 방법
- **그리디 알고리즘**을 사용합니다.
- section을 순서대로 탐색하면서, 롤러로 칠할 수 없는 구간이 나오면 롤러를 한 번 더 사용합니다.
- 현재 롤러로 칠할 수 있는 마지막 위치를 갱신하며 진행합니다.

## 🛠️ 코드
```python
def solution(n, m, section):
    paint = section[0]
    result = 1  # 첫 구간은 무조건 칠해야 하므로 1로 시작

    for i in range(1, len(section)):
        if section[i] - paint >= m:
            result += 1
            paint = section[i]

    return result

print(solution(16, 4, [2, 3, 15, 16]))  # 예시 출력: 3
```

## ⏰ 시간 복잡도
- O(N) (section의 길이만큼 한 번씩 순회)

## 📝 정리
- 이 문제는 그리디 알고리즘의 대표적인 예시입니다.
- 롤러로 칠할 수 있는 최대 범위를 항상 유지하면서, 새로운 구간이 나오면 롤러를 추가로 사용하면 됩니다.
- 실전 코딩테스트에서 자주 등장하는 유형이니 꼭 익혀두세요!

