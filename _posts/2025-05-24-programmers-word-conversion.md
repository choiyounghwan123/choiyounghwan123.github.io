---
title: "[프로그래머스] 단어 변환"
excerpt: "BFS를 활용한 단어 변환(Word Conversion) 문제 풀이"

categories:
  - Algorithm
tags:
  - [프로그래머스, Python, BFS, 그래프]

permalink: /algorithm/word-conversion/

toc: true
toc_sticky: true

date: 2025-05-25
last_modified_at: 2025-05-25
---

# 문제 설명

주어진 시작 단어(begin)에서 목표 단어(target)로, 한 번에 한 글자씩만 바꿔가며 변환하는 최소 횟수를 구하는 문제입니다. 변환 과정에서 만들어지는 모든 단어는 주어진 단어 리스트(words)에 포함되어 있어야 합니다.

- 한 번에 한 글자만 변경할 수 있습니다.
- words에 포함된 단어로만 변환할 수 있습니다.
- begin에서 target으로 변환하는 가장 짧은 과정을 찾아야 합니다.

# 제한사항

- 각 단어의 길이는 1 이상 10 이하입니다.
- words에는 3개 이상 50개 이하의 단어가 들어있습니다.
- begin과 target은 같지 않습니다.
- 모든 단어는 알파벳 소문자로만 이루어져 있습니다.
- words에는 begin이 들어있지 않습니다.
- 변환할 수 없는 경우 0을 반환합니다.

# 입출력 예

```
입력:
begin = "hit"
target = "cog"
words = ["hot", "dot", "dog", "lot", "log", "cog"]

출력: 4
```

# 풀이 방법

이 문제는 BFS(너비 우선 탐색)를 활용하여 해결할 수 있습니다. 각 단계에서 한 글자만 다른 단어로 변환하며, 변환된 단어가 words에 있고 아직 방문하지 않았다면 큐에 추가합니다. target에 도달하면 그때의 변환 횟수를 반환합니다.

# 코드

```python
from collections import deque

def solution(begin, target, words):
    visited = [begin]
    queue = deque([[begin,0]])
    while queue:
        vertex,count = queue.popleft()

        if vertex == target:
            return count
        for i in range(len(vertex)):
            for j in range(26):
                next_char = chr(ord('a') + j)
                if next_char == vertex[i]:
                    continue
                new_word = vertex[:i] + next_char + vertex[i+1:]
                if new_word in words and new_word not in visited:
                    visited.append(new_word)
                    queue.append([new_word,count + 1])

print(solution("hit","cog",["hot", "dot", "dog", "lot", "log", "cog"]))
```

# 코드 설명

1. `visited` 리스트에 이미 방문한 단어를 저장합니다.
2. 큐에는 [현재 단어, 변환 횟수] 형태로 저장합니다.
3. 큐에서 단어를 꺼내 target과 같으면 변환 횟수를 반환합니다.
4. 각 위치의 알파벳을 a~z로 바꿔가며 새로운 단어를 만듭니다.
5. 새 단어가 words에 있고 아직 방문하지 않았다면 큐에 추가합니다.
6. 큐가 빌 때까지 target에 도달하지 못하면 0을 반환합니다.

# 시간/공간 복잡도

- 시간 복잡도: O(N * L * 26)
  - N: words의 길이 (최대 50)
  - L: 단어의 길이 (최대 10)
  - 각 단어의 각 위치마다 26개의 알파벳을 시도
- 공간 복잡도: O(N)
  - 방문 리스트와 큐에 저장되는 단어의 수

BFS를 통해 모든 가능한 변환 경로를 탐색하므로, 최단 변환 횟수를 효율적으로 구할 수 있습니다. 