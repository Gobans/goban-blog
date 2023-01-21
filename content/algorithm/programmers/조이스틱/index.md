---
emoji: 🧶
title: '[프로그래머스] 조이스틱'
date: '2023-01-21 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit Hello 조이스틱](https://school.programmers.co.kr/learn/courses/30/lessons/42860)


<br/>

## 2. 핵심 아이디어

`그리디`

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ name:String) -> Int {
    let targetArray: [String] = name.map{ String($0) }
    var currentAray: [String] = Array(repeating: "A", count: targetArray.count)
    var minMove = targetArray.count - 1
    var answer = 0

    for index in 0..<targetArray.count {
        if currentAray[index] != targetArray[index] {
            var currentChar = currentAray[index]
            var updownCnt = 0
            while currentChar != targetArray[index] {
                currentChar = String(UnicodeScalar(UnicodeScalar(currentChar)!.value + 1)!)
                updownCnt += 1
            }
            currentAray[index] = currentChar
            answer += min(updownCnt, 26 - updownCnt)
        }

        var endIdx = index + 1
        while endIdx < targetArray.count && targetArray[endIdx] == "A" {
            endIdx += 1
        }

        let moveFront = index * 2 + (currentAray.count - endIdx)
        let moveBack = (currentAray.count - endIdx) * 2 + index

        minMove = min(minMove, moveFront)
        minMove = min(minMove, moveBack)
    }
    return answer + minMove
}


```

<br/>

## 4. 풀이 과정

쉬운 문제인줄 알았는데.. 생각보다 훨씬 어려웠다.

처음의 생각은 이랬다.

    1. 조이스틱의 이동을 코드로 작성.
    -> 조이스틱의 커서 이동 시 "A" 라면 카운트를 무시함
    2. A 에서 아래로 이동 or 위로 이동, 더 짧은 것을 문자 변경 횟수로 사용함.

여기서 조이스틱의 이동을 코드로 작성하는 과정에서, 좌측 우측을 반복문으로 탐색하고 최소 값을 가지는 길을 선택하면 된다고 생각했다..

그런데 알고보니 `"A" 가 중간에 무수히 많은 경우` 지금까지 왔던 길을 다시 되돌아 가는 것이 빠른 길일때가 있었다.

<br/>

이 경우를 어떻게 처리할지 감이 안잡혀 [이곳](https://inuplace.tistory.com/1091)을 참고하여 코드를 작성하였다.

핵심은 index 0 인 자리에서 앞 뒤로 갔을 때를 계산하여, 가장 빠른 경우 모두 찾아주는 것이였다.

어렵다 어려워..!

<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ name:String) -> Int {
    let name = Array(name)
    let aValue = Int(Character("A").asciiValue!)
    let zValue = Int(Character("Z").asciiValue!)

    var updown = 0    
    var leftright = name.count-1  

    for startIdx in 0..<name.count {
   
        updown += min(Int(name[startIdx].asciiValue!) - aValue, zValue - Int(name[startIdx].asciiValue!) + 1)

        var endIdx = startIdx + 1
        while endIdx < name.count && name[endIdx] == "A" {
            endIdx += 1
        }

        let moveFront = startIdx * 2 + (name.count - endIdx)
        let moveBack = (name.count - endIdx) * 2 + startIdx

        leftright = min(leftright, moveFront)
        leftright = min(leftright, moveBack)
    }

    return updown + leftright
}

```

내가 참고한 코드인데, 아주 깔끔하다..!👍

<br/>


```toc

```
