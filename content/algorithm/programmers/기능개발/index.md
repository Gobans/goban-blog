---
emoji: 🧶
title: '[프로그래머스] 기능개발'
date: '2023-01-01 00:00:00'
author: 고반
tags: 알고리즘 프로그래머스
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586?language=swift)


<br/>

## 2. 핵심 아이디어

`Queue`와 비슷한 동작을 하는 반복문.

<br/>

## 3. 코드

```swift
func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    var answer: [Int] = []
    var cProgresses = progresses
    var index = 0
    while(true) {
        // 1. progress + speed 를 반복해서 더해준다.
        for idx in index..<cProgresses.endIndex {
            cProgresses[idx] += speeds[idx]
        }
        var cnt = 0
        // 2. 첫번쨰 작업이 완료되었다면 그 다음 작업도 확인해준다.
        while (index < cProgresses.count) {
            if cProgresses[index] >= 100{
                cnt += 1
                index += 1
            } else {
                break
            }
        }
        // 3. 완료된 작업을 answer 배열에 추가해준다.
        if cnt != 0 {
            answer.append(cnt)
        }
        if index == cProgresses.count {
            break
        }
    }
    return answer
}

```

<br/>

## 4. 풀이 과정

풀이의 플로우를 한번 생각을 해본 다음 문제를 풀었다.

그런데 애초에 프로그래머스에서 분류가 스택/큐 라고 딱 정해져 있으니, '해당 플로우에서 어떤 알고리즘을 이용해서 풀어야할까?' 라는 물음 없이 스택/큐로 풀어야겠다 라는 생각이 그냥 들어서 코드 작성 전 알고리즘 플로우를 구상하는 연습을 조금 아쉽게 했다.

백준으로 넘어가면 이제 어떤 알고리즘으로 문제를 풀어야할지 생각을 하는 연습이 되지않을까 생각한다.

<br/>

코드에서도 적혀있듯이

1. progress + speed 를 반복해서 더해준다.
2. 첫번쨰 작업이 완료되었다면 그 다음 작업도 확인해준다.
3. 완료된 작업을 answer 배열에 추가해준다.

이렇게 플로우를 생각하고, 구현하면서 필요한 변수를 그떄마다 추가했다.

변수를 미리 정하는건 특별한 자료구조를 사용해야할때 필요한 작업일듯 하다. 그냥 구현하는거면 코드를 작성하면서 생각하는게 편한 것 같다.

<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    var lastReleaseDate: Int = 0
    var numOfReleases: [Int] = []
    for i in 0..<progresses.count {
        let progress = Double(progresses[i])
        let speed = Double(speeds[i])
        let day = Int(ceil((100 - progress) / speed))
        if day > lastReleaseDate {
            lastReleaseDate = day
            numOfReleases.append(1)
        } else {
            numOfReleases[numOfReleases.count - 1] += 1
        }
    }
    return numOfReleases
}

```

프로그래머스 다른 사람의 풀이에 있는 코드인데, 보니까 다들 날짜를 speed로 나눠서 곧바로 완료되는 시점의 날짜를 구해놓는식으로 풀었다.

이렇게 완료되는 시점을 미리 구해놓으면 내가 쓴 코드처럼 불필요하게 반복할 필요가 없다..! 👍

<br/>


```toc

```
