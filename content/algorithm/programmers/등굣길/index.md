---
emoji: 🧶
title: '[프로그래머스] 등굣길'
date: '2023-01-30 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 등굣길](https://school.programmers.co.kr/learn/courses/30/lessons/42898)


<br/>

## 2. 핵심 아이디어

`DP`

<br/>

## 3. 코드

[swift]
```swift
func solution(m: Int, n: Int, puddles: [[Int]]) -> Int {
    var grid: [[Int]] = Array(repeating: Array(repeating: 0, count: m + 1), count: m + 1)
    grid[1][1] = 1
    for puddle in puddles {
        grid[puddle[1]][puddle[0]] = -1
    }
    for i in 1...n {
        for j in 1...m {
            if grid[i][j] == -1 {
                grid[i][j] = 0
                continue
            }
            grid[i][j] += (grid[i - 1][j] + grid[i][j - 1]) % 1000000007
        }
    }
    return grid[n][m]
}
```

<br/>

## 4. 풀이 과정

처음엔 DFS로 여러 경로를 탐색하면 바로 풀 수 있지 않나? 생각을 했다.

    1. path 를 DFS 로 탐색. (경로의 개수를 구해야하기 때문에)
    2. 격자에다가 현재까지 탐색한 이동값을 넣어줌 (같거나 작은 경우만 계속 탐색)
    3. 탐색을 완료하면 값에 따라 cnt 갱신

<br/>

이 생각을 바탕으로 코드를 작성하였는데..

```python
minMoveCnt = 100000
pathCnt = 0
def solution(m, n, puddles):
    grid = [[ 0 for _ in range(m + 1)] for _ in range(n + 1)]
    moves = [(0,1), (1,0)]
    for puddle in puddles:
        grid[puddle[1]][puddle[0]] = -1
    def DFS(x,y, moveCnt):
        global minMoveCnt
        global pathCnt
        if (x,y) == (m,n):
            if moveCnt == minMoveCnt:
                pathCnt += 1
            elif moveCnt < minMoveCnt:
                minMoveCnt = moveCnt
                pathCnt = 1
            return
        for move in moves:
            dx = x + move[0]
            dy = y + move[1]
            if dx <= m and dy <= n:
                if grid[dy][dx] == 0 or moveCnt + 1 <= grid[dy][dx]:
                    grid[dy][dx] = moveCnt + 1
                    DFS(dx,dy, moveCnt + 1)

    DFS(1,1,0)
    return pathCnt
```

시원하게~ 시간초과가 먹혔다.

이것도 바로 이전에 풀었던 [정수삼각형](https://gobanest.com/algorithm/programmers/정수%20삼각형/) 과 비슷한 점화식으로 풀어야하는 문제였다.

<br/>

`아래` `오른쪽`으로만 방향이동이 가능하기 때문에,

<br/>

> dp[i][j] = dp[i-1][j] + dp[i][j-1]

새로 값을 넣을 칸 = 바로 위칸까지의 경로수 + 바로 왼쪽칸 까지의 경로수

이 점화식이 성립하는거였다.

<br/>

나는 왜 바로 알아차리지 못했을까..! 🥲

[이곳](https://dev-note-97.tistory.com/141)을 참고해서 알게되었다..

<br/>
<br/>

덧붙여 프로그래머스 문제들 swift 지원을 왜이렇게 안하는걸까... 3개에 하나꼴로 지원을 안하는 느낌이다. 😿 주절주절...

<br/>


```toc

```
