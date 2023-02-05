---
emoji: 🧶
title: '[프로그래머스] 여행경로'
date: '2023-02-05 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 여행경로](https://school.programmers.co.kr/learn/courses/30/lessons/43164)


<br/>

## 2. 핵심 아이디어

`DFS`

<br/>

## 3. 코드

```swift

import Foundation

func solution(_ tickets:[[String]]) -> [String] {
    var flightDict: [String: Int] = [:]
    var flightSet = Set<String>()
    for ticket in tickets {
        ticket.forEach { flight in
            flightSet.insert(flight)
        }
    }
    let sortedFlightSet = flightSet.sorted(by: <)
    var flightIndexCnt = 0
    for flight in sortedFlightSet {
        flightDict[flight] = flightIndexCnt
        flightIndexCnt += 1
    }
    var flightMap: [[Int]] = Array(repeating: Array(repeating: 0, count: sortedFlightSet.count), count: sortedFlightSet.count)
    for ticket in tickets {
        let departFlightIndex = flightDict[ticket[0]]!
        let arriveFlightIndex = flightDict[ticket[1]]!
        flightMap[departFlightIndex][arriveFlightIndex] += 1
    }
    let flightCnt = sortedFlightSet.count
    let depth = tickets.count
    var targetSearched = false
    var answerTravelCourse: [String] = []
    func DFS(departFlightIndex: Int, travelCourse: [String], usedTicketCnt: Int) {
        if usedTicketCnt == depth {
            targetSearched = true
            answerTravelCourse = travelCourse
            return
        }
        var travelCourse = travelCourse
        for i in 0..<flightCnt {
            if !targetSearched && flightMap[departFlightIndex][i] >= 1 {
                travelCourse.append(sortedFlightSet[i])
                flightMap[departFlightIndex][i] -= 1
                DFS(departFlightIndex: i, travelCourse: travelCourse, usedTicketCnt: usedTicketCnt + 1)
                flightMap[departFlightIndex][i] += 1
                travelCourse.removeLast()
            }
        }
    }
    DFS(departFlightIndex: flightDict["ICN"]!, travelCourse: ["ICN"], usedTicketCnt: 0)
    return answerTravelCourse
}
```

<br/>

## 4. 풀이 과정

탐색문제인데, 특이하게 알파벳 사전순으로 정렬된 경로를 출력해야한다.

다음과 같이 코드 플로우를 생각하였다.

    1. 항공권이 갈 수 있는 경로를 2차원 배열로 나타냄.
    2. 단어마다 딕셔너리에 저장하여, 인덱스를 해싱함. (set 으로 미리 중복 제거 후 정렬하고, 딕셔너리 생성)
    3. 항공권 경로대로 2차원 배열 채우기
    4. 채워진 항공권 경로대로 DFS 후, return

이렇게 풀었고, 테스트 케이스도 무난히 통과하길래 정답인줄 알았다.

그런데 **테스트 케이스 1번** 에서 실패했다. (2,3,4 모두 통과)

<br/>

이유가 뭔가 보니, 나는 항공권으로 갈 수 있는 경로를 나타낸 2차원 배열을 [[Bool]] 로 사용해서 썼는데, 이 때 경로가 같은 항공권이 여러개 중복으로 있을경우, 해당 상황을 나타내지 못해서 실패하는 것이였다.

그래서 해당 2차원 배열을 [[Int]]로 바꾸면 항공권이 중복으로 존재하는 경우 숫자로 표현이 가능하다.

이렇게 바꿔서 문제를 통과했다!

<br/>

`BFS` 는 문제의 상황과 맞지 않는 알고리즘이라 생각해서 따로 구현하지 않았다. 여행 순서를 이미 정렬로 어느정도 정했는데 BFS 로 탐색하여 루트를 찾는 것은 비효율적이다. 찾긴 찾겠지만.. 오래 걸릴 것이다.

<br/>


```toc

```
