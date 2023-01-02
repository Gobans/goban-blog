---
emoji: 🧶
title: '[프로그래머스] 더 맵게'
date: '2023-01-02 00:00:00'
author: 고반
tags: 알고리즘 프로그래머스
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 더 맵게](https://school.programmers.co.kr/learn/courses/30/lessons/42626)


<br/>

## 2. 핵심 아이디어

`힙`을 이용한 `우선 순위 큐`

<br/>

## 3. 코드

```python
import heapq

def solution(scoville, k):
    heap = []
    for num in scoville:
        heapq.heappush(heap, num)
    mix_cnt = 0
    while heap[0] < k:
        try:
            heapq.heappush(heap, heapq.heappop(heap) + (heapq.heappop(heap) * 2))
        except IndexError:
            return -1
        mix_cnt += 1
    return mix_cnt

```

[코드](https://itholic.github.io/kata-more-spicy/)

<br/>

## 4. 풀이 과정

```python
def solution(scoville, K):
    lowestScoville = min(scoville)
    is_made = False
    cnt = 0
    if lowestScoville >= K:
        return 0
    while len(scoville) >= 2:
        scoville.sort(reverse=True)
        newScoville = scoville.pop() + (scoville.pop() * 2)
        scoville.append(newScoville)
        cnt += 1
        if min(scoville) >= K:
            is_made = True
            break
    print(scoville)
    if is_made:
        return cnt
    return -1

```

플로우를 다음과 같이 생각했다.

    1. 스코빌 지수가 가장 낮은 두 개의 음식 조합
    2. 공식: newScoville = lowestScoville + (lowestSecondScoville * 2)
    3. 해당 동작을 모든 음식의 스코빌 지수가 K 이상이 될때까지 반복.
    4. -> 최소 반복 횟수를 리턴
    5. 만들 수 없는 경우 -1 리턴

힙을 이용한 `우선 순위 큐` 를 잘몰라서 시간초과 날 것을 예상했지만, `정렬` 을 사용해 일단 풀었다.

역시나 시간초과였고, 블로그를 참고하기로 했다.

[알고리즘](https://suyeon96.tistory.com/31)

보고 어떤 것인지 대충 이해를 했는데, 한번 Swift 로 heap 을 이용해 우선순위 큐를 구현해봐야겠다고 생각했다.

일단 파이썬은 다른사람의 코드를 적었는데, 해당 코드를 바탕으로 Swift 로 heap을 이용해 우선순위큐를 구현해보기로 했다.


```toc

```
