---
emoji: 🧶
title: '[프로그래머스] 정수 삼각형'
date: '2023-01-30 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 정수 삼각형](https://school.programmers.co.kr/learn/courses/30/lessons/43105)


<br/>

## 2. 핵심 아이디어

`DP` `메모이제이션`

<br/>

## 3. 코드

[swift]
```swift
import Foundation

func solution(triangle:[[Int]]) -> Int {
    var answer = 0
    var nTriangle = triangle.map { [0] + $0 + [0] }
    for i in 1..<nTriangle.count {
        for j in 1...i+1 {
            nTriangle[i][j] += max(nTriangle[i-1][j-1], nTriangle[i-1][j])
        }
    }
    answer = nTriangle[nTriangle.endIndex - 1].max()!
    return answer
}
```

<br/>

## 4. 풀이 과정

간단하게 DFS 로 거쳐간 숫자를 탐색 후 업데이트 하면 되지 않을까 생각을 했는데.. DFS로 풀었더니 시간초과가 났다.

```python
maxSum = 0
def solution(triangle):
    answer = 0
    def DFS(nodeIndex, lineIndex, sum):
        global maxSum
        sum += triangle[lineIndex][nodeIndex]
        if sum > maxSum:
            maxSum = sum
        if lineIndex >= len(triangle) - 1:
            return
        leftIndex = nodeIndex
        rightIndex = nodeIndex + 1
        if leftIndex >= 0:
            DFS(leftIndex, lineIndex + 1, sum)
        if rightIndex < len(triangle[lineIndex + 1]):
            DFS(rightIndex, lineIndex + 1, sum)
    DFS(0,0,0)
    return maxSum
```

<br/>

조금 성능을 개선해보기 위해서 0으로 초기화 된 Visit 배열을 만들어, DFS 로 탐색을 할 때 한번 더 조건을 걸어 탐색하는 코드를 작성해봤다.
```python
maxSum = 0
def solution(triangle):
    answer = 0
    # visit 배열을 만들어서 최댓값 저장, 최댓값 보다 크면 해당 노드 재탐색
    triangleSum = []
    for line in triangle:
        temp = []
        for _ in line:
            temp.append(0)
        triangleSum.append(temp)

    def DFS(nodeIndex, lineIndex, sum):
        global maxSum
        sum += triangle[lineIndex][nodeIndex]
        triangleSum[lineIndex][nodeIndex] = sum
        if sum > maxSum:
            maxSum = sum
        if lineIndex >= len(triangle) - 1:
            return
        leftIndex = nodeIndex
        rightIndex = nodeIndex + 1
        if leftIndex >= 0 and triangleSum[lineIndex + 1][leftIndex] < sum + triangle[lineIndex + 1][leftIndex]:
            DFS(leftIndex, lineIndex + 1, sum)
        if rightIndex < len(triangle[lineIndex + 1]) and triangleSum[lineIndex + 1][rightIndex] < sum + triangle[lineIndex + 1][rightIndex]:
            DFS(rightIndex, lineIndex + 1, sum)
    DFS(0,0,0)
    return maxSum
```

하지만 여전히 효율성 테스트는 통과하지 못했다..ㅜ

<br/>

[이곳](https://velog.io/@younge/Python-프로그래머스-정수-삼각형-동적계획법) 을 참고하여 답을 봤는데, `메모이제이션` 이라는게 이때 사용되는거구나 알게되었다.

확실히 문제를 많이 풀어야 어떻게 풀지 좀 감을 잡을 수 있을 것 같다.

<br/>


```toc

```
