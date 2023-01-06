---
emoji: 🧶
title: '[프로그래머스] 이중우선순위큐'
date: '2023-01-06 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 이중우선순위큐](https://school.programmers.co.kr/learn/courses/30/lessons/42628)


<br/>

## 2. 핵심 아이디어

`heap` `정렬`

<br/>

## 3. 코드

[swift]
```swift
import Foundation


func solution(_ operations:[String]) -> [Int] {
    var maxHeap = Heap { (lhs: Int, rhs:Int) in
        return lhs > rhs
    }
    var minHeap = Heap { (lhs: Int, rhs:Int) in
        return lhs < rhs
    }
    
    for operation in operations {
        let trimedOperation = operation.components(separatedBy: " ")
        let command = trimedOperation[0]
        let paramater = trimedOperation[1]
        switch command {
        case "I":
            maxHeap.insert(Int(paramater)!)
            minHeap.insert(Int(paramater)!)
        case "D":
            if paramater == "1" {
                guard let element = maxHeap.remove() else { break }
                if let index = minHeap.index(of: element, startingAt: 0) {
                    minHeap.remove(at: index)
                }
            }
            if paramater == "-1" {
                guard let element = minHeap.remove() else { break }
                if let index = maxHeap.index(of: element, startingAt: 0) {
                    maxHeap.remove(at: index)
                }
            }
        default: break
        }
    }
    if minHeap.isEmpty && maxHeap.isEmpty {
        return [0,0]
    }
    return [maxHeap.remove()!,minHeap.remove()!]
}

```

<br/>

## 4. 풀이 과정

문제에서 삽입, 최댓값 삭제, 최솟값 삭제 세개의 연산을 반복하기 떄문에, 최소 최대 `heap` 두개를 만들어 각각 사용을 하면 문제가 풀릴 것 같아서 그대로 구현하였다.

다만 효용이 떨어져 시간초과날 것을 예상했지만 일단 구현 후 개선점을 찾으려 했었는데, 시간초는 문제에서 테스팅을 하지 않았고, 그래서 쉽게 통과를 한 것 같다.

<br/>

## 5. 다른 사람의 코드

[swift]
```swift
import Foundation

func solution(_ operations:[String]) -> [Int] {
    var items:[Int] = []


        for i in 0..<operations.count {

            let ii = operations[i].components(separatedBy: " ")

            if String(ii[0]) == "D" {
                if items.count > 0 {
                    items.remove(at: (Int(ii[1])! > 0 ? (items.count - 1) : 0))
                }

            }else if String(ii[0]) == "I"{
                items.append(Int(ii[1])!)
                items = items.sorted(by: {$0 < $1})
            }

        }

        items = items.sorted(by: {$0 < $1})

        var i:[Int] = [0,0]
        if items.count > 0 {
            i.removeAll()

            i.append(items.last!)
            i.append(items.first!)
        }
        return i
}

```

프로그래머스의 다른 사람의 코드를 봤는데, 그냥 배열을 사용해서 푼게 많았다.

테스트케이스 테스팅을 돌려보니 `heap` 과 큰 차이는 나지 않았다. (미세하게 heap 이 더 빨랐다) 


<br/>


```toc

```
