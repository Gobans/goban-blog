---
emoji: 🧶
title: '[프로그래머스] H-Index'
date: '2023-01-08 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)


<br/>

## 2. 핵심 아이디어

`정렬`

<br/>

## 3. 코드

[swift]
```swift
import Foundation

func solution(_ citations:[Int]) -> Int {
    var hIndex = 0
    guard let maxCitation = citations.max(), maxCitation != 0 else {
        return 0
    }
    let sortedCitations = citations.sorted(by: >)
    for index in 1...maxCitation {
        var citationCount = 0
        for citation in sortedCitations {
            if citation >= index {
                citationCount += 1
            } else {
                break
            }
        }
        if citationCount >= index {
            hIndex = index
        } else {
            break
        }
    }
    return hIndex
}

```

<br/>

## 4. 풀이 과정

문제를 보고 다음과 같이 생각하였다.

- h 번이상 인용, h 편 이상 인용
    - -> 인용된 횟수와 인용된 갯수가 함께 h 이상이여야함.
- h 번 이하 인용 
    - -> H-Index 의 최대를 구해야하고, H-Index 가 인용횟수 범위를 이상, 이하 모두 포함하기 떄문에 별로 상관 없음

때문에 'h 번이상 인용, h 편 이상 인용' 이 조건에 맞는지 검증을 해야하기 떄문에 다음과 같이 코드의 플로우를 생각했다.

1. max citation 만큼 반복
2. 1부터 검증
3. 내림차순으로 정렬된 citations 에서 h 번 이상 인용된 논문이 몇개인지 확인.
4. 불만족 시 현재까지의 최대 h-index return

그대로 구현하니까 통과할뻔 했는데, `(signal: illegal instruction (core dumped))`
오류가 발생했다.

크래쉬가 날곳이 어딘지 생각해보니 maxCitation을 구하고, 1부터 반복문을 실행하는 구간이였다. 

maxCitation이 0일 경우 `Fatal error: Range requires lowerBound <= upperBound` 오류가 떴다.

떄문에 guard let 으로 조건을 추가하여 예외 케이스를 처리해줬다.

<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ citations:[Int]) -> Int {
    let citations = citations.sorted() { $0 > $1 }
    var result = 0

    for i in 0..<citations.count {
        if i + 1 <= citations[i] {
            result = i + 1
        } else {
            break
        }
    }

    return result
}

```

인용갯수와 인용횟수를 이런식으로 간단하게 함께 확인을 할수가 있었다.

이런 풀이가 바로 떠오를려면 어떻게 해야할까?

부단히 연습을 해야겠다..

<br/>


```toc

```
