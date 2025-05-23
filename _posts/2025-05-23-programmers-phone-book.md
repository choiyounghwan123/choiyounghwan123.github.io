---
title: "[프로그래머스] 전화번호 목록"
excerpt: "해시맵을 활용한 전화번호 목록 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 해시]

permalink: /algorithm/phone-book/

toc: true
toc_sticky: true

date: 2025-05-24
last_modified_at: 2025-05-24
---

# 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하는 문제입니다.

- 전화번호부에 적힌 전화번호가 문자열 배열로 주어집니다.
- 한 번호가 다른 번호의 접두어라면, false를 반환해야 합니다.
- 그렇지 않으면 true를 반환합니다.

# 제한사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
- 각 전화번호의 길이는 1 이상 20 이하입니다.
- 같은 전화번호가 중복해서 들어있지 않습니다.

# 입출력 예

```
입력: ["119", "97674223", "1195524421"]
출력: False
```

# 풀이 방법

이 문제는 해시맵을 활용하여 효율적으로 풀 수 있습니다. 모든 전화번호를 해시맵에 저장한 뒤, 각 번호의 접두어가 해시맵에 존재하는지 확인합니다. 만약 접두어가 존재하고, 그 접두어가 자기 자신이 아니라면 false를 반환합니다.

# 코드

```python
def solution(phone_book):
    hash_map = dict()
    for number in phone_book:
        hash_map[number] = 1

    for number in phone_book:
        arr = ""
        for n in number:
            arr += n
            if arr in hash_map and arr != number:
                return False
    return True

print(solution(["119", "97674223", "1195524421"]))
```

# 코드 설명

1. 모든 전화번호를 해시맵에 저장합니다.
2. 각 전화번호에 대해, 한 글자씩 접두어를 만들어가며 해시맵에 존재하는지 확인합니다.
3. 만약 접두어가 해시맵에 존재하고, 그 접두어가 자기 자신이 아니라면 false를 반환합니다.
4. 모든 번호에 대해 접두어가 없으면 true를 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n * m)
  - n: phone_book의 길이
  - m: 전화번호의 최대 길이(최대 20)
- 공간 복잡도: O(n)
  - n: 해시맵에 저장되는 전화번호의 수

각 전화번호의 모든 접두어를 확인하므로, 시간 복잡도는 O(n * m)입니다. 해시맵에 전화번호를 저장하므로 공간 복잡도는 O(n)입니다. 