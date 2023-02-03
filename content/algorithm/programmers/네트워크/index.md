---
emoji: 🧶
title: '[프로그래머스] 네트워크'
date: '2023-02-03 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 네트워크 ](https://school.programmers.co.kr/learn/courses/30/lessons/43162)


<br/>

## 2. 핵심 아이디어

`DFS` `BFS`

<br/>

## 3. 코드

[DFS]
```swift
func solution(_ n:Int, _ computers:[[Int]]) -> Int {
    var connectMap: [[Int]] = Array(repeating: Array(repeating: 0, count: n), count: n)
    var isVisited: [Bool] = Array(repeating: false, count: n)
    var cnt = 0
    for i in 0..<n {
        for j in 0..<n {
            if computers[i][j] == 1 {
                connectMap[i][j] = 1
            }
        }
    }
    func DFS(node: Int) {
        for idx in 0..<n  {
            if !isVisited[idx] && connectMap[node][idx] == 1 {
                isVisited[idx] = true
                DFS(node: idx)
            }
        }
    }
    for i in 0..<n {
        if !isVisited[i] {
            isVisited[i] = true
            cnt += 1
            DFS(node: i)
        }
    }
    return cnt
}
```

[BFS]
```swift
func solution(_ n:Int, _ computers:[[Int]]) -> Int {
    var connectMap: [[Int]] = Array(repeating: Array(repeating: 0, count: n), count: n)
    var isVisited: [Bool] = Array(repeating: false, count: n)
    var cnt = 0
    for i in 0..<n {
        for j in 0..<n {
            if computers[i][j] == 1 {
                connectMap[i][j] = 1
            }
        }
    }
    var queue: [Int] = []
    for i in 0..<n {
        if !isVisited[i] {
            queue.append(i)
            cnt += 1
            isVisited[i] = true
            while !queue.isEmpty {
                let node = queue.removeFirst()
                for k in 0..<n {
                    if !isVisited[k] && connectMap[node][k] == 1 {
                        isVisited[k] = true
                        queue.append(k)
                    }
                }
            }
        }
    }
    return cnt
}
```

<br/>

## 4. 풀이 과정

문제를 보고 다음과 같이 생각했다.

    1. 2차원 배열로, 해당 노드에 연결된 노드를 1로 표현함
    2. 노드의 방문을 나타내는 배열을 하나 만들어 순회.
    3. 반복문으로 노드를 방문했는지 DFS/BFS 로 확인.
    4. 방문한 노드를 업데이트.

이 문제를 푼 기억이 희미하게 있었다.. 아마 예전에 한번 풀었어서 쉽게 풀이가 떠오른 것 같다.

<br/>

해당 플로우로 코드를 작성하였다! 

처음에는 DFS로 구현을 하였고 BFS로도 구현 해보았다.


<br/>


```toc

```
