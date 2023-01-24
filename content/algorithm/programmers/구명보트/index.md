---
emoji: 🧶
title: '[프로그래머스] 구명보트'
date: '2023-01-24 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 구명보트](https://school.programmers.co.kr/learn/courses/30/lessons/42885)


<br/>

## 2. 핵심 아이디어

`그리디`

<br/>

## 3. 코드

[swift]
```swift
import Foundation

func solution(_ people:[Int], _ limit:Int) -> Int {
    var answer: Int = 0
    var people = people.sorted(by: >)
    while people.count > 1 {
        if people[0] + people[people.endIndex - 1] <= limit {
            people.removeFirst()
            people.removeLast()
        } else {
            people.removeFirst()
        }
        answer += 1
    }
    if !people.isEmpty {
        answer += 1
    }
    return answer
}
```
<br/>

[python]
```python
 from collections import deque

 def solution(people, limit):
     answer = 0
     people.sort(reverse=True)
     people = deque(people)
     while len(people) > 1:
         if people[0] + people[-1] <= limit:
             people.popleft()
             people.pop()
         else:
             people.popleft()
         answer += 1

     if people:
         answer += 1
     return answer
```

<br/>

## 4. 풀이 과정

처음 문제를 보고 다음과 같이 생각했다.

    - 최대한 무게를 채워서 탑승하자
        - 내림차순 정렬, dequeue 을 사용
    - 반복적으로 처음과 끝을 비교함, limit 을 초과안하면 첫번째, 마지막 원소 제거. (무거운 쪽, 가벼운 쪽)
    - limit 을 초과한다면, 첫번째 원소 (무거운쪽) 제거.

여기까지 잘 생각을 하고, 구현을 하는데...

문득 다른 생각이 떠올랐다.

**가장 큰 몸무게와 합하여 limit과 가장 가까운 몸무게를 찾아야하지 않나?**

<br/>

예컨데 이런 것이다.

limit이 100kg이고, 사람들이 80kg, 20kg, 10kg 이 있으면, **80kg 과 20kg 의 사람을 쌍**으로 보트에 타야 가장 많이 보트를 태울 수 있지 않는가?

<br/>

그러나 이는 틀린 것이다.

<br/> 

    최대 몸무게 -> 감소
    최소 몸무게 -> 증가

서로 증감이 반대이기 때문에, 항상 최대의 짝을 지을 수 있는 경우는 최대와 최소 몸무게가 짝이 되었을 때이다.

앞선 예제에서 80 + 20 으로 짝을 짓는 것은 20 보다 작은 몸무게를 제치고 선택을 하는 것인데, 이렇게 선택을 해도 큰 의미가 없다. 

왜냐하면 **그 다음 최대 몸무게는 같거나 감소** 하고 앞서 제쳐져 남아있는 최소 몸무게는 **이미 그전 최대 몸무게와 조합하여 limit 이내 라는 것이 보장 되어있기 떄문이다.**

그러므로 작은 몸무게를 뛰어넘어 조합할 수 있는 limit을 최대한으로 맞춘다 한들, 뛰어넘은 몸무게들은 어차피 조합이 될것이 보장되어있기 때문에 필요가 없다.

<br/>
<br/>

나만 이게 좀 많이 헷갈린건지 모르겠는데... 어느순간 이런 생각의 샛길로 빠져서 문제 풀이의 시간이 오래걸렸다.. 

<br/>

<br/>

## 5. 다른 사람의 코드

```python
def solution(people, limit) :
    answer = 0
    people.sort()

    a = 0
    b = len(people) - 1
    while a < b :
        if people[b] + people[a] <= limit :
            a += 1
            answer += 1
        b -= 1
    return len(people) - answer
```

인덱스를 start, end 시점으로 투 포인터로 접근하여 문제를 풀었는데, 참고할만한 것 같다.

<br/>


```toc

```
