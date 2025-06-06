---
title: "[프로그래머스] 기능개발"
excerpt: "큐를 활용한 기능개발 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, 큐]

permalink: /algorithm/function-development/

toc: true
toc_sticky: true

date: 2025-05-27
last_modified_at: 2025-05-27
---

# 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.
또한, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.
먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하는 문제입니다.

# 제한사항

- 작업의 개수(progresses, speeds 배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어집니다.

# 입출력 예

```
입력:
progresses = [93, 30, 55]
speeds = [1, 30, 5]

출력: [2, 1]
```

# 풀이 방법

이 문제는 큐를 활용하여 해결할 수 있습니다.

1. 각 기능의 진행도를 매일 업데이트합니다.
2. 첫 번째 기능이 100% 이상이 되면, 연속된 100% 이상의 기능들을 모두 배포합니다.
3. 배포된 기능의 개수를 결과 배열에 추가합니다.
4. 모든 기능이 배포될 때까지 반복합니다.

# 코드

```python
def solution(progresses, speeds):
    answer = []
    
    while progresses:  # 모든 기능이 배포될 때까지 반복
        # 각 기능의 진행도 업데이트
        for i in range(len(progresses)):
            progresses[i] += speeds[i]
        
        # 배포할 수 있는 기능 확인
        temp = 0
        while progresses and progresses[0] >= 100:
            progresses.pop(0)  # 첫 번째 기능 제거
            speeds.pop(0)      # 해당 기능의 속도 제거
            temp += 1          # 배포할 기능 수 증가
        
        # 배포할 기능이 있으면 결과에 추가
        if temp:
            answer.append(temp)
            
    return answer

print(solution([93, 30, 55], [1, 30, 5]))  # [2, 1]
```

# 코드 설명

1. `progresses`와 `speeds` 배열이 비어있을 때까지 반복합니다.
2. 매일 각 기능의 진행도를 업데이트합니다.
3. 첫 번째 기능이 100% 이상이 되면:
   - 연속된 100% 이상의 기능들을 모두 제거합니다.
   - 제거된 기능의 수를 `temp`에 저장합니다.
4. `temp`가 0보다 크면 결과 배열에 추가합니다.
5. 최종적으로 각 배포마다 배포된 기능의 수를 담은 배열을 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N²)
  - 최악의 경우 모든 기능이 순차적으로 배포되어야 할 때 O(N²) 시간이 소요됩니다.
- 공간 복잡도: O(N)
  - 결과를 저장하는 배열에 O(N) 공간이 필요합니다.

이 방법을 사용하면 효율적으로 기능 배포 일정을 계산할 수 있습니다. 