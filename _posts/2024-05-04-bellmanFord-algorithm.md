---
title: "벨만포드 알고리즘 Bellman-Ford Algorithm"
author: gyeomji
date: 2024-05-04 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Bellman-Ford]
pin: false
math: true
mermaid: true
---

<br/> 
<span style="color:#9fb584">**최단 경로 찾기 알고리즘**</span>으로 <span style='background-color:#c8d8b4'>음수 가중치 가중 그래프에서도</span> 출발점으로부터 각 정점까지의 <span style="color:#9fb584">**최단거리 및 경로를 계산**</span>하는 알고리즘이다. 단, 입력 그래프에 싸이클 상 간선들의 <span style="color:#9fb584">**가중치 합이 0보다 작은 음수 싸이클이 없어야한다.**</span> 다익스타는 방문하지 않은 정점 중 최단 거리가 가장 가까운 정점만을 방문하지만, <span style="color:#9fb584">**벨만포드는 매 단계마다 모든 정점을 방문**</span>한다. 따라서, 음의 가중치를 가지는 간선이 있어도 최적의 해를 구할 수 있지만, 다익스트라보다 시간이 오래 소요된다.

<br/> 

>💡  <span style="font-size: 15px">가중치 합이 0보다 작은 음수 싸이클이 없어야 하는 이유<br /> 　 　음수 싸이클을 반복할수록 경로의 길이가 더 짧아지는 모순 발생</span>


<br/>

## Bellman-Ford 알고리즘 - 인접리스트

---

- O(VE)의 시간 복잡도를 가진다.
  - 간선 완화 O(E) 총 V-1번 수행

### Bellman-Ford 알고리즘 구현

1. distance를 초기화 시키고, 출발 정점을 설정한다.
2. 다음 과정을 V-1번 반복한다.
   1. 모든 간선 E개를 하나씩 확인한다.
   2. 각 간선을 거쳐 다른 노드로 가는 비용을 계산하여 distance를 갱신한다.
3. 만약 음수 간선 순환이 발생하는지 체크하고 싶다면 2번 과정을 한 번 더 수행한다.
   1. 이때 distance가 갱신된다면 음수 간선 순환이 존재하는 것이다.
   2. 음의 사이클이 존재할 경우 최단 경로가 존재하지 않는다고 결론 짓는다.

<br/>
> V-1번 반복하는 이유 : v개의 정점과 e개의 간선이 있는 가중 그래프에서 **정점 A에서 정점B까지의 최단 거리는 최대 v-1개의 정점을 지나기 때문**이다. v-1개 이상의 정점을 방문하는 것은 **중복 방문**하는 것이기 때문에 최단 경로가 성립될 수 없다.

<br/>

``` swift 

struct Edge: Comparable, CustomStringConvertible {
    let adjvertex : Int
    let weight : Int
    
    init(adjvertex: Int, weight: Int) {
        self.adjvertex = adjvertex
        self.weight = weight
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
           lhs.weight < rhs.weight
    }
    
    var description: String {
        return "(\(adjvertex), \(weight))"
    }
}


struct BellmanFord {
    private var count = 0
    private var prev = [Int]()
    private var graph = [[Edge]]()
    
    init(graph: [[Edge]]) {
        self.graph = graph
        self.count = graph.count
    }
    
    func printShortestPath() {
        for i in 1..<count {
            var back = i
            print(back, terminator: " ")
            while back != 0 {
                print(" <- \(prev[back])", terminator: " ")
                back = prev[back]
            }
            print()
        }
    }
    
    mutating func bellmanFord(_ start: Int) -> [Int] {
        // 초기화
        var distance = Array(repeating: Int.max, count: count)
        prev = Array(repeating: -1, count: count)
        
        distance[start] = 0
        prev[start] = 0

        // 총 V-1번 반복
        for k in 0..<count - 1 {
            for i in 0..<count {
                for edge in graph[i]{
                    let newDist = distance[i] + edge.weight

                    if newDist < distance[edge.adjvertex] {
                        distance[edge.adjvertex] = newDist // 간선 완화
                        prev[edge.adjvertex] = i
                    }
                }
            }
        }
        return distance
    }
}

```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
