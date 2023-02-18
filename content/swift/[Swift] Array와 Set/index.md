---
emoji: 🧐
title: '[Swift] Array와 Set의 속도차이'
date: '2023-02-17 00:00:00'
author: 고반
tags: Swift
categories: Swift
---

# 개요

[알고리즘 문제](https://gobanest.com/algorithm/programmers/가장%20먼%20노드/)를 풀던 도중.. Array와 Set을 사용했을때 많은 시간차이가 발생하는 것을 발견했다.

<br/>

그 이유는 무엇일까.

간단한 실험을 통해서 얼마나 속도가 차이나는지 알아보고 이유 또한 알아보자.

<br/>

## 실험

우선 실험에 사용할 유용한 조수를 소개한다.

코드를 클로저로 받아서 사용할 수 있는 아주 좋은 녀석이다.

<br/>

```swift
public func progressTime(_ closure: () -> ()) -> TimeInterval {
    let start = CFAbsoluteTimeGetCurrent()
    closure()
    let diff = CFAbsoluteTimeGetCurrent() - start
    return (diff)
}
```

코드는 [이곳](https://chanhhh.tistory.com/94)에서 가져왔다.

<br/>

그 다음 Array와 Set에 원소 추가, 순회, 삭제를 테스트했다.

<br/>

```swift
import Foundation

public func ArraySet_Performance() {
    
    var testArray: [Int] = []
    var testSet = Set<Int>()
    
    // Addition
    /// Array: 18.3260840177536
    print(progressTime {
        for i in 0...10*1000 {
            testArray.append(i)
        }
    })
    
    /// Set: 0.09251093864440918
    print(progressTime {
        for i in 0...10*1000 {
            testSet.insert(i)
        }
    })
    
    // Iteration
    /// Array: 0.029738903045654297
    print(progressTime {
        for _ in testArray {
            
        }
    })
    
    /// Set: 0.000051975250244140625
    print(progressTime {
        for _ in testSet {
            
        }
    })
    
    // Removal
    /// Array: 0.008929967880249023
    print(progressTime{
        while !testArray.isEmpty {
            testArray.removeFirst()
        }
    }
    )
    
    /// Set: 0.0012810230255126953
    print(progressTime{
        while !testSet.isEmpty {
            testSet.removeFirst()
        }
    }
    )
    
}

```

<br/>

1만번 기준 속도의 차이는 다음과 같다.

<br/>

    1. 추가: 약 198배
    2. 순회: 약 570배
    3. 삭제: 약 6배

<br/>

순회가 속도 차이는 가장 크지만, 기준 속도의 차이 떄문에 추가가 값의 차이가 가장 많이난다.

<br/>

실로 어마어마한 차이이다.. 왜 이런 차이가 나는 것일까?

<br/>

## 이유

<br/>

우선 Array와 Set의 가장 큰 차이는 `정렬`이다.

코드를 보자.

<br/>

```swift
public func AccessValue() {
    
    var testArray: [Int] = []
    var testSet = Set<Int>()
    
    for i in 0...100 {
        testArray.append(i)
    }

    for i in 0...100 {
        testSet.insert(i)
    }

    print("Array")
    for i in testArray {
        print(i, terminator: " ")
    }
    print("")
    print("Set")
    for i in testSet {
        print(i, terminator: " ")
    }

}

/* Result
 
 // Array is an ordered, random-access collection.
 
 Array
 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100
 
 // Set is an unordered collection of unique elements.
 
 Set
 51 10 85 95 84 76 78 25 5 77 53 18 40 29 36 45 96 80 28 0 34 4 56 48 35 15 3 42 41 68 50 64 47 88 16 57 14 38 92 74 7 24 90 46 87 1 62 65 23 54 12 55 58 79 32 43 31 17 22 81 100 6 89 94 99 66 82 61 8 9 60 91 26 19 86 93 2 71 52 27 13 37 39 73 49 72 59 33 75 11 83 98 20 63 67 97 21 30 70 69 44
 */

```

<br/>

보다시피 Array와 Set에 똑같은 순서로 삽입을 했지만, Array는 순차적으로 제거된 반면 Set은 랜덤하게 삭제되었다.

이러한 차이의 이유를 Array와 Set의 원소 추가, 순회, 삭제 메소드에서 살펴보자.

<br/>

### 추가

[Array]
```swift
  public mutating func append(_ newElement: __owned Element) {
    // Separating uniqueness check and capacity check allows hoisting the
    // uniqueness check out of a loop.
    _makeUniqueAndReserveCapacityIfNotUnique()
    let oldCount = _buffer.mutableCount
    _reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)
    _appendElementAssumeUniqueAndCapacity(oldCount, newElement: newElement)
    _endMutation()
  }
```

<br/>

- _makeUniqueAndReserveCapacityIfNotUnique()
    - 값의 unique 체크 및 추가 buffer 생성
- _reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)
    - 특수한 경우 떄문에 buffer 가 제대로 생성되지 않았을 떄, buffer 생성
- _appendElementAssumeUniqueAndCapacity
    - 새로운 버퍼에 원소 추가

Array의 경우 element가 추가될 떄 값이 unique 한지 (주요하게는 class 인지) 체크한 후 buffer를 생성하여 원소를 추가해준다.

이 과정에서 element의 순서와 unique가 유지된다.


<br/>



[Set]
```swift
  internal func _unsafeInsertNew(_ element: __owned Element) {
    _internalInvariant(count + 1 <= capacity)
    let hashValue = self.hashValue(for: element)
    if _isDebugAssertConfiguration() {
      // In debug builds, perform a full lookup and trap if we detect duplicate
      // elements -- these imply that the Element type violates Hashable
      // requirements. This is generally more costly than a direct insertion,
      // because we'll need to compare elements in case of hash collisions.
      let (bucket, found) = find(element, hashValue: hashValue)
      guard !found else {
        ELEMENT_TYPE_OF_SET_VIOLATES_HASHABLE_REQUIREMENTS(Element.self)
      }
      hashTable.insert(bucket)
      uncheckedInitialize(at: bucket, to: element)
    } else {
      let bucket = hashTable.insertNew(hashValue: hashValue)
      uncheckedInitialize(at: bucket, to: element)
    }
    _storage._count &+= 1
  }
```

<br/>

반면에 Set의 원소 추가 과정을 보면, hashValue로 중복된 값을 찾고 중복된 값이 있다면 아예 원소를 찾지 않는다.

```swift
    let (bucket, found) = find(element, hashValue: hashValue)
    guard !found else {
    ELEMENT_TYPE_OF_SET_VIOLATES_HASHABLE_REQUIREMENTS(Element.self)
    }
```

<br/>

때문에 Array와는 다르게 `중복된 값이 없다.`

<br/>

```swift
/// Insert a new entry for an element at index'.
@inlinable @inline(__always)
internal func insert( bucket: Bucket) {
    _internalInvariant(!isOccupied(bucket))
    words[bucket.word].uncheckedInsert(bucket.bit)
}
```

<br/>

또 다르게 주목해야할 부분은 hashTable의 insert 동작이다.

해당 코드를 보면 words의 subscript로 bucket.word를 사용하는 것을 볼 수 있다. 그리고 이 words[bucket.word] (UnsafeMutablePointer) 공간에 bit가 저장된다.

<br/>

```swift
extension _UnsafeBitset {
@inlinable
@inline (__ always)
internal static func word(for element: Int) -> Int {
    _internalInvariant(element >= 0)
    // Note: We perform on UInts to get faster unsigned math (shifts).
    let element = UInt (bitPattern: element)
    let capacity = UInt(bitPattern: Word.capacity)
    return Int(bitPattern: element / capacity)
}
```

<br/>

word는 offset (element)과 capacity를 이용해 고유하게 생성되어 사용된다.

<br/>


### 순회

[Set]
```swift
    // hashTable Iterator
    @inlinable
    @inline(__always)
    internal mutating func next() -> Bucket? {
      if let bit = word.next() {
        return Bucket(word: wordIndex, bit: bit)
      }
      while wordIndex + 1 < hashTable.wordCount {
        wordIndex += 1
        word = hashTable.words[wordIndex]
        if let bit = word.next() {
          return Bucket(word: wordIndex, bit: bit)
        }
      }
      return nil
    }
```

<br/>

hashTable의 Iterator를 보면 다음 bucket을 wordIndex와 다음 bit로 식별하여 Iteration 하고 있다.

하지만 words는 UnsafeMutablePointer<Word>이고, 저장되는 값의 주소는 hashValue가 바뀜에 따라 word의 생성에서 매번 달라진다.

때문에 매번 Set의 순서가 달라지는 것이다.


### 삭제