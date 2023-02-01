---
emoji: 🧶
title: '[프로그래머스] 도둑질'
date: '2023-02-01 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 도둑질](https://school.programmers.co.kr/learn/courses/30/lessons/42897?language=python3)


<br/>

## 2. 핵심 아이디어

`DP`

<br/>

## 3. 코드

[swift]
```swift
import Foundation

func solution(_ money: [Int]) -> Int {
    var dp1 = Array(repeating: 0, count: money.count)
    var dp2 = Array(repeating: 0, count: money.count)
    dp1[0] = money[0]
    for i in 1..<money.count - 1 {
        guard i-2 >= 0 else {
            dp1[i] = max(dp1[i-1], dp1[dp1.endIndex - 1] + money[i])
            continue
        }
        dp1[i] = max(dp1[i-1], dp1[i-2] + money[i])
    }
    for i in 1..<money.count {
        guard i-2 >= 0 else {
            dp2[i] = max(dp2[i-1], dp2[dp2.endIndex - 1] + money[i])
            continue
        }
        dp2[i] = max(dp2[i-1], dp2[i-2] + money[i])
    }
    return max(dp1[dp1.endIndex - 2], dp2[dp2.endIndex - 1])
}

```

<br/>

## 4. 풀이 과정

우선 DFS 로 생각을 하면, 모든 경우를 탐색하는 경우를 찾기 위해서는 **찾거나** **찾지 않거나** 두가지의 경우를 생각하면 된다.

그런데 한집을 털면 인접한 두 집은 털 수 없으니 다음과 같이 생각하고 점화식을 세울 수 있다.

<br/>

    인접한 두 집을 털면 방범장치 울림
    -> 집 하나를 털면 1 + n 의 집을 털 수 있음.
    -> i, i-2 집과 i -1 의 집을 비교. money 가 더 많은 집을 선택

>  dp[i] = max(dp[i-1], dp[i-2] + money[i])

<br/>

뭔가 어디서 본듯한 점화식을 세우고 나서 보면, 집이 **원형** 으로 줄 세워져있다는 조건을 처리해줘야하는데, 

    1. 첫번째 집을 선택하는경우
    2. 마지막 집을 선택하는경우

이 두가지 조건은 양립할 수 없으므로, 두가지의 경우 모두 따로 점화식을 사용하여 값을 구해주면 된다.


<br/>


```toc

```
