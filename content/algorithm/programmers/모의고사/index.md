---
emoji: 🧶
title: '[프로그래머스] 모의고사'
date: '2023-01-10 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 모의고사](https://school.programmers.co.kr/learn/courses/30/lessons/42840)


<br/>

## 2. 핵심 아이디어

`완전탐색`

<br/>

## 3. 코드

```swift

func solution(_ answers:[Int]) -> [Int] {
    let supoja:[[Int]] = [[1,2,3,4,5],[2,1,2,3,2,4,2,5],[3,3,1,1,2,2,4,4,5,5]]
    var supojaCorrects: [Int] = []
    var answer: [Int] = []
    for supojaIndex in 0...2 {
        var pickIndex = 0
        var correctCnt = 0
        for answer in answers {
            if answer == supoja[supojaIndex][pickIndex] {
                correctCnt += 1
            }
            pickIndex += 1
            if pickIndex == supoja[supojaIndex].count {
                pickIndex = 0
            }
        }
        supojaCorrects.append(correctCnt)
    }
    let maxCorrect = supojaCorrects.max()!
    for supojaNum in 0...2 {
        if supojaCorrects[supojaNum] == maxCorrect {
            answer.append(supojaNum + 1)
        }
    }
    
    return answer
}

```

<br/>

## 4. 풀이 과정

문제를 보고 다음과 같이 생각하였다.

    - 가장 많은 문제를 맞힌 사람
    -> 찍는 방식을 answer에 대입했을떄, 가장 많이 맞힌사람.

이를 바탕으로 찍는 방식을 answer에 대입하여 정답의 갯수를 확인하는 코드의 플로우는 다음과 같이 구상하였다.
    
    1. 수포자 1,2,3 의 찍는 방식을 배열로 저장한다.
    2. 3번 반복, 수포자의 찍는 방식을 index 로 이동하면서 정답과 일치하는지 확인한다.
    3. 맞춘 갯수를 1,2,3 각각 저장하고, 최대로 맞춘 사람을 구한다.

<br/>

이대로 구현하여 쉽게 문제를 맞혔는데, 다른 사람의 코드를 보고나니 나의 코드에 아쉬운 부분이 있었다.
    
    1. 전체적으로 *3 번의 반복이 더 들어간다. (supojaIndex)
    2. 최댓값을 배열로 만드는 코드가 우아하지 못하다.

아래에 다른 사람의 코드를 보면 아주 깔끔하고 우아하게 문제를 해결했다.



<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ answers:[Int]) -> [Int] {
    let answer = (
        a: [1, 2, 3, 4, 5], // index % 5
        b: [2, 1, 2, 3, 2, 4, 2, 5], // index % 8
        c: [3, 3, 1, 1, 2, 2, 4, 4, 5, 5] // index % 10
    )
    var point = [1:0, 2:0, 3:0]

    for (i, v) in answers.enumerated() {
        if v == answer.a[i % 5] { point[1] = point[1]! + 1 }
        if v == answer.b[i % 8] { point[2] = point[2]! + 1 }
        if v == answer.c[i % 10] { point[3] = point[3]! + 1 }
    }

    return point.sorted{ $0.key < $1.key }.filter{ $0.value == point.values.max() }.map{ $0.key }
}

```

나의 코드와 결정적으로 다른점은 아래와 같았다.

    1. dictionary 를 사용함.
    2. 정답을 찍는 배열의 index 를 '%' 로 반복해서 사용하도록 만듦.
    3. 정답 배열을 고차함수로 우아하게 만듦.

보고 배울점이 많은 코드였다.

특히 dictionary를 

```swift
let answer = (
    a: [1, 2, 3, 4, 5],
    b: [2, 1, 2, 3, 2, 4, 2, 5], 
    c: [3, 3, 1, 1, 2, 2, 4, 4, 5, 5] 
)
```

이렇게 선언해서 `answer.a` `answer.b` 과 같이 접근하는 방식이 생소해서 뭔가 싶었는데,  

이건 '튜플' 이였다. 

이렇게 데이터 그룹을 임시로 이름을 붙여서 사용하는게 정말 괜찮은 방식이라 느꼈다. 직관성 up.


<br/>


```toc

```
