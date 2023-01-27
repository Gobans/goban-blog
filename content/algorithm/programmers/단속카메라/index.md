---
emoji: 🧶
title: '[프로그래머스] 단속카메라'
date: '2023-01-26 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 단속카메라](https://school.programmers.co.kr/learn/courses/30/lessons/42884)


<br/>

## 2. 핵심 아이디어

`그리디`

<br/>

## 3. 코드

```swift
import Foundation

func solution(routes:[[Int]]) -> Int {
    var answer = 0
    let routes = routes.sorted(by: { $0[1] < $1[1] })
    var camera = -30001
    for route in routes {
        if camera < route[0] {
            answer += 1
            camera = route[1]
        }
    }
    return answer
}
```

<br/>

## 4. 풀이 과정

일단.. 문제를 완전히 잘못 생각했다.

나는 이 문제가 정점 2개가 주어지고, 해당 정점들에 단속 카메라를 설치해나가는 것인줄 알았는데... 주어진 배열은 그게아니라 **구간** 을 나타낸 것이였다.

<br/>

그래서 원래 쓰던 풀이를 지우고, 다시 생각해봤는데 잘 풀리지가 않았다. 문제를 잘못 이해했다는 것에 멘탈이 나가버린걸까.. 머리가 안돌아갔다.

그래서 [이곳](https://wwlee94.github.io/category/algorithm/greedy/speed-enforcement-camera/) 을 참조하였고

아주 간결한 풀이에 넋이 나갔다.

<br/>

```python
def solution(routes):
    answer = 0
    routes.sort(key=lambda x:x[1])
    leng = len(routes)
    checked = [0] * leng

    for i in range(leng):
        if checked[i] == 0:
            camera = routes[i][1] # 진출 지점에 카메라를 갱신
            answer += 1
        for j in range(i+1, leng):
            if routes[j][0] <= camera <= routes[j][1] and checked[j] == 0:
                checked[j] = 1
    return answer
```

이 코드가 내가 생각했던 풀이와 가상 유사했는데,

그보다 간결한 풀이가 있었다...

<br/>

진출지점을 정렬하고, 진입지점 보다 작은지 확인하고 
1. count를 늘리고,
2. 카메라의 위치를 갱신한다

어떻게 이런 생각을 바로 떠올릴 수 있을까.

나는 노력하는 수 밖에 없는 것 같다.

<br/>

## 5. 다른 사람의 코드

```swift

```

<br/>


```toc

```
