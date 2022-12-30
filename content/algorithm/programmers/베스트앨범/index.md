---
emoji: 🧶
title: '[프로그래머스] 베스트 앨범'
date: '2022-12-30 14:00:00'
author: 고반
tags: 알고리즘 프로그래머스
categories: Algorithm
---

## 1. 문제

`프로그래머스`

[고득점 Kit 베스트앨범](https://school.programmers.co.kr/learn/courses/30/lessons/42579)


<br/>

## 2. 핵심 아이디어

Dictionary 를 이용한 `해시`, `정렬`.

<br/>

## 3. 코드

```swift
import Foundation

func solution(_ genres:[String], _ plays:[Int]) -> [Int] {
    // 장르별로 재생횟수의 총합을 구한 후 내림차순으로 정렬
    var genrePlayCountDict: [String:Int] = [:]
    for (genre, play) in zip(genres, plays){
     if genrePlayCountDict[genre] != nil {
         genrePlayCountDict[genre]! += play
     } else {
         genrePlayCountDict[genre] = play
     }
    }
    let sortedGenrePlayCountDict = genrePlayCountDict.sorted(by: {$0.value > $1.value}).map{ $0.key }
    
    // 장르마다 반목문을 돌면서 해당 song의 고유 번호 (idx) 를 dictionary의 key값, 재생횟수를 value값으로 저장
    var bestAlbum: [Int] = []
    for genreName in sortedGenrePlayCountDict {
        var songPlayCountDict: [Int:Int] = [:]
        for (idx, genre) in genres.enumerated() {
            if genreName == genre {
                songPlayCountDict[idx] = plays[idx]
            }
        }
        // 재생횟수가 같으면 고유번호가 낮은 순으로 정렬, 그외에는 재생횟수의 내림차순으로 정렬
        let sortedSongPlayCountDict = songPlayCountDict.sorted{ (f,s) -> Bool in
            if f.value == s.value {
                return f.key < s.key
            }
            return f.value > s.value
        }.map { $0.key }
        // bestAlbum의 최대 수록 횟수가 2번이기 떄문에 maxAddCount로 수록 횟수 제어
        var maxAddCount = 2
        for songNum in sortedSongPlayCountDict {
            maxAddCount -= 1
            bestAlbum.append(songNum)
            if maxAddCount == 0 {
                break
            }
        }
    }
    return bestAlbum
}

```

<br/>

## 4. 풀이 과정

문제에서 명확한 문제해결의 흐름을 준다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

이를 그대로 코드의 플로우로 옮긴다면

1. 장르에 속한 노래의 재생횟수를 구한다. 이를 재생횟수의 내림차 순으로 정렬한다.
2. 정렬된 장르의 순서에 따라 가장 많이 재생된 노래를 수록한다. <br/>(재생 횟수가 같다면 고유 번호가 낮은 노래 우선)

따라서 필요한 것은 `장르별 재생횟수 내림차순 배열`, `재생횟수, 고유 번호를 기준으로 정렬된 배열` 이 두가지이다.

이 배열들을 만들때 `해시` 와 `정렬`를 사용하였다.



<br/>

## 5. 다른 사람의 코드

```swift
import Foundation

func solution(_ genres:[String], _ plays:[Int]) -> [Int] {
    var playList = [String:(play: Int, music: [Int:Int])]()
    var answer = [Int]()

    for (index, value) in genres.enumerated() {
        if let genre = playList[value]?.play {
            playList[value]?.play = genre + plays[index]
            playList[value]?.music[index] = plays[index]
        } else {
            playList[value] = (play: plays[index], music: [index:plays[index]])
        }
    }

    let rank = playList.sorted(by: { $0.value.play > $1.value.play })

    rank.forEach { song in
        let songRank = song.value.music.sorted{$0.key < $0.key }.sorted { $0.value > $01.value }

        let max = songRank.count > 1 ? 2 : 1
        for i in 0..<max {
            answer.append(songRank[i].key)
        }
    }

    return answer
}

```

하나의 dictionary 안에 튜플 형태로 play, music 두개를 두는 것이 인상적이였다.

아주 깔끔한듯!

<br/>


```toc

```
