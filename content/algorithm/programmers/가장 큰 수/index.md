---
emoji: 🧶
title: '[프로그래머스] 가장 큰 수'
date: '2023-01-08 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42747)


<br/>

## 2. 핵심 아이디어

`문자열 정렬`

<br/>

## 3. 코드

[swift]
```swift
func solution(_ numbers:[Int]) -> String {
    var answer = ""
    var stringNumbers = numbers.map({ String($0)})
    stringNumbers.sort { lhs, rhs in
        return isLhsBigger(lhs: lhs, rhs: rhs)
    }
    stringNumbers.forEach { stringNumber in
        answer += stringNumber
    }
    if Int(answer) == 0{
        return "0"
    }
    return answer
}

func isLhsBigger(lhs: String, rhs: String) -> Bool {
    let lhsFirst = lhs + rhs
    let rhsFirst = rhs + lhs
    return lhsFirst > rhsFirst
}


```

<br/>

## 4. 풀이 과정

우선 문제를 보자 아래와 같은 생각을 했다.

1. 정수를 이어 붙였을 떄 가장 큰수를 구하라
    - 앞자리 수가 가장 큰 수를 붙여서 만든 수
2. 문자로 정렬해놓고 붙이면 되지않나?

그래서 단순하게 문자를 대소 비교해서 정렬 했는데, 제대로 정렬이 되지 않는 상황이 발생했다.

예를들면 "121" 과 "12", "10" 과 "1" 문자열에서는 이들을 "121" 과 "10" 이 우선되게 정렬을 했다.


그래서 그 다음 든 생각이 '문자열 안의 chracter 를 자릿수마다 하나씩 비교하자' 였는데, 문자열 서로의 길이가 다를 경우, 자릿수를 다시 반복해서 비교해줘야하는 번거롭고 비효율적인 문제가 있었다.


고민고민을 하다 결국 좋은 방법이 떠올랐는데, `문자열 두개를 서로 다른 순서를 덧붙여 만들어진 문자열을 비교`하는 것이였다. 

[swift]
```swift
// compare one by one
func isLhsBigger(lhs: String, rhs: String) -> Bool {
    let lhsFirst = lhs + rhs
    let rhsFirst = rhs + lhs
    let lhsArray: [Int] = lhsFirst.compactMap{ $0.wholeNumberValue }
    let rhsArray: [Int] = rhsFirst.compactMap{ $0.wholeNumberValue }
    for (lhsNum, rhsNum) in zip(lhsArray, rhsArray) {
        if lhsNum > rhsNum {
            return true
        } else if lhsNum < rhsNum{
            return false
        }
    }
    return true
}
```

그런데 또 생각해보니 이렇게 하나하나씩 반복문으로 비교할 필요가 없었다. 어차피 합쳤을 떄 큰수 대로 정렬하면 되지 않는가?

그래서 코드를 바꿨고 그것이 지금 코드이다.

그런데 테스트 케이스에서 하나의 케이스에서 계속 틀린다고 뜨길래, 뭔가 싶었는데 numbers가 0 으로만 이루어졌을 떄의 케이스였다.. 

예외 케이스를 한번 생각해보고 테스트 해보는 것이 필요하다고 느꼈다.


<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ numbers: [Int]) -> String {
    let sortedNumbers = numbers.sorted {
        Int("\($0)\($1)")! > Int("\($1)\($0)")!
    }

    let answer = sortedNumbers.map { String($0) }.reduce("") { $0 + $1 }
    return sortedNumbers.first == 0 ? "0" : answer
}
```

reduce 를 써서 문자열을 더해준 것이 인상깊었다.

나도 reduce 를 잠깐 생각했는데, 왜 Int 자료형만 된다고 생각하고 안썼을까..? 바보다

<br/>


```toc

```
