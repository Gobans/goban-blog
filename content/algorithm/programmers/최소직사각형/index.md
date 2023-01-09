---
emoji: 🧶
title: '[프로그래머스] 최소직사각형'
date: '2023-01-09 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit ](https://school.programmers.co.kr/learn/courses/30/lessons/86491)


<br/>

## 2. 핵심 아이디어

`완전탐색` `정렬`

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ sizes:[[Int]]) -> Int {
    let sortedSizes = sizes.sorted { lhs, rhs in
        return lhs[0]*lhs[1] > rhs[0]*rhs[1]
    }
    let maxSize = sizes.max { lhs, rhs in
        return lhs.max()! <= rhs.max()!
    }!
    let someMax = maxSize.max()!
    var someSize = 0
    while true {
        var isPossible = true
        for size in sortedSizes {
            if (someSize < size[0] && someSize < size[1] ) {
                isPossible = false
                break
            }
        }
        if isPossible {
            return someMax*someSize
        }
        someSize += 1
    }
    return someSize
}
```

<br/>

## 4. 풀이 과정

문제를 이해하는데 시간을 많이 썼고, 구현 방법이 곧바로 떠오르지 않아서 또 헤맸던 문제이다.

현재 답으로 제출한 코드도 문제를 조금 오해해서 작성한 코드인데, 나는 가로 세로 중 최솟값이 지갑들(sizes) 말고 다른 숫자에서 나올 수도 있을 것이라 생각하고 코드를 작성했다.

이는 틀린 것으로, 최대값 최소값 모두 sizes 안에 있는 지갑의 크기에서 산출된다. (당연한건데 왜 이상하게 생각했을까..!)

<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ sizes:[[Int]]) -> Int {
    var maxNum = 0
    var minNum = 0

    for size in sizes {
        maxNum = max(maxNum, size.max()!)
        minNum = max(minNum, size.min()!)
    }
    return maxNum * minNum
}

```

깔끔하게 size 중에서 최대값 최소값을 찾고있다.

살짝 재귀적 느낌의 변수가 특이하다. 👍

<br/>


```toc

```
