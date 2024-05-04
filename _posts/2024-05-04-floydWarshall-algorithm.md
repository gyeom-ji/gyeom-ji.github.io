---
title: "플로이드 와샬 알고리즘 Floyd-Warshall Algorithm"
author: gyeomji
date: 2024-05-04 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Floyd-Warshall]
pin: false
math: true
mermaid: true
---

<br/> 
<span style="color:#9fb584">**최단 경로 찾기 알고리즘**</span>으로 음수 순환 사이클이 없는 그래프에서 <span style='background-color:#c8d8b4'>모든 지점에서 다른 모든 지점까지의</span> <span style="color:#9fb584">**최단거리 및 경로를 계산**</span>하는 알고리즘이다. 방향 그래프와 무방향 그래프 모두 가능하며, 음의 가중치를 가지는 간선이 있어도 되지만, <span style="color:#9fb584">**합이 음수 가중치를 가지는 사이클이 있는 경우 작동하지 않는다.**</span>

<br/> 


## Dijkstra VS Floyd-Warshall

---

1. 경로 지점
    - 다익스트라의 경우 한 지점에서 다른 특정 지점까지의 최단 경로를 구한다.
    - 플로이드 와샬은 <span style="color:#9fb584">**모든 지점에서 다른 모든 지점까지의 최단 경로**</span>를 구한다. 
2. 노드 선택, 최단 거리 테이블 갱신 방법
    - 다익스트라의 경우 단계마다 최단 거리를 가지는 노드를 하나씩 반복적으로 선택하고, 해당 노드를 거쳐가는 경로를 확인하며 최단 거리 테이블을 갱신하는 방식으로 동작한다.
   - 플로이드 와샬은 단계마다 <span style="color:#9fb584">**거쳐 가는 노드를 기준**</span>으로 알고리즘을 수행하지만, 매 단계마다 방문하지 않은 노드 중에서 최단 거리를 가지는 노드를 찾을 필요가 없다.
3. 최단 거리 테이블
   - 다익스트라는 한 지점에서 다른 지점까지의 최단 거리이기 때문에 1차원 리스트에 최단 거리 정보를 저장한다.
   - 플로이드 와샬은 모든 지점에서 다른 모든 지점까지의 최단 거리를 저장해야 하기 때문에 <span style="color:#9fb584">**2차원 테이블에 최단 거리 정보를 저장**</span>한다.
4. 알고리즘
   - 플로이드 와샬은 `DP 알고리즘`에 속한다.
     - 노드의 개수가 N개라 할 때, N번의 단계를 반복하며 점화식에 맞게 2차원 리스트를 갱신한다.
     - <span style="color:#9fb584">**distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])**</span>
   - 다익스트라는 그리디 알고리즘에 속한다.

<br/>
<br/>


### Floyd-Warshall 알고리즘

---

- O(n^3)의 시간 복잡도를 가진다.
  - 노드의 개수가 n개 일 때, n번의 단계를 수행하며 단계마다 O(n^2)의 연산을 통해 <span style="color:#9fb584">**현재 노드를 거쳐 가는 모든 경로**</span>를 고려한다.
- O(n^2)의 공간 복잡도를 가진다.


### Floyd-Warshall 알고리즘 구현

1. 2차원 최단 거리 테이블을 각 간선의 가중치로 초기화한다. 
   1. 가중치가 없는 경우 Int.max로 설정한다.
   2. i == j일 경우 0으로 설정한다.
2. 각 중간 정점 K에 대해 시작 정점 i와 도착 정점 j의 가능한 모든 조합을 반복문으로 탐색한다.
3. (현재 거리 > 중간 정점 k를 거친 거리)일 경우 distance를 업데이트 한다.
4. 그래프의 모든 정점에 대해 2, 3번을 반복한다.


<br/>

``` swift 
func floydWarshall() {
    let count = graph.count
    // 초기화
    var distance = Array(repeating: Array(repeating: Int.max, count: count), count: count)
    for i in 0..<count {
        for j in 0..<count {
            if i == j {
                distance[i][j] = 0
            }
        }
    }
    
    // 간선 정보 입력
    for i in 0..<count {
        for edge in graph[i] {
            distance[i][edge.adjvertex] = min(edge.weight, distance[i][edge.adjvertex])
        }
    }
    
    // k = 거쳐가는 노드
    for k in 0..<count{
        // i = 출발 노드
        for i in 0..<count {
            // j = 도착 노드
            for j in 0..<count {
                if distance[k][j] == Int.max || distance[i][k] == Int.max { continue }
                distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])
            }
        }

    }
}

```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
