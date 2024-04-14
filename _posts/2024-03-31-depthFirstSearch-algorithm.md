---
title: "깊이 우선 탐색 DFS"
author: gyeomji
date: 2024-03-31 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, DFS, 깊이우선탐색]
pin: false
math: true
mermaid: true
---

<br/> 
트리나 그래프를 탐색하는 기법 중 하나로 특정 정점 또는 모든 정점에 <span style="color:#9fb584">**최대한 빨리 도달하는것이 목표**</span>일때 유용하다.<br />
시작 노드에서 자식 노드들을 순서대로 탐색하며 <span style='background-color:#c8d8b4'>깊이를 우선으로 탐색하는 알고리즘</span>이다.<br /> 
깊이를 우선시해 모든 경우의 수를 탐색하기 때문에 완전 탐색 알고리즘에 속하기는 하지만, 항상 완전탐색으로 사용되지는 않는다. 주로 <span style="color:#9fb584">**반복문 (stack)**, **재귀문**</span>으로 구현된다. 

> 💡  <span style="font-size: 15px"> DFS는 노드 수가 많고, 경로의 특징(가중 간선이 있는 그래프)을 저장해야 하는 경우 유용하다.</span>

<br/> 
<br/>

## DFS 장단점

---
 [ 장점 ]

1. 현재 경로상 노드들만 저장하는 스택 구조를 사용하기 때문에 BFS에 비해 메모리 공간을 적게 차지한다.
2. 목표 노드가 깊은 단계에 있는 경우 해를 빨리 구할 수 있다.
3. 그래프에서 <span style="color:#9fb584">**순환을 감지**</span>할 수 있다.
<br />

 [ 단점 ]

1. 단순 검색 속도가 BFS 보다 느리다.
2. 해가 없는 경로에 깊이 빠질 수 있다. (스택오버플로우)
    - 사전에 탐색할 임의의 깊이를 지정
3. <span style="color:#9fb584">**최단 경로의 정점을 찾는다는 보장을 할 수 없다.**</span>
    - 목표에 이르는 경로가 다수인 문제인 경우 해를 구할 시 탐색이 종료되기 때문


<br/>
<br/>

## DFS 구현

---

[ 반복 구현 ( Stack ) ]
- 반복 구현에서는 스택을 사용해 방문할 정점 추적

1. 임의의 정점에서 시작해 해당 정점을 방문 처리하고 스택에 넣음
2. 스택에서 맨 위 정점을 가져옴
3. 방문하지 않은 모든 인접 정점을 방문하고 방문 처리 후 스택에 넣음
4. 스택이 비워질 때까지 반복

<br/> 

```swift
func dfs(_ graph: [[Int]], _ start: Int) {
    var stack : [Int] = [start]
    visited[start] = true

    while !stack.isEmpty {
        let now = stack.removeLast()
        for i in graph[now] {
            if !visited[i] {
                stack.append(i)
                visited[i] = true
            }
        }
    }
}
```

<br/>

[ 재귀 구현 ]
- 모든 정점을 방문하는 경우 유용

<br/> 

```swift
func dfs(_ graph: [[Int]], _ start: Int){
    visited[start] = true

    for i in graph[start] {
        if !visited[i] {
            dfs(graph, i)
        }
    }
}
```
<br/> 

>[ 시간 복잡도 ]
>: 노드 수가 많은 경우 인접리스트를 사용하여 시간 복잡도를 줄일 수 있다.
>
>- **인접 행렬 : O(n^2)**
    - 노드의 수가 많을 수록 메모리 사용량이 증가한다.
    - 두 노드의 연결 관계를 확인할 경우 O(1)
    - 모든 노드를 방문할 경우 O(n^2)
    - 노드에 비해 **간선이 많을 경우** 효율적이다.
>
>
>- **인접 리스트 : O(n+e)**
    - 연결된 정보(간선의 개수)만 저장하기 때문에 메모리를 효율적으로 사용할 수 있다.
    - 두 노드의 연결 관계를 확인할 경우 O(n)
    - **모든 노드를 방문할 경우** O(n+e)
    - **간선이 적을 경우** 효율적이다.

<br/> 
<br/> 


## DFS 사용해 해결한 문제

---

#### Programmers - 타겟 넘버

\- n개의 음이 아닌 정수들이 있을 때 순서를 바꾸지 않고 + - 하여 타겟 넘버를 만드는 방법의 수를 구하는 문제<br />

> 💡 모든 경우의 수에 + - 연산 수행해 타겟 넘버가 되는 횟수를 구한다 <br />

<br />

```swift
import Foundation

func solution(_ numbers:[Int], _ target:Int) -> Int {
    var anw = 0
    var plusMinus: [Int] = [1, -1]
    
    func dfs(_ start: Int, _ max: Int,  _ num: Int, _ i: Int) {
        if start == max{
            if num == target {
                anw += 1
            }
            return
        }
        for value in plusMinus {
            dfs(start + 1, max, num + (numbers[i] * value), i + 1)
        }
    }
    
    dfs(0, numbers.count, 0, 0)
    
    return anw
}
```

- 배운점
  : 처음 문제를 봤을 때 dfs를 이용하는 문제라는걸 생각하지 못해 헤맸던 것 같다.. 문제를 많이 풀며 감을 익혀야겠다.

<br />

---

#### Programmers - 네트워크

\- 컴퓨터 A 와 B 가 연결되어 있고, B 와 C 가 연결되어 있을 때 A B C 는 같은 네트워크 상에 있다. 컴퓨터의 개수와 연결 정보가 주어졌을 때 이에 대한 네트워크의 개수를 구하는 문제<br />

> 💡 반복문을 돌며 방문하지 않은 정점이 있을 때마다 dfs로 순회 <br />

<br />

```swift
import Foundation

func solution(_ n:Int, _ computers:[[Int]]) -> Int {
    var anw = 0
    var visited = Array(repeating: false, count: n)

    func dfs(_ start: Int) {
        visited[start] = true

        for i in 0..<n {
            // 방문하지 않은 연결되어 있는 컴퓨터
            if !visited[i] && computers[start][i] == 1 {
                dfs(i)
            }
        }
    }
    
    for i in 0..<n {
        if !visited[i] {
            dfs(i)
            anw += 1
        }
    }
    
    return anw
}
```

- 배운점
  : <span style="color:#9fb584">**모든 정점들이 연결되지 않은 경우 반복문을 돌며 방문하지 않은 정점을 볼 때마다 dfs 시행**</span> <br />
  (방문 시도하는 횟수가 컴포턴트의 개수)

<br />

---

#### Programmers - 단어 변환

\- 두 개의 단어 begin, target과 단어의 집합 words가 있을 때 규칙을 이용해 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾는 문제<br />
규칙 : 한 번에 한 개의 알파벳만 바꿀 수 있으며, words에 있는 단어로만 변환할 수 있다.<br />

<br />

```swift
import Foundation

func solution(_ begin:String, _ target:String, _ words:[String]) -> Int {
    if !words.contains(where: { element in
        if target == element {
            return true
        }
        return false
    }) {
        return 0
    }
    
    var anw = Int.max, wordCnt = words[0].count
    var visited = Array(repeating: false, count: words.count)
    
    func isTranslate(_ before: [Character], _ after: [Character]) -> Bool {
        var count = 0

        for i in 0..<wordCnt {
            if before[i] == after[i] {
                count += 1
            }
        }
        return count == wordCnt - 1
    }

    func dfs(_ start: Int,  _ now: String) {
        if now == target {
            anw = min(anw, start)
            return
        }

        for i in 0..<words.count where !visited[i]{
            if isTranslate(Array(words[i]), Array(now)) {
                // 더 짧은 경로의 값을 구할 수도 있음
                visited[i] = words[i] == target ? false : true
                dfs(start + 1, words[i])

            }
        }
    }

    dfs(0, begin)
    
    return anw
}
```

- 배운점
  : 더 짧은 경로의 값을 구할 수도 있기 때문에 words[i] == target인 경우 visited[i] 를 false로 처리

<br />

---

#### Programmers - 여행 경로

\- 주어진 항공권을 모두 이용해 여행 경로를 짜려고 할때 방분하는 공항 경로를 배열에 담아 반환하는 문제<br />
항상 ICN 공항에서 출발하며 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 반환<br />

> 💡 알파벳 순서가 앞서는 경로를 반환 -> 정렬 처리 후 시작<br />
<br />

```swift
func solution(_ tickets:[[String]]) -> [String] {
    var graph = [String: [(String, Int)]]()
    var anw = ["ICN"]
    var count = tickets.count
    var tickets = tickets.sorted(by: {$0[1] < $1[1]})
    var visited = Array(repeating: false, count: tickets.count)
    
    for i in 0..<count {
        if let exist = graph[tickets[i][0]] {
            graph[tickets[i][0]]!.append((tickets[i][1], i))
        } else {
            graph[tickets[i][0]] = [(tickets[i][1], i)]
        }
    }

    func dfs(_ to: String) {
        if anw.count == count + 1 { return }
        
        guard let current = graph[to] else {return}
        
        for cur in current where !visited[cur.1]{
            anw.append(cur.0)
            visited[cur.1] = true
            dfs(cur.0)
            // 모든 도시 방문 종료
            if anw.count == count + 1 { return }
            // 해당 경로로 모든 도시 방문 X 방문 취소 경로 마지막 값 삭제
            anw.removeLast()
            visited[cur.1] = false
        }
    }
    dfs("ICN")
    return anw
}

```

- 배운점
  : <span style="color:#9fb584">**해당 경로로 모든 도시를 방문할 수 없을 경우 방문 취소, 경로의 마지막 값을 삭제**</span>해주는 것을 생각하지 못해 시간이 오래 걸렸던 것 같다. 비슷한 유형의 문제가 나오면 바로 해결할 수 있을 정도로 다시 풀어봐야겠다!

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
