---
emoji: 🧶
title: '[프로그래머스] 징검다리'
date: '2023-02-15 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 징검다리](https://school.programmers.co.kr/learn/courses/30/lessons/43236)


<br/>

## 2. 핵심 아이디어

`이분탐색`

<br/>

## 3. 코드

```swift
import Foundation

func deleteRocks(start: Int, end: Int, rocks: [Int], n: Int) -> Bool {
    let mid = (start + end) / 2
    var count = 0
    var prevRock = 0
    for rock in rocks {
        if rock - prevRock < mid {
            count += 1
            if count > n {
                break
            }
        } else {
            prevRock = rock
        }
    }
    return count <= n
}

func solution(_ distance:Int, _ rocks:[Int], _ n:Int) -> Int {
    let rocks = rocks.sorted(by: <)
    var start = 0
    var end = distance
    var answer = 0
    while start <= end {
        let mid = (start + end) / 2
        // 삭제된 바위가 n과 같거나 적을 때
        if deleteRocks(start: start, end: end, rocks: rocks, n: n) {
            answer = mid
            start = mid + 1
        }
        // 삭제된 바위가 n보다 더 많을 때
        else {
            end = mid - 1
        }
    }
    return answer
}

```

<br/>

## 4. 풀이 과정

[입국심사](https://gobanest.com/algorithm/programmers/입국심사/) 문제와 동일하게 이분탐색 문제이다.

다만 조금 더 응용해서 이분탐색을 사용해야했다.

<br/>

이분탐색을 할 때 탐색할 값과 조건을 정하는게 가장 중요한데, 징검다리에서는 `두개의 돌 사이의 거리`와 `제거한 바위의 갯수`를 사용해야했다.

두개의 돌 사이의 거리를 탐색값으로 정해서 해당 값보다 작다면 바위를 제거한다고 생각을 하고 시뮬레이션을 하는 아이디어였다.

이러한 아이디어를 떠올리는게 가장 키포인트 였는데, 나는 떠올리지 못해서 검색을 통해 알게되었다.

[이곳](https://fomaios.tistory.com/entry/Swift-프로그래머스-징검다리-with-쉬운-풀이-포함)에서 자세하고 쉽게 풀이를 해주셨다.

<br/>

두개의 이분탐색 문제를 보니 이제 중요한 부분이 뭔지알것 같다.

중요한것은 `탐색값이 될만한 것을 찾는 것`과 `가정`을 하는 것이다.

<br/>

그런데 그런 아이디어를 떠올리고 곧바로 코드로 풀어내려면 좀 문제를 많이 접하는게 중요할듯하다.

연습을 통한 경험의 필요성을 다시금 느낀다..!

<br/>


```toc

```
