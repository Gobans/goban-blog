---
emoji: 🧶
title: '[프로그래머스] 체육복'
date: '2023-01-19 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 체육복](https://school.programmers.co.kr/learn/courses/30/lessons/42862)


<br/>

## 2. 핵심 아이디어

`그리디`

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ n:Int, _ lost:[Int], _ reserve:[Int]) -> Int {
    var sortedLost = lost.sorted(by: < )
    var suitDict: [Int:Bool] = [:]
    var count = 0
    for suitNum in reserve {
        if sortedLost.contains(suitNum) {
            let removeIndex = sortedLost.firstIndex(of: suitNum)
            sortedLost.remove(at: removeIndex!)
            count += 1
            continue
        }
        suitDict[suitNum] = true
    }
    for lostNum in sortedLost {
        if let suit = suitDict[lostNum - 1], suit == true {
            suitDict[lostNum - 1] = false
            count += 1
            continue
        }
        if let suit = suitDict[lostNum + 1], suit == true {
            suitDict[lostNum + 1] = false
            count += 1
            continue
        }
    }
    let answer = n - lost.count + count
    return answer
}

```

<br/>

## 4. 풀이 과정

문제를 보고 다음과 같이 생각했다.

    앞 뒤 순서의 친구에게만 체육복을 빌려줄 수 있음.
    -> 앞의 친구가 우선 빌려주고, 안되면 뒤의 친구가 빌려주자.

이를 바탕으로 코드의 흐름을 짜봤다.
    
    1. lost: 오름차순으로 정렬.
    2. reserve: dict으로 저장.
    3. lost 하나씩 보고 앞,뒤에 체육복을 빌려줄 수 있는지 확인.
        - 빌려줄 수 있다면 빌려주고 dict 업데이트

<br/>

이렇게 해서 문제가 잘 풀릴줄 알았건만...!

계속 실패가 떠서 뭐가 잘못되었나 봤더니,

- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

<br/>
요런 조건이 있었다.

이걸 안보고 풀다가 실패가 계속 뜬것..!

그래서 dict 을 만드는 반복문에 코드를 추가하여, 잃어버린 체육복을 돌려줬다.

<br/>

> 문제 조건을 잘 보자!


<br/>


```toc

```
