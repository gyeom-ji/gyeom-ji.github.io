---
title: "Two Pointer & Sliding Window"
author: gyeomji
date: 2024-05-06 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Two Pointer, Sliding Window]
pin: false
math: true
mermaid: true
---


## Two Ponter

---

리스트에 순차적으로 접근해야 할 때 <span style="color:#9fb584">**두 개의 점의 위치를 기록하며 처리하는 알고리즘**</span>으로, 구간의 합을 구하거나, 구간을 비교할 때 효율적으로 사용할 수 있고, 정렬된 두 리스트의 합집합에도 사용된다. 한 번의 순회에 목표값을 찾을 수 있기 때문에 <span style="color:#9fb584">**O(N)의 시간 복잡도**</span>를 가진다.

<br/> 

## Two Ponter 동작 방식

---

- 포인트를 2개 준비한다. (2개의 포인터는 현재 부분 배열의 시작(start) 과 끝(end)을 가르킨다.)
- 시작은 start = end = 0이고, 항상 두 포인터들의 관계는 start<=end 이다.
    - start = end 일 경우 크기가 0인 아무것도 포함하지 않는 부분 배열을 뜻한다.
- 아래의 과정을 start < N 동안 반복한다.

1. 현재 부분합이 target 이상이거나, 이미 end = N 일 경우 start += 1
2. 그렇지 않을 경우 e += 1
3. 현재 부분합이 target과 같으면 결과 += 1 

<br/>

## Two Ponter 사용해 해결한 문제

---

#### 백준 - 수들의 합

\- N개의 수로 된 수열 A[1], A[2], …, A[N] 이 있을 때, 이 수열의 i번째 수부터 j번째 수까지의 합 A[i] + A[i+1] + … + A[j-1] + A[j]가 M이 되는 경우의 수를 구하는 문제<br />
<br />

``` swift 
var sum = 0, result = 0, start = 0, end = 0

let input = readLine()!.split(separator: " ").map{Int(String($0))!}
let target = input[1]
let arr = readLine()!.split(separator: " ").map{Int($0)!}

while true {
    if sum >= target {
        sum -= arr[start]
        start += 1
    } else if end == input[0] {
        break
    } else {
        sum += arr[end]
        end += 1
    }
    
    if sum == target {
        result += 1
    }
}

print(result)

```

<br />

## Sliding Window

---

연속되는 투 포인터와 유사하게 부분 배열들을 활용하여 특정 조건을 일치시키는 알고리즘이지만, <span style="color:#9fb584">**부분 배열의 길이가 고정**</span>되어있다. 고정 사이즈의 윈도우가 이동하며 윈도우 내에 있는 데이터를 이용해 문제를 처리한다. 부분 배열의 길이가 고정되어 있기 때문에 포인터 변수가 2개일 필요가 없다. 배열이나 리스트의 일정 범위 값을 비교할 때(구간 합, 부분 문자열 구하기 등) 효율적으로 사용할 수 있다. 한 번의 순회에 목표값을 찾을 수 있기 때문에 <span style="color:#9fb584">**O(N)의 시간 복잡도**</span>를 가진다.

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
