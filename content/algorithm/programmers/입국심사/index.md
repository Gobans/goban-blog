---
emoji: 🧶
title: '[프로그래머스] 입국심사'
date: '2023-02-14 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 입국심사](https://school.programmers.co.kr/learn/courses/30/lessons/43238?language=swift)


<br/>

## 2. 핵심 아이디어

`이분탐색`

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ n:Int, _ times:[Int]) -> Int64 {
    var low: Int64 = 1
    var high: Int64 = Int64(times.max()!*n)
    while low < high {
        let mid: Int64 = (low + high) / 2
        var total = 0
        for t in times {
            total += Int(mid) / t
            if total >= n {
                break
            }
        }
        if total >= n {
            high = mid
        } else {
            low = mid + 1
        }
    }
    return low
}

```

<br/>

## 4. 풀이 과정

우선 이분탐색을 어떻게 해야할지 감이 안잡혀서 검색을 통해 풀이를 봤다.

[이곳](https://fomaios.tistory.com/entry/Swift-Algorithm-프로그래머스-입국심사)에서 잘 정리해주셨는데,

핵심은 전체 소요시간을 `가정`하여 탐색하는 것이였다.

<br/>

코드에 있듯이 최소, 최대로 심사 시간을 정해놓고 그 중간의 시간부터 탐색을 시작한다.

<br/>

```swift
for t in times {
    total += Int(mid) / t
    if total >= n {
        break
    }
}
```

<br/>

그리고 현재 탐색하는 총 심사시간(mid)을 각각의 심사시간(t)로 나눠서 심사할 수 있는 인원의 수(total)를 판단한다.

<br/>

```swift
if total >= n {
    high = mid
} else {
    low = mid + 1
}
```
<br/>

심사할 수 있는 인원의 수가 n 을 넘으면 탐색하는 총 심사시간을 줄인다.

반대로 n을 넘지 못한다면 총 심사시간을 늘인다.

<br/>
<br/>

이분탐색을 어렴풋이 알고있어서 문제를 못풀기도 했고,

`탐색하는 총 심사시간(mid)을 각각의 심사시간(t)로 나눠서 심사할 수 있는 인원의 수(total)를 판단` 하는 아이디어가 떠오르지 않았다.

이렇게 쓰는거구나 대충 알았으니 이제 비슷한 문제는 잘풀 수 있지않을까 생각한다.

```toc

```
