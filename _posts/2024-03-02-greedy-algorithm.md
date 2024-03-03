---
title: "탐욕 알고리즘 Greedy Algorithm"
author: gyeomji
date: 2024-03-02 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, 탐욕]
pin: true
math: true
mermaid: true
---

<br/> 
선택의 순간마다
<span style='background-color:#c8d8b4'> 눈앞에 보이는 가장 큰 이익</span>
만 쫓는 방법  
선택은 그 순간 지역적으로는 최적이지만, **최종적인 해답이 최적이라는 보장은 없다**  
하지만, **계산 속도**가 빠르기 때문에 탐욕 알고리즘을 적용할 수 있는 문제에서는 최적해를 빠르게 산출할 수 있다

> 💡 <span style="font-size: 15px">탐욕 알고리즘은 항상 최적의 결과를 도출하는 것은 아니지만, 어느 정도 최적에 근사한 값을 빠르게 도출할 수 있다<br /> 　 　지역적으로 최적이며 전역적으로도 최적인 문제에 적용</span>

<br/> 
<br/>

## 탐욕 알고리즘 문제 해결 방법

---

- **선택 절차 Selection Procedure** : 현 상태에서 최적의 해답을 선택
- **적절성 검사 Feasibility Check** : 선택된 해가 문제의 조건을 만족하는지 검사
- **해답 검사 Solution Check** : 문제가 해결되었는지 검사하고 해결되지 않았다면 위의 과정 반복

<br/>
<br/>

## 탐욕 알고리즘 성립 조건

---

1. 탐욕 선택 조건 Greedy choice property

   : 한번의 선택이 다음 선택에 영향을 주지 않아야 한다.

2. 최적 부분 구조 조건 Optimal Substructure

   : 문제에 대한 최종 해결 방법이 부분 문제에 대해서도 최적 문제 해결 방법이여야 한다.

<br/>
<br/>

## 탐욕 알고리즘 사용해 해결한 문제

---

#### Programmers - 조이스틱

\- 조이스틱을 최소한으로 조작하여 문자열을 변환하는 문제<br />

> 💡 오른쪽, 왼쪽 기준점을 정하고, A 발견 시 왕복 최소 거리 구하기

<br />

```swift
import Foundation

func solution(_ name:String) -> Int {
    let name = name.utf8.map {min(Int($0) - 65, 90 - Int($0) + 1)}
    let cntAlpa = name.reduce(0, +)
    var cntMove = name.count - 1

    for right in 0..<name.count {
        var left = right + 1
        while left < name.count && name[left] == 0 {
            left += 1
        }
        // 왕복 최소 거리
        let compareDis = min(right , name.count - left)
        // 최소 거리 최소값
        cntMove = min(cntMove, right + (name.count - left) + compareDis)
    }

    return cntMove + cntAlpa
}
```

- 배운점
  : `utf8` 프로퍼티 사용시 ascii 코드 값을 쉽게 얻을 수 있다 <br />
  처음에는 오른쪽으로 간 후 되돌아와 반대쪽으로 가는 방법이 최소 거리가 될 수 있다는 생각을 못해서 헤맸다 <br />
  다음에 이런 유형의 문제를 풀게 된다면 테스트 케이스를 여러개 만들어 체크해가면서 해결해야겠다!

  ![joystick](/assets/img/joystick.jpeg){: width="300" height="200"}
  _( 위의 예시처럼 문자열 중간에 A가 많을 경우 되돌아 가는 것이 최소 거리가 될 수 있다 )_

<br />

---

#### Programmers - 체육복

\- 여벌 체육복을 가진 학생이 도난 당한 학생에게 체육복을 빌려줄 때 수업을 들을 수 있는 학생의 최댓값을 구하는 문제<br />
\- 앞 뒤 번호에게만 체육복을 빌려 줄 수 있다<br />
\- 여벌 체육복을 가진 학생이 체육복을 도난 당했을 경우 다른 학생에게 체육복을 빌려줄 수 없다

> 💡 여벌 학생 목록, 도난 학생 목록에 공통적으로 존재하는 학생 제거하기 <br /> 　 　여벌 학생, 도난 학생 오름차순 정렬하기

<br />

```swift
import Foundation

func solution(_ n:Int, _ lost:[Int], _ reserve:[Int]) -> Int {
var lostFilter = lost.filter{ !reserve.contains($0)}.sorted()
var reserveFilter = reserve.filter{ !lost.contains($0)}.sorted()
var start = 0, resCount = reserveFilter.count
var anw = n - lostFilter.count

    for lost in lostFilter {
        for i in start..<resCount where reserveFilter[i] < lost + 2 {
            if lost == reserveFilter[i] - 1 || lost == reserveFilter[i] + 1 {
                anw += 1
                start = i + 1
                break
            }
        }
    }
    return anw
}
```

- 배운점
  : `filter` `contains` 을 사용해 중복값을 제거한 데이터를 추출할 수 있다 <br />

<br />

---

#### Programmers - 큰 수 만들기

\- 어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하는 문제<br />

> 💡 (숫자 길이 - k개)까지 스택에 넣으면서 스택의 마지막 값이 현재 값보다 작을 때까지 k개 제거하기

<br />

```swift
import Foundation

func solution(_ number:String, _ k:Int) -> String {

    var number: [Character] = Array(number)
    var stack = [Character]()
    var k = k
    var count = number.count - k

    for (i, num) in number.enumerated() {
        while !stack.isEmpty && stack.last! < num && k > 0 {
            stack.removeLast()
            k -= 1
        }
        if stack.count < count {
            stack.append(num)
        }

    }
    return String(stack)
}
```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
