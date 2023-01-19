---
emoji: 🧶
title: '[프로그래머스] 모음사전'
date: '2023-01-19 00:00:00'
author: 고반
tags: 알고리즘
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 모음사전](https://school.programmers.co.kr/learn/courses/30/lessons/84512)


<br/>

## 2. 핵심 아이디어

`완전탐색` `DFS`

``

<br/>

## 3. 코드

```swift
import Foundation

var count = 0
var isSearched = false

func solution(_ word:String) -> Int {
    DFS(combString: "", targetWord: word)
    return count
}

func DFS(combString: String, targetWord: String) {
    if combString == targetWord {
        isSearched = true
        return
    } else if combString.count == 5 {
        return
    } else {
        let letters = ["A", "E", "I", "O", "U"]
        for letter in letters {
            count += 1
            if !isSearched {
                DFS(combString: combString + letter, targetWord: targetWord)
            }
        }
    }
}
```

<br/>

## 4. 풀이 과정

어떻게 풀어야할지 한 30분정도 고민을 했는데, 도무지 방법이 떠오르지 않아서, 다른 사람의 풀이를 참고했다.

내가 참고한 곳은 [이곳](https://velog.io/@j_aion/프로그래머스-LV2-모음사전-i93blnu7) 이다.

여기서는 DFS를 멈추는 flag가 설정되어 있지 않아서, 조금 리팩토링 해서 코드를 작성했다.

<br/>

답을 보면 단순한데, 문제를 풀때는 AAAAA -> AAAAE -> AAAAI -> .... -> E -> EE

이런식으로 사전이 형성되는것이 DFS의 재귀로 만들 수 있다는 것을 떠올리기가 힘들었다.

`재귀의 실행 순서`가 이 풀이의 핵심 키였다.

<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ word:String) -> Int {
    var answer = word.count
    var count = 0
    var arr = [781, 156, 31, 6, 1]
    var arr2 = ["A": 0, "E": 1, "I": 2, "O":3, "U":4]

    for ch in word{
        answer += arr2[String(ch)]!*arr[count]
        count += 1
    }
    return answer
}
```
<br/>

다른 글자로 변경될 떄 필요한 자릿수마다의 코스트를 미리 저장해서 더해주는 방법이 있었다.

그런데 AAAAA -> EAAAA 로 변경될 떄 코스트를 어떻게 알아낸걸까?

이를 파악하기가 실전에서는 조금 어려울 것 같다.


```toc

```