---
title: "[프로그래머스] 개인정보 수집 유효기간"
excerpt: "날짜 계산을 활용한 개인정보 수집 유효기간 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 구현]

permalink: /algorithm/privacy-collection/

toc: true
toc_sticky: true

date: 2024-03-26
last_modified_at: 2024-03-26
---

# 문제 설명

고객의 개인정보를 수집한 날짜와 약관 종류가 주어졌을 때, 파기해야 할 개인정보의 번호를 구하는 문제입니다.

- 각 약관마다 개인정보 보관 유효기간이 달라집니다.
- 유효기간이 지난 개인정보는 파기해야 합니다.
- 오늘 날짜가 주어지고, 이 날짜를 기준으로 파기할 개인정보를 판단합니다.

# 제한사항

- today는 "YYYY.MM.DD" 형태로 주어집니다.
- terms는 약관 종류와 유효기간이 공백으로 구분되어 주어집니다.
- privacies는 수집된 개인정보의 날짜와 약관 종류가 공백으로 구분되어 주어집니다.
- 모든 날짜는 2000년 1월 1일 이상, 2022년 12월 28일 이하입니다.

# 입출력 예

```
입력: 
today = "2019.01.01"
terms = ["B 12"]
privacies = ["2017.12.21 B"]

출력: [1]
```

# 풀이 방법

1. 날짜를 연, 월, 일로 분리하여 계산합니다.
2. 약관 종류와 유효기간을 해시맵에 저장합니다.
3. 각 개인정보에 대해:
   - 수집일자에 유효기간을 더해 만료일을 계산합니다.
   - 만료일이 오늘 날짜보다 이전이면 파기 대상입니다.

# 코드

```python
def change(date, term_month):
    year, month, day = date.split(".")
    year = int(year)
    month = int(month) + int(term_month)
    while month > 12:
        year += 1
        month -= 12
    if month == 0:
        month = 12

    return [int(year), int(month), int(day)]

def solution(today, terms, privacies):
    result = []
    hash_map = dict()
    today_year, today_month, today_day = today.split(".")
    today_year = int(today_year)
    today_month = int(today_month)
    today_day = int(today_day)
    
    for term in terms:
        a, b = term.split(" ")
        hash_map[a] = b
        
    for i in range(len(privacies)):
        date, term = privacies[i].split(" ")
        validation_date = change(date, hash_map[term])
        
        if validation_date[0] < today_year:
            result.append(i+1)
        elif today_year == validation_date[0] and validation_date[1] < today_month:
            result.append(i+1)
        elif today_year == validation_date[0] and validation_date[1] == today_month and validation_date[2] <= today_day:
            result.append(i+1)
            
    return result
```

# 코드 설명

1. `change` 함수:
   - 날짜와 유효기간(월)을 입력받아 만료일을 계산합니다.
   - 월이 12를 초과하면 연도를 증가시키고 월을 조정합니다.
   - 월이 0이 되는 경우를 처리합니다.

2. `solution` 함수:
   - 오늘 날짜를 연, 월, 일로 분리합니다.
   - 약관 종류와 유효기간을 해시맵에 저장합니다.
   - 각 개인정보에 대해:
     - 수집일자와 약관 종류를 분리합니다.
     - 만료일을 계산합니다.
     - 만료일이 오늘 날짜보다 이전이면 결과 리스트에 추가합니다.

# 시간 복잡도

- 시간 복잡도: O(n)
  - n: privacies 배열의 길이
- 공간 복잡도: O(m)
  - m: terms 배열의 길이

각 개인정보에 대해 한 번씩 순회하면서 만료일을 계산하므로, 시간 복잡도는 O(n)입니다. 