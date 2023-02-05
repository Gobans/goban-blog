---
emoji: 🧶
title: '[프로그래머스] 단어 변환'
date: '2023-02-04 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 단어 변환](https://school.programmers.co.kr/learn/courses/30/lessons/43163?language=swift)


<br/>

## 2. 핵심 아이디어

`DFS` `BFS`

<br/>

## 3. 코드

[DFS]
```swift
import Foundation

func solution(_ begin:String, _ target:String, _ words:[String]) -> Int {
    var isVisited: [Bool] = Array(repeating: false, count: words.count)
    var minCnt = 52
    func DFS(nowWord: String, cnt: Int, isVisited: [Bool]) {
        if nowWord == target {
            if cnt < minCnt {
                minCnt = cnt
            }
            return
        }
        if cnt >= minCnt {
            return
        }
        var cVisited = isVisited
        for i in 0..<words.count {
            if !isVisited[i] && isPossibleChange(lhs: nowWord, rhs: words[i]) {
                cVisited[i] = true
                DFS(nowWord: words[i], cnt: cnt + 1, isVisited: cVisited)
                cVisited[i] = false
            }
        }
    }
    DFS(nowWord: begin, cnt: 0, isVisited: isVisited)
    return minCnt == 52 ? 0 : minCnt
}

func isPossibleChange(lhs: String, rhs: String) -> Bool {
    var lhs = Array(lhs)
    var rhs = Array(rhs)
    var cnt = 0
    for i in 0..<lhs.count {
        if lhs[i] != rhs[i] {
            cnt += 1
            if cnt == 2 {
                return false
            }
        }
    }
    return true
}

```

[BFS]

```swift
struct WordInfo {
    let word: String
    let cnt: Int
    let isVisited: [Bool]
}

func solution(_ begin:String, _ target:String, _ words:[String]) -> Int {
    let isVisited: [Bool] = Array(repeating: false, count: words.count)
    var minCnt = 52
    var queue: [WordInfo] = []
    queue.append(WordInfo(word: begin, cnt: 0, isVisited: isVisited))
    while !queue.isEmpty {
        let wordInfo = queue.removeFirst()
        if wordInfo.word == target {
            if wordInfo.cnt < minCnt {
                minCnt = wordInfo.cnt
            }
            continue
        }
        if wordInfo.cnt >= minCnt {
            continue
        }
        var cVisited = wordInfo.isVisited
        for i in 0..<words.count {
            if !wordInfo.isVisited[i] && isPossibleChange(lhs: wordInfo.word, rhs: words[i]) {
                cVisited[i] = true
                queue.append(WordInfo(word: words[i], cnt: wordInfo.cnt + 1, isVisited: cVisited))
                cVisited[i] = false
            }
        }
    }
    return minCnt == 52 ? 0 : minCnt
}

func isPossibleChange(lhs: String, rhs: String) -> Bool {
    print("lhs: \(lhs) -> rhs: \(rhs)")
    let lhs = Array(lhs)
    let rhs = Array(rhs)
    var cnt = 0
    for i in 0..<lhs.count {
        if lhs[i] != rhs[i] {
            cnt += 1
            if cnt == 2 {
                return false
            }
        }
    }
    return true
}

```

<br/>

## 4. 풀이 과정

다음과 같이 풀면 되겠다고 생각했다.

    바꿀 수 있는 단어 (바꾸는 cost 가 1 이하인 알파벳)를 찾은 후 BFS or DFS

우선 DFS로의 구현이 쉬울 것 같아서 먼저 구현했고, 다음에 BFS로 구현했다.

성능은 큰 차이 없을 것 같다. 

최소 cnt를 조건으로 비교하는 조건은 두 방법 모두 있기 때문에, 결국 최소 cnt 를 찾기위해서 탐색하는 양은 비슷하다.

<br/>


```toc

```
