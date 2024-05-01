---
title: "너비 우선 탐색 BFS"
author: gyeomji
date: 2024-04-10 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, BFS, 너비우선탐색]
pin: false
math: true
mermaid: true
---

<br/> 
트리나 그래프를 탐색하는 기법 중 하나로 가중치가 없는 그래프에서 <span style="color:#9fb584">**최단거리를 구하거나 임의의 경로**</span>를 찾는 것이 목표일때 유용하다.<br />
시작 노드에서 자식 노드들을 순서대로 탐색하며 <span style='background-color:#c8d8b4'>너비를 우선으로 탐색하는 알고리즘</span>이다.<br /> 
같은 거리에 있는 모든 노드를 탐색한 후 다음 거리의 노드를 탐색하는 방법으로 <span style="color:#9fb584">**Queue**</span>를 사용하여 구현한다. 

> 💡  <span style="font-size: 15px"> BFS는 노드 수가 많은 그래프에는 적합하지 않으며, 가중 간선이 있는 그래프에서는 제대로 작동하지 않는다.</span>

<br/> 
<br/>

## BFS 장단점

---
 [ 장점 ]

1. 출발노드에서 목표노드까지의 <span style="color:#9fb584">**최단경로를 보장 한다.**</span>
2. 노드 수가 적고 깊이가 얕은 해가 존재할 때 유리하다.
<br />

 [ 단점 ]

1. 큐를 이용하여 다음에 탐색할 정점들을 저장하므로 더 큰 저장공간이 필요하다.
    - <span style="color:#9fb584">**큰 그래프에는 적합하지 않다.**</span>
2. 그래프를 순회할 때 가중치를 고려하지 않기 때문에 <span style="color:#9fb584">**가중치 간선이 있는 그래프에는 적합하지 않다.**</span>
3. 목표 노드가 시작 노드에서 멀리 떨어져 있을 경우, 다음 깊이로 이동하기 전 주어진 깊이의 모든 노드를 탐색하기 때문에 비효율적이다.


<br/>
<br/>

## BFS 구현

---

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리
2. 큐에서 노드를 꺼낸 후 해당 노드의 인접 노드 중 방문하지 않은 노드를 모두 큐에 삽입 후 방문 처리
3. 2번 반복

<br/> 

```swift
func bfs(_ graph: [[Int]], _ start: Int) {
    var queue : [Int] = [start]
    visited[start] = true

    while !queue.isEmpty {
        let now = queue.removeFirst()
        for i in graph[now] {
            if !visited[i] {
                queue.append(i)
                visited[i] = true
            }
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
    - 인접 리스트에 비해 **간선이 많을 경우** 효율적이다.
>
>
>- **인접 리스트 : O(n+e)**
    - 연결된 정보(간선의 개수)만 저장하기 때문에 메모리를 효율적으로 사용할 수 있다.
    - 두 노드의 연결 관계를 확인할 경우 O(n)
    - **모든 노드를 방문할 경우** O(n+e)
    - **간선이 적을 경우** 효율적이다.

<br/> 
<br/> 


## BFS 사용해 해결한 문제

---

#### Programmers - 리코쳇 로봇

\- 격자모양 게임판 위에서 시작 위치에서 목표 위치까지 최소 도달 횟수를 구하는 문제<br /> 말을 상하좌우 중 한 방향을 선택해 장애물이나 벽에 부딪힐 때까지 미끄러져 이동하는 것을 한 번의 이동으로 친다.

> 💡 **map과 방향이 나올 경우 dx, dy 값 생성**

<br />

```swift
import Foundation

func solution(_ board:[String]) -> Int {
    let board = board.map{ $0.map{ String($0) } }
    var visited = Array(repeating: Array(repeating: false, count: board[0].count), count: board.count)
    let rmax = board.count - 1, cmax = board[0].count - 1
    let dy = [-1,1,0,0], dx = [0,0,-1,1]
    
    func findStart() -> (Int, Int) {
        for i in 0...rmax {
            for j in 0...cmax {
                if board[i][j] == "R" {
                    return (i,j)
                }
            }
        }
        return (-1,-1)
    }

    func isPossible(_ r: Int, _ c: Int) -> Bool {
        return r >= 0 && r <= rmax && c >= 0 && c <= cmax && board[r][c] != "D"
    }

    func bfs(_ start: (Int, Int)) -> Int {
        var queue : [(Int, Int, Int)] = [(start.0, start.1, 0)]
        visited[start.0][start.1] = true
        
        while !queue.isEmpty {
            let (r,c,count) = queue.removeFirst()

            if board[r][c] == "G" {
                return count
            }
            
            for i in 0..<4 {
                var nr = r, nc = c
                
                while isPossible(nr + dy[i], nc + dx[i]) {
                    nr += dy[i]
                    nc += dx[i]
                }

                if !visited[nr][nc] {
                    queue.append((nr, nc, count + 1))
                    visited[nr][nc] = true
                }
            }
        }
        return -1
    }

    let start = findStart()
    return bfs(start)
}

```

- 배운점
  : <span style="color:#9fb584">**최소 거리**</span>를 구하는 문제로 bfs를 사용해야 한다는 걸 알았지만 어떻게 풀어 나가야 할지 떠오르지 않아서 풀이하는데 시간이 오래 걸렸다.. 결국 힌트를 얻어서 문제를 해결했는데, 다음에 <span style="color:#9fb584">**map과 방향을 사용하는 문제가 나오면 dx, dy를 사용**</span>해 풀이 해봐야겠다! 


<br />

---

#### Programmers - 가장 먼 노드

\- n개의 노드가 있는 그래프가 있을때, 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하는 문제<br /> 가장 멀리 떨어진 노드 = 최단 경로로 이동 했을 경우 간선의 개수가 가장 많은 노드

> 💡 인접리스트와 최대간선(50000 + 1)크기의 배열을 만든다.<br />bfs로 노드를 탐색하며 array[1번 노드 -> 해당 노드 간선 개수] += 1 을 해줘 최대 간선 개수 위치의 노드 수를 구한다.

<br />

```swift
import Foundation

func solution(_ n:Int, _ edge:[[Int]]) -> Int {
    var graph = Array(repeating: Array(repeating:0, count: 0), count: n + 1)
    var maxCnt = 0
    var edgeArr = Array(repeating: 0, count: 50001)
    var visited = Array(repeating: false, count: n + 1)

    for i in 0..<edge.count {
        let nodes = edge[i]
        graph[nodes[0]].append(nodes[1])
        graph[nodes[1]].append(nodes[0])
    }

    func bfs() {
        var queue : [(Int, Int)] = [(1,0)]
        visited[1] = true

        while !queue.isEmpty {
            let (now, cnt) = queue.removeFirst()
            maxCnt = max(maxCnt, cnt)
            edgeArr[cnt] += 1

            for element in graph[now] {
                if !visited[element] {
                    queue.append((element, cnt + 1))
                    visited[element] = true
                }
            }
        }
    }

    bfs()
    return edgeArr[maxCnt]
}

```

- 배운점
  : <span style="color:#9fb584">**최단경로로 모든 노드를 방문해야하는 경우 인접리스트를 사용해 bfs로 풀기**</span>

<br />

---

#### Programmers - 부대복귀

\- 여러 지역에 흩어져 있는 부대원이 부대로 복귀할 수 있는 최단 시간을 담은 배열을 구하는 문제<br /> 주어진 sources 원소 순서대로 배열을 반환해야하며, 적군의 방해로 인해 복귀가 불가능한 부대원은 -1 

> 💡 **destination을 시작 노드로 하여 각 노드들까지 최단 시간 구하기**

<br />

```swift
import Foundation

func solution(_ n:Int, _ roads:[[Int]], _ sources:[Int], _ destination:Int) -> [Int] {
    var graph = Array(repeating: [Int](), count: n + 1)
    var anw = Array(repeating: -1, count: n + 1)

    for road in roads {
        graph[road[0]].append(road[1])
        graph[road[1]].append(road[0])
    }
    anw[destination] = 0

    func bfs() {
        var queue = [(destination, 0)]

        while !queue.isEmpty {
            let (now, cnt) = queue.removeFirst()

            for element in graph[now] {
                if anw[element] == -1 {
                    queue.append((element, cnt + 1))
                    anw[element] = cnt
                }
            }
        }
    }

    bfs()
   
    return sources.map {anw[$0]}
}

```

- 배운점
  : 처음엔 sources를 돌며 각 지역에서 destination까지의 최단시간을 구해서 시간초과가 났다. 시간초과를 어떻게 해결할까 고민하다 거꾸로 destination에서 각 지역까지의 최단시간을 구하니 시간초과를 해결할 수 있었다. 다시 생각해보면  destination에서 각 지역까지 bfs 한번으로 최단시간을 구할 수 있는 문제인데 왜 바로 떠올리지 못했는지 모르겠다.. 생각의 전환이 중요한것 같다!! 다음에는 반대로도 생각해보기!
  
<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
