---
title: "[프로그래머스] 베스트앨범"
excerpt: "defaultdict를 활용한 베스트앨범 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, defaultdict, 정렬]

permalink: /algorithm/best-album/

toc: true
toc_sticky: true

date: 2025-05-27
last_modified_at: 2025-05-27
---

# 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다:

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

# 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

# 입출력 예

```
입력:
genres = ["classic", "classic", "classic"]
plays = [500, 500, 500]

출력: [0, 1]
```

# 풀이 방법

이 문제는 defaultdict를 활용하여 해결할 수 있습니다.

1. 장르별 노래 정보와 총 재생 횟수를 저장할 두 개의 defaultdict를 생성합니다.
2. 장르별 총 재생 횟수를 기준으로 정렬합니다.
3. 각 장르별로 노래를 재생 횟수와 고유 번호를 기준으로 정렬합니다.
4. 각 장르에서 최대 2개의 노래를 선택합니다.

# 코드

```python
from collections import defaultdict

def solution(genres, plays):
    hash_map = defaultdict(list)  # 장르별 노래 정보 저장
    hash_map1 = defaultdict(int)  # 장르별 총 재생 횟수 저장
    answer = []
    
    # 장르별 노래 정보와 총 재생 횟수 계산
    for i, (genre, play) in enumerate(zip(genres, plays)):
        hash_map[genre].append([play, i])
        hash_map1[genre] += play
    
    # 장르별 총 재생 횟수로 정렬
    sorted_genres = sorted(hash_map1.items(), key=lambda x: x[1])
    
    # 각 장르별로 노래 선택
    for i in range(len(sorted_genres)):
        genre = sorted_genres[-1 - i][0]  # 재생 횟수가 많은 장르부터
        hash_map[genre].sort(key=lambda x: (x[0], -x[1]))  # 재생 횟수 내림차순, 고유 번호 오름차순
        
        # 최대 2개의 노래 선택
        j = 0
        while j < 2 and hash_map[genre]:
            answer.append(hash_map[genre].pop(-1)[1])
            j += 1
            
    return answer

print(solution(["classic", "classic", "classic"], [500, 500, 500]))  # [0, 1]
```

# 코드 설명

1. `hash_map`: 장르별로 [재생 횟수, 고유 번호] 쌍을 저장합니다.
2. `hash_map1`: 장르별 총 재생 횟수를 저장합니다.
3. 각 노래에 대해:
   - 장르별 노래 정보를 `hash_map`에 추가합니다.
   - 장르별 총 재생 횟수를 `hash_map1`에 누적합니다.
4. 장르를 총 재생 횟수 기준으로 정렬합니다.
5. 각 장르별로:
   - 노래를 재생 횟수(내림차순)와 고유 번호(오름차순) 기준으로 정렬합니다.
   - 최대 2개의 노래를 선택하여 answer에 추가합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N log N)
  - 장르 정렬에 O(G log G), G는 장르의 수
  - 각 장르별 노래 정렬에 O(N log N)
- 공간 복잡도: O(N)
  - 장르별 노래 정보와 총 재생 횟수를 저장하는 데 O(N) 공간이 필요합니다.

defaultdict를 활용하면 효율적으로 베스트앨범을 구성할 수 있습니다. 