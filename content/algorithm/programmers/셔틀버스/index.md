---
emoji: 🧶
title: '[프로그래머스] [1차] 셔틀버스'
date: '2023-03-21 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[[1차]셔틀버스](https://school.programmers.co.kr/learn/courses/30/lessons/17678)


<br/>

## 2. 핵심 아이디어

구현

<br/>

## 3. 코드

```swift
func solution(_ n:Int, _ t:Int, _ m:Int, _ timetable:[String]) -> String {
    var answer = 0
    var busCnt = 0
    var curTime = 540
    var curRide = 0
    let minutetable = timetable.map{convertMinute($0)}.sorted(by:>)
    var cMinutetable = minutetable
    var isFull = false
    while busCnt < n {
        busCnt += 1
        while !cMinutetable.isEmpty && cMinutetable.last! <= curTime && curRide < m{
            cMinutetable.removeLast()
            curRide += 1
        }
        isFull = curRide == m ? true : false
        curTime += t
        curRide = 0
    }
    let lastRiderIdx = cMinutetable.count
    if isFull {
        answer = minutetable[lastRiderIdx] - 1
    } else {
        answer = 540 + (n-1)*t
    }
    return convertMinuteToFormat(answer)
}

func convertMinute(_ time: String) -> Int {
    let sep = time.components(separatedBy: ":")
    let hour = Int(sep[0])!
    let minute = Int(sep[1])!
    return hour*60 + minute
}

func convertMinuteToFormat(_ time: Int) -> String {
    var hour = time/60
    var minute = time%60
    var strHour = String(hour)
    var strMinute = String(minute)
    if hour < 10 {
       strHour = "0" + strHour 
    }
    if minute < 10 {
       strMinute = "0" + strMinute
    }
    return strHour + ":" + strMinute
}

```

<br/>

## 4. 풀이 과정

뭔가 특별한 알고리즘을 써야하나 싶어서 문제를 시간을 들여 좀 자세히 봤는데, 딱히 떠오르지 않았다. 또한 주어진 데이터의 크기가 작았기 때문에 구현으로 문제를 풀어도 충분하다고 생각이 들었다.

중요한 포인트는 다음과 같다.

<br/>

    마지막 버스에 사람들이 꽉 차는지에 따라 답이 달라짐
    - 꽉찬경우: 마지막으로 탄 사람의 시간 - 1
    - 꽉안찬경우: 마지막 버스의 도착시간 - 1

<br/>

마지막 버스에 사람들이 다 차있는지 확인하기 위해 시뮬레이션 형태로 버스마다 탑승하는 사람들을 빼주었다.

그리고 마지막으로 탑승한 사람의 idx는 대기열에 있는 사람의 수가 된다.

이를 이용하여 경우에 따라 다르게 답을 산출했다.


<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ n:Int, _ t:Int, _ m:Int, _ timetable:[String]) -> String {
    let initTime = 9*60
    let sortedCrewTime = timetable.map { Int($0.prefix(2))! * 60 + Int($0.suffix(2))! }.sorted()
    var crewIndex = 0
    for i in 0..<n {
        var cnt = 0
        while crewIndex < sortedCrewTime.count, sortedCrewTime[crewIndex] <= initTime + (i * t) {
            cnt += 1
            crewIndex += 1
            if cnt == m {
                guard i != n-1 else {
                    return String(format: "%02d:%02d", (sortedCrewTime[crewIndex-1]-1)/60, (sortedCrewTime[crewIndex-1]-1)%60)
                }
                break
            }
        }
    }
    let lastTime = initTime + (n-1) * t
    return String(format: "%02d:%02d", lastTime/60, lastTime%60)
}
```

<br/>

프로그래머스에 있는 다른사람의 풀이인데, 

`prefix` `suffix` `String(format)` 사용이 신기해서 가져와봤다.

prefix는 접두사 suffix는 접미사 라는 의미인데, paramater에 숫자n을 넣으면 prefix는 앞에서부터 n 만큼, suffix는 뒤에서부터 n만큼 원소를 가져온다.

비단 String 뿐만 아니라 Array에서도 사용가능하다.

<br/>

`String(format)` 이거는 C나 C++ 에서 출력할때 쓰는것처럼 쓰면되는데, **Format Specifier** 라고 부른다고 한다.

소수점 n번까지 출력할때 유용할듯하다.

ex) String(format: "%.2f", number)

소수점 2번째 자리까지만 출력


```toc

```
