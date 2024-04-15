---
title: "동적 계획법 Dynamic Programming"
author: gyeomji
date: 2024-03-03 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Dynamic Programming, 동적 계획법]
pin: false
math: true
mermaid: true
---

<br/> 
<span style='background-color:#c8d8b4'>한 큰 문제를 여러개의 작은 문제로 쪼개 그 결과를 저장하고 큰 문제를 해결할 때 **재사용**</span>하는 특정한 알고리즘이 아닌 하나의 문제 해결 패러다임<br /> 
문제를 작게 쪼개서 하위 문제를 해결하고 연계적으로 큰 문제를 헤결한다는 점은 분할 정복과 동일하지만, DP는 중복이 일어나는 경우 사용한다.<br /> 

> 💡  <span style="font-size: 15px"> 분할 정복은 1. 분할 2. 정복으로 문제를 나눠서 해결한다.<br /> 　 　분할된 하위 문제가 중복이 일어나지 않는 경우에 사용한다.<br />　 　병합 정렬, 이진 탐색, 거듭제곱 연산</span>

<br/> 
<br/>

## DP 사용 이유

---

- 재귀 방식은 DP와 매우 유사한데, 일반적인 재귀를 단순히 사용 시 동일한 작은 문제들이 여러번 반복되어 비효율적인 계산이 될 수 있다

- 동일한 문제들이 여러번 반복 될 때 DP를 사용하면 효율적으로 문제 해결이 가능하다

<br/>
<br/>

## DP 사용 조건

---

1. 겹치는 부분 문제 Overlapping Subproblems
    - DP는 문제를 **나누고** 그 문제의 결과 값을 **재사용**하여 전체 답을 구한다
    - <span style='background-color:#c8d8b4'>동일한 작은 문제들이 반복해 나타나는 경우 사용 가능</span>

2. 최적 부분 구조 Optimal Substructure
    - <span style='background-color:#c8d8b4'>부분 문제의 최적 결과 값을 사용해 전체 문제의 최적 결과를 낼 수 있는 경우 사용 가능</span>

<br/> 
<br/>

## DP 사용

---

DP는 특정 경우에 사용하는 알고리즘이 아닌 하나의 **방법론**이기 때문에 **다양한 문제 해결에 사용** 가능하다.
<br />

### [ DP 사용 과정 ]

1. DP 사용 조건을 만족하는 문제인지 확인

2. 문제 변수 파악
    - DP는 현재 변수에 따라 결과 값을 찾고 그 값을 재사용한다
    - => 문제 내 <span style='background-color:#c8d8b4'>변수 개수</span>를 알아야 한다

3. 변수 간 관계식 만들기 ( 점화식 )

4. 메모하기(memoization or tabulation)
    - 변수 값에 따른 결과 저장 <span style="color:#9fb584">**Memoization**</span>

5. 기저 상태 파악하기
    - 가장 작은 문제의 상태를 알아야 한다
    - 피보나치 수열 f(0) = 0, f(1) = 1
    - <span style='background-color:#c8d8b4'>해당 기저 문제에 대해 파악 후 미리 저장</span>

6. 구현하기
    - Bottom-Up (Tabulation 방식) - 반복문 사용 
    - Top-Down (Memoization 방식) - 재귀 사용

<br/> 

### [ DP 구현 ]

- Bottom-Up 방식
: 밑에서 부터 계산을 수행 하고 누적시켜 전체 큰 문제를 해결하는 방식이다<br />
dp[0]부터 시작하여 반복문을 통해 점화식으로 결과를 내서 dp[n]까지 그 값을 전이시켜 재활용

> Tabulation
> : 반복을 통해 dp\[0](기저 상태) 부터 하나 하나씩 채우는 과정을 table-filling 이라 하며,<br /> 이 테이블에 저장된 값에 직접 접근하여 재활용하므로 **Tabulation**이라고 한다<br />결과값을 기억하고 재사용한다는 측면에서 Memoization과 크게 다르지 않다.

<br/> 

- Top-Down 방식
 : dp[0]의 기저 상태에서 출발하지 않고 dp[n]의 값을 찾기 위해 위에서 부터 호출을 시작해 dp[0]의 상태까지 내려간 다음 해당 결과 값을 재귀를 통해 전이시켜 재활용<br />- 이전에 해당 문제 계산을 완료한 경우, 메모리에 저장된 값을 꺼내 활용<br /> <span style='background-color:#c8d8b4'>=> 가장 최근 상태 값을 메모해 둠 Memoization</span>

<br/>
<br />

## DP 사용해 해결한 문제

---

#### Programmers - 스티커 모으기

\- n개의 스티커가 원형 형태로 부착되어 있다고 가정할때 원 안의 스티커 중 일부를 떼어내 스티커에 적힌 숫자의 최대 합을 구하는 문제<br />스티커의 원형 형태를 유지하기 위해 첫 번째 요소와 마지막 요소가 서로 연결되어 있다고 가정한다.<br />스티커를 떼어내면 해당 스티커 양쪽의 스티커는 쓸 수 없다.

> 💡 0번째와 1번째 스티커를 뜯는 경우의 합을 계산해서 최대 합 구하기 <br />

<br />

```swift
import Foundation

func solution(_ sticker:[Int]) -> Int{
    let cnt = sticker.count
    if cnt < 3 {
        return sticker.max()!
    }
    var dp = Array(repeating: 0, count: cnt)
    var anw = 0

    // 0번째 스티커 뜯을 경우의 기저 상태
    dp[0] = sticker[0]
    dp[1] = sticker[0]

    for i in 2..<cnt - 1 {
        // 점화식
        dp[i] = max(dp[i - 1], dp[i - 2] + sticker[i])
    }
    anw = dp[cnt - 2]

    // 1번째 스티커 뜯을 경우의 기저 상태
    dp[0] = 0
    dp[1] = sticker[1]

    for i in 2..<cnt {
        dp[i] = max(dp[i - 1], dp[i - 2] + sticker[i])
    }
    return max(anw, dp[cnt - 1])
}
```

- 배운점
  : 현재의 최적 합을 이용해 전체 문제의 최적 합을 구할 수 있기 때문에 dp를 사용해서 문제를 해결했다. 스티커가 원형이고 해당 스티커를 뜯을 경우 양쪽 스티커를 사용할 수 없기 때문에 0번째 스티커를 뜯는 경우, 1번째 스티커를 뜯는 경우를 나눠서 계산한 후 최대합을 찾았다. (0번째 스티커를 뜯을 경우 cnt - 1번째 스티커 사용할 수 없음)

<br />

---

#### Programmers - 연속 펄스 부분 수열의 합

\- 어떤 수열의 연속 부분 수열에 같은 길이의 펄스 수열을 각 원소끼리 곱해 연속 펄스 부분 수열을 만들 때 연속 펄스 부분 수열의 합 중 가장 큰 것을 구하는 문제<br /> 펄스 수열이란 [-1,1,-1,1...], [1,-1,1,-1...] 과 같이 1 또는 -1 로 시작하며 1과 -1이 번갈아 나오는 수열이다.

> 💡 1, -1로 시작하는 펄스를 곱하여 수열의 합을 구하는 dp배열을 만들고 최대합 구하기 <br />

<br />

```swift
import Foundation

func solution(_ sequence:[Int]) -> Int64 {
    let cnt = sequence.count
    var dp1 = Array(repeating: 0, count: cnt)
    var dp2 = Array(repeating: 0, count: cnt)
    var pulse1 = 1, pulse2 = -1

    dp1[0] = sequence[0] * pulse1
    dp2[0] = sequence[0] * pulse2

    for i in 1..<cnt {
        pulse1 *= -1
        pulse2 *= -1
        // 점화식
        dp1[i] = max(dp1[i - 1] + (sequence[i] * pulse1), (sequence[i] * pulse1))
        dp2[i] = max(dp2[i - 1] + (sequence[i] * pulse2), (sequence[i] * pulse2))
    }

    return Int64(max(dp1.max()!, dp2.max()!))
}
```

- 배운점
  : 펄스 수열이 1, -1 두 가지로 시작할 수 있기 때문에 dp배열을 2개 만들어 계산했다. **연속 부분 수열**의 합을 구하는 문제이기 때문에 i번째 원소를 i - 1번째 원소에 더했을 때와 i번째 원소의 크기를 비교하여 더 큰 값을 dp[i]에 저장하는 점화식을 사용했다.

<br />

---

#### Programmers - 풍선 터트리기

\- 일렬로 나열된 N개의 서로 다른 숫자가 쓰인 풍선이 있다. 아래의 과정을 반복하며 풍선이 1개만 남을 때까지 계속 터트릴 때 최후까지 남는 것이 가능한 풍선의 개수를 구하는 문제<br />1. 임의의 인접한 두 풍선을 고른 뒤, 두 풍선 중 하나를 터트린다.<br />2. 터진 풍선으로 인해 빈 공간이 생길 시, 빈 공간이 없도록 풍선들을 중앙으로 밀착시킨다.<br />3. 인접한 풍선 중 번호가 더 작은 풍선을 터트리는 행위는 1번만 할 수 있다. 

> 💡 마지막 풍선이 될 수 있는 조건 : 좌우 방향 중 한쪽에만 작은 숫자가 존재할 경우 (작은 풍선은 1번만 터트릴 수 있음)<br />

<br />

```swift
import Foundation

func solution(_ a:[Int]) -> Int {
    let cnt = a.count
    var leftDp = Array(repeating: 0, count: cnt)
    var rightDp = Array(repeating: 0, count: cnt)
    var anw = 0

    leftDp[0] = a[0]
    rightDp[cnt - 1] = a[cnt - 1]

    // 오른쪽에서부터 최솟값 저장
    for i in stride(from: cnt - 2, to: -1, by: -1) {
        rightDp[i] = min(a[i], rightDp[i + 1])
    }

    // 왼쪽에서부터 최솟값 저장
    for i in 1..<cnt {
        leftDp[i] = min(a[i], leftDp[i - 1])
    }

    for i in 0..<cnt {
        // 한쪽이라도 i번째 풍선이 최소 값이라면 마지막까지 살아남을 수 있음
        if a[i] <= leftDp[i] || a[i] <= rightDp[i] {
            anw += 1
        }
    }
    return anw
}
```

- 배운점
  : 풍선이 마지막까지 살아남으려면 한쪽 방향에만 자신보다 작은 풍선이 있어야 한다. 왼쪽, 오른쪽에서 시작하는 dp배열을 만들어 i번째 풍선과 그전의 풍선을 비교해 더 작은 풍선을 저장한다. 반복문을 돌며 한쪽이라도 i번째 풍선이 최솟값이라면 해당 풍선은 살아남을 수 있다. 풍선이 살아남을 수 있는 조건을 생각하는 게 어려웠다..

<br />
<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
