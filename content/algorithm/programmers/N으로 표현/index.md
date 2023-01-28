---
emoji: 🧶
title: '[프로그래머스] N으로 표현'
date: '2023-01-28 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit N으로 표현](https://school.programmers.co.kr/learn/courses/30/lessons/42895)


<br/>

## 2. 핵심 아이디어

`DP` `DFS` `BruteForce`

<br/>

## 3. 코드

[DFS]
```swift
func dfs(_ N: Int, _ number: Int, _ depth: Int, _ temp: Int, _ answer: inout Int)  {
    if depth > 8 {
        return
    }

    if temp == number && (answer > depth || answer == -1) {
        answer = depth
        return
    }

    var calc = 0

    for index in 0 ..< 8 {
        calc = calc * 10 + N
        dfs(N, number, depth + 1 + index, temp + calc, &answer)
        dfs(N, number, depth + 1 + index, temp - calc, &answer)
        dfs(N, number, depth + 1 + index, temp * calc, &answer)
        dfs(N, number, depth + 1 + index, temp / calc, &answer)
    }
}
func solution(_ N:Int, _ number:Int) -> Int {

    var answer = -1
    dfs(N, number, 0, 0, &answer)
    return answer
}
```

<br/>

[BruteForce]
```swift
import Foundation

var stack: [Int] = []
var count = 0
var minCount = 9

func bruteForceSearch(_ N: Int, _ number: Int) {
    let lastNum = count == 0 ? 0 : stack.last!
    if lastNum == number {
        minCount = count
    }
    
    var n = 0
    var addCount = 0
    var digit = 1
    
    while digit <= 100000 {
        addCount = addCount + 1
        if (count + addCount) >= minCount {
            break
        }
        
        n += (N*digit)
        count += addCount
        
        stack.append(lastNum + n)
        bruteForceSearch(N, number)
        stack.removeLast()
        
        if (lastNum - n) != 0 {
            stack.append(lastNum - n)
            bruteForceSearch(N, number)
            stack.removeLast()
        }
        
        stack.append(lastNum * n)
        bruteForceSearch(N, number)
        stack.removeLast()

        if (lastNum / n) != 0 {
            stack.append(lastNum / n)
            bruteForceSearch(N, number)
            stack.removeLast()
        }
        
        count -= addCount
        
        digit *= 10
    }
}

func solution(_ N:Int, _ number:Int) -> Int {
    bruteForceSearch(N, number)

    return minCount < 9 ? minCount : -1
}
```

<br/>

## 4. 풀이 과정

처음 문제를 풀때, 카테고리가 DP 라는 타이틀을 달고있어서 점화식을 찾아야겠다 라며 머리를 싸맸는데...

결국 못찾아서 인터넷에서 답을 봤다.

<br/>

[이곳](https://apple-apeach.tistory.com/70)을 봤는데.. 알고보니 점화식이 아니라 DFS BFS 를 이용한 완전탐색을 통해 문제를 풀었어야 했다.

DFS 를 이용한 코드는 [피로도](https://gobanest.com/algorithm/programmers/피로도/) 에서 나온 DFS와 상당히 유사하다. 똑같이 depth 를 확인하는데, N으로 표현은 최소 depth를 찾아야하고 피로도는 최대 depth를 찾아야하는 문제였다.

<br/>

똑같은 DFS이지만 알고리즘이 조금 다른 방식으로도 문제를 푼 곳도 있었다. ([이곳](https://boycoding.tistory.com/227))

차이점은 stack을 이용하고, 조금 더 정밀한 조건을 줘서 

    1. if (lastNum - n) != 0
    2. if (lastNum / n) != 0)
    3. if (count + addCount) >= minCount {
            break
        }

조금 더 탐색 시간을 단축시키는 것에 있었다.

<br/>


```toc

```
