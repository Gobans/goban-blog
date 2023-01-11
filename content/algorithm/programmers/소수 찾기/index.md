---
emoji: 🧶
title: '[프로그래머스] 소수 찾기'
date: '2023-01-11 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)


<br/>

## 2. 핵심 아이디어

`순열` `완전탐색`

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ numbers:String) -> Int {
    var answer = 0
    var madePermuString = Set<String>()
    let splitedNumbers = numbers.map{ $0 }
    for targetNum in 1...splitedNumbers.count {
        let permuByTargetNum = permutation(splitedNumbers, targetNum)
        for combArray in permuByTargetNum {
            let combString = combArray.reduce("") { String($0) + String($1)}
            if combString.first == "0" || madePermuString.contains(combString) {
                continue
            }
            madePermuString.insert(combString)
            if isPrime(Int(combString)!) {
                answer += 1
            }
        }
    }
    
    return answer
}

func isPrime(_ n: Int) -> Bool {
    guard n >= 2     else { return false }
    guard n != 2     else { return true  }
    guard n % 2 != 0 else { return false }
    return !stride(from: 3, through: Int(sqrt(Double(n))), by: 2).contains { n % $0 == 0 }
}

```

<br/>

## 4. 풀이 과정

풀이를 다음과 같이 두가지 방법을 생각했다.

    종이 조각으로 만들 수 있는 소수가 몇개인지 확인
    1. 조합을 사용하여 숫자를 만들어냄 -> 해당 숫자가 소수인지 판별.
    2. 소수의 조건에 따라 숫자를 만들기 (가능한가?)
    -> 소수의 조건: 1과 자신을 제외한 자연수로 나눠지지 않음.
    -> 이건 결국 숫자를 만들어서 확인을 해야하나? 잘 모르겠다.

그런데 2번 방법은 더 복잡하게 푸는 것 같아서 직관적인 1번 방법으로 코드를 구현하기로 했다.

코드 플로우를 다음과 같이 생각했다.

    1. numbers 를 문자열 배열로 바꿈
    2. 바꾼 문자열 배열을 조합한 수들을 배열로 만듬
    3. filtering 하여 맨앞 숫자가 0인 경우를 제외
    4. 반복문으로 소수인지 판별

<br/>

우선 조합해서 만들어진 수들이 순서가 상관있었기 떄문에 `순열`을 사용했다.

ex) 17, 71

해당코드는 [이곳]() 에서 확인할 수 있다.

<br/>

조합으로 만들어진 string 중에 중복되는 string은 거르기 위해 set으로 `madePermuString` 에 문자열을 넣어가며 중복을 판별하였다.

소수 판별은 isPrime 메소드를 만들어 사용했는데 제곱근을 이용해서 2 씩 n을 늘려가며 소수를 판별하면 

$$O(\frac{1}{2}\sqrt{n})$$

의 시간복잡도를 가진다.

<br/>


```toc

```
