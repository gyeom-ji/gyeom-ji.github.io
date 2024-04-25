---
title: "탐색 알고리즘 Search Algorithm"
author: gyeomji
date: 2024-03-01 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Linear Search, Binary Search, 선형 탐색, 이진 탐색]
pin: false
math: true
mermaid: true
---

## 선형 탐색 (Linear Search) - O(N)

---
<br/> 
정렬되지 않은 배열, 리스트 형태의 자료구조에서 순차적으로 앞 또는 뒤에서부터 모든 데이터 탐색<br />


[ 장점 ]
<br />
- 탐색 알고리즘의 가장 기초가 되는 알고리즘으로 구현이 쉬움
- 순차적으로 값을 탐색하기 때문에, 정렬 유무에 관계 없음
<br />
[ 단점 ]
<br />
- 배열의 크기가 커질수록 시간이 오래 걸림 

<br/> 
<br/>

## 이진 탐색 (Binary Search) - O(logn)

---

<span style='background-color:#c8d8b4'>정렬</span>된 데이터에서 사용하며 데이터가 저장된 구간을 반으로 나눠 데이터가 저장된 위치를 찾음
<br/> 

1. 정렬된 배열의 mid 와 value 비교
2. mid == value 종료
3. 탐색 범위 재설정 ( mid < data ? low ~ mid - 1 : mid + 1 ~ high)
4. mid == value 일때까지 반복

<br/> 
[ 단점 ]
<br/> 
- 정렬되어 있어야만 구현할 수 있다
<br/>

```swift
func recursiveBinarySearch(_ arr: [Int],_ value: Int,_ start: Int,_ end: Int) -> Int {
    if start > end {
        return -1
    }
    let mid = arr.count / 2
    if arr[mid] == value { return mid }
    if arr[mid] < value {
        return recursiveBinarySearch(arr, value, mid + 1, end)
    } else {
        return recursiveBinarySearch(arr, value, start, mid - 1)
    }
}

func iterativeBinarySearch(_ arr: [Int],_ value: Int,_ start: Int,_ end: Int) -> Int {
    var start = start, end = end

    while start <= end {
        let mid = (start + end) / 2
        if arr[mid] == value { return mid }
        if arr[mid] < value {
            start = mid + 1
        } else {
            end = mid - 1
        }
    }
    return -1
}
```
<br/>
<br/>

## 이진탐색 알고리즘 사용해 해결한 문제

---

#### Programmers - 입국심사

\- 입국 심사관 마다 걸리는 시간이 다를때 n명이 입국 심사를 완료하는데 걸리는 최소 시간 구하는 문제<br />

> 💡 n명이 입국 심사 완료하는데 걸리는 최대 시간 = times.max()! * n

<br />

```swift
import Foundation

func solution(_ n:Int, _ times:[Int]) -> Int64 {
    var end = n * times.max()!
    var start = 1

    while start <= end {
        var mid = Int(start + end) / 2
        var person = 0

        for t in times {
            person += mid / t
        }
        if person < n {
            start = mid + 1
        } else {
            end = mid - 1
        }
    }
    return Int64(start)
}
```

- 배운점
  : person == n 을 만족해도 **최소시간**을 구해야하기 때문에 break 하지 않고 계속 진행 해야한다

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source