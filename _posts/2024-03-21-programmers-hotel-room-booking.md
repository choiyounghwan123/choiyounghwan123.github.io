---
title: "[프로그래머스] 호텔 대실"
excerpt: "그리디 알고리즘을 활용한 호텔 객실 예약 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, 그리디, Greedy, Python]

permalink: /algorithm/hotel-room-booking/

toc: true
toc_sticky: true

date: 2025-05-19
last_modified_at: 2025-05-29
---

# 문제 설명

호텔 대실 문제는 호텔의 예약 시간이 주어졌을 때, 필요한 최소 객실의 수를 구하는 문제입니다.

- 각 예약은 입실 시간과 퇴실 시간이 주어집니다.
- 퇴실 시간은 청소 시간 10분이 포함됩니다.
- 한 객실은 동시에 하나의 예약만 가능합니다.
- 시간은 "HH:MM" 형식으로 주어집니다.

# 풀이 방법

이 문제는 그리디 알고리즘을 사용하여 해결할 수 있습니다. 다음과 같은 단계로 풀이합니다:

1. 예약 시간을 입실 시간 기준으로 정렬합니다.
2. 각 예약에 대해:
   - 기존 객실 중에서 예약이 가능한 객실이 있는지 확인합니다.
   - 가능한 객실이 있으면 해당 객실에 예약을 추가합니다.
   - 가능한 객실이 없으면 새로운 객실을 추가합니다.
3. 최종적으로 필요한 객실의 수를 반환합니다.

# 코드

```python
def solution(book_time):
    rooms = []
    book_time.sort(key=lambda x:x[0])  # 입실 시간 기준으로 정렬
    
    for start_time, end_time in book_time:
        # 시간 문자열을 정수로 변환 (HH:MM -> HHMM)
        start_time, end_time = start_time.split(":"), end_time.split(":")
        start_time = int(start_time[0] + start_time[1])
        end_time = int(end_time[0] + end_time[1]) + 10  # 청소 시간 10분 추가
        
        # 시간이 60분을 넘어가는 경우 처리
        end_time_str = str(end_time)
        if int(end_time_str[-2] + end_time_str[-1]) >= 60:
            end_time = end_time + 100 - 60
        
        # 첫 예약인 경우
        if not rooms:
            rooms.append([[start_time, end_time]])
            continue
        
        # 기존 객실 중 예약 가능한 객실 찾기
        check = 0
        for i in range(len(rooms)):
            check1 = 0
            check = 0
            for s, e in rooms[i]:
                # 시간이 겹치는지 확인
                if s <= start_time < e or s < end_time <= e:
                    check = 1
                    check1 = 1
                    break
            if check1:
                continue
            else:
                # 예약 가능한 객실을 찾은 경우
                rooms[i].append([start_time, end_time])
                check = 0
                break
        
        # 예약 가능한 객실이 없는 경우 새로운 객실 추가
        if check == 1:
            rooms.append([[start_time, end_time]])
            
    return len(rooms)
```

# 코드 설명

1. 예약 시간을 입실 시간 기준으로 정렬합니다.
2. 각 예약에 대해:
   - 시간 문자열을 정수로 변환합니다 (예: "14:30" -> 1430)
   - 퇴실 시간에 청소 시간 10분을 추가합니다
   - 퇴실 시간이 60분을 넘어가는 경우 시간을 조정합니다
3. 객실 배정:
   - 첫 예약인 경우 새로운 객실을 생성합니다
   - 기존 객실 중에서 예약이 가능한 객실을 찾습니다
   - 예약 가능한 객실이 없으면 새로운 객실을 추가합니다
4. 최종적으로 필요한 객실의 수를 반환합니다

# 시간 복잡도

- 시간 복잡도: O(N * M)
  - N: 예약의 개수
  - M: 객실의 개수
- 공간 복잡도: O(N)

각 예약에 대해 모든 객실을 확인해야 하므로, 시간 복잡도는 예약의 개수와 객실의 개수의 곱에 비례합니다.

# 예시

입력: `[["15:00", "17:00"], ["16:40", "18:20"], ["14:20", "15:20"], ["14:10", "19:20"], ["18:20", "21:20"]]`

1. 예약을 입실 시간 기준으로 정렬
2. 각 예약에 대해 객실 배정
3. 결과: 3개의 객실이 필요

이 문제는 실제 호텔 예약 시스템에서 사용될 수 있는 알고리즘의 기본 형태를 보여줍니다. 