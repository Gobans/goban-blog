---
emoji: 🧶
title: '[프로그래머스] 타겟 넘버'
date: '2023-02-06 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 타겟 넘버](https://school.programmers.co.kr/learn/courses/30/lessons/43165)


<br/>

## 2. 핵심 아이디어

`DFS` `BFS`

<br/>

## 3. 코드

[DFS]
```swift
import Foundation

func solution(_ numbers:[Int], _ target:Int) -> Int {
    let n = numbers.count
    var answer = 0

    func DFS(num: Int, index: Int) {
        if index == n {
            if num == target {
                answer += 1
            }
            return
        }
        DFS(num: num + numbers[index], index: index + 1)
        DFS(num: num - numbers[index], index: index + 1)
    }
    DFS(num: 0, index: 0)

    return answer
}
```

<br/>

[BFS]

```swift
import Foundation

func solution(_ numbers:[Int], _ target:Int) -> Int {
    var queue = EffectiveQueue<(Int, Int)>()
    queue.enqueue((0,0))
    let n = numbers.count
    var answer = 0
    while !queue.isEmpty {
        let element = queue.dequeue()!
        let num = element.0
        let index = element.1
        if index == n {
            if num == target {
                answer += 1
            }
            continue
        }
        queue.enqueue((num + numbers[index], index + 1))
        queue.enqueue((num - numbers[index], index + 1))
    }
    return answer
}
```

<br/>

[EffectiveQueue](https://github.com/Gobans/Swift-Algorithm/blob/main/SwiftAlgorithm/DataStrcutre/EffectiveQueue.swift) 자료구조

이 코드는 [이곳](https://one10004.tistory.com/247) 에서 작성한 코드입니다.

<br/>

## 4. 풀이 과정

간단하게 숫자들을 DFS/BFS 로 탐색하는 코드를 구현하면 되는 문제였다.

나는 BFS로 문제를 먼저 풀었는데 시간초과가 났다..!

<br/>

그리고 나서 DFS로 풀었는데, 시간초과가 나지 않았다.

이상해서 왜인지 찾아봤는데..

<br/>

```swift
removeFirst()
```

이 명령어 때문이였다.

!!!!

나만 이 명령어 O(1)인줄 알고 있었나..

-> 실험
stack을 하나 더 써서 queue가 empty 될때마다 교체하는 방법

다른 queue 사용 (https://nitinagam17.medium.com/data-structure-in-swift-queue-part-5-985601071606)

<br/>

## 5. 다른 사람의 코드

```swift

```

<br/>


```toc

```
