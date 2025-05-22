---
title: "[2021 카카오 채용연계형 인턴십] 숫자 문자열과 영단어"
excerpt: "문자열 치환을 활용한 숫자 문자열과 영단어 문제 풀이"

categories:
  - Algorithm
tags:
  - [카카오, 문자열, Python]

permalink: /algorithm/number-string/

toc: true
toc_sticky: true

date: 2025-05-22
last_modified_at: 2025-05-22
---

# 문제 설명

숫자의 일부 자릿수가 영단어로 바뀌어져 있는 문자열을 원래 숫자로 바꾸는 문제입니다. 예를 들어, "one4seveneight"은 1478로 변환됩니다.

- 0부터 9까지의 숫자가 영단어로 바뀌어져 있을 수 있습니다.
- 영단어는 다음과 같이 변환됩니다:
  - zero → 0
  - one → 1
  - two → 2
  - three → 3
  - four → 4
  - five → 5
  - six → 6
  - seven → 7
  - eight → 8
  - nine → 9

# 제한사항

- 1 ≤ s의 길이 ≤ 50
- s가 "zero" 또는 "0"으로 시작하는 경우는 없습니다.
- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 주어집니다.

# 입출력 예

```
입력: 
s = "one4seveneight"

출력: 1478
```

# 풀이 방법

이 문제는 문자열 치환을 통해 해결할 수 있습니다. 각 영단어를 해당하는 숫자로 변환하는 과정을 반복하면 됩니다.

# 코드

```python
def solution(s):
    # 숫자와 영단어 매핑
    hash_map = {
        0: 'zero',
        1: 'one',
        2: 'two',
        3: 'three',
        4: 'four',
        5: 'five',
        6: 'six',
        7: 'seven',
        8: 'eight',
        9: 'nine'
    }
    
    # 각 영단어를 숫자로 변환
    for key, value in hash_map.items():
        while value in s:
            s = s.replace(value, str(key))
            
    return int(s)
```

# 코드 설명

1. 숫자-영단어 매핑:
   - `hash_map` 딕셔너리를 사용하여 각 숫자와 해당하는 영단어를 매핑합니다.
   - 0부터 9까지의 숫자와 영단어가 1:1로 매핑됩니다.

2. 문자열 변환:
   - 각 영단어가 문자열에 있는지 확인합니다.
   - 영단어가 발견되면 해당하는 숫자로 치환합니다.
   - 모든 영단어가 숫자로 변환될 때까지 반복합니다.

3. 결과 반환:
   - 변환된 문자열을 정수로 변환하여 반환합니다.

# 시간 복잡도

- 시간 복잡도: O(n * m)
  - n: 문자열의 길이
  - m: 영단어의 개수 (10개)
- 공간 복잡도: O(1)

각 영단어에 대해 문자열을 한 번씩 검사하고 치환하는 작업이 필요하므로, 시간 복잡도는 문자열의 길이와 영단어의 개수의 곱에 비례합니다. 