---
title: "다익스트라 알고리즘 Dijkstra Algorithm"
author: gyeomji
date: 2024-05-03 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Dijkstra]
pin: false
math: true
mermaid: true
---

<br/> 
<span style="color:#9fb584">**최단 경로 찾기 알고리즘**</span>으로 <span style='background-color:#c8d8b4'>음의 간선이 없는 가중 그래프</span>에서 출발점으로부터 각 정점까지의 <span style="color:#9fb584">**최단거리 및 경로를 계산**</span>하는 알고리즘이다. [`Prim 알고리즘`](https://gyeom-ji.github.io/posts/prim-algorithm/)과 매우 유사하며, <span style="color:#9fb584">**동일한 수행시간**</span>을 가진다. Dijkstra 알고리즘은 출발점이 주어지지만, Prim 알고리즘은 출발점이 주어지지 않는다. Prim 알고리즘은 배열 distance의 원소에 간선의 가중치가 저장되지만, Dijkstra 알고리즘은 distance의 원소에 출발점으로부터 각 정점까지의 <span style="color:#9fb584">**경로 길이가 저장**</span>된다.

<br/> 
<br/>

## Dijkstra 알고리즘 - 배열

---

- O(n^2)의 시간 복잡도를 가진다.
  - n번 minVertex를 찾고 minVertex에 인접하는 미방문 정점들의 간선완화 수행
  - 배열 distance에서 minVertex 탐색하는데 O(n)
  - minVertex에 인접한 정점 검사로 distance 원소를 갱신하므로 추가로 O(n)소요
- 정점의 수가 적거나 간선의 수가 매우 많은 경우 유용하다.
- 입력 그래프에 음수 가중치가 있으면 최단 경로 찾기에 실패할 수 있다.
  - distance 원소 값의 증가 순으로 minVertex를 선택하고, 한번 방문된 정점의 distance원소를 다시 갱신하지 않기 때문이다.

### Dijkstra 알고리즘 - 배열 구현

1. distance를 초기화 시키고, 출발 정점을 설정한다.
2. 방문되지 않은 각 정점 i에 대해 distance[i]가 최소인 정점 minVertex를 찾고, 방문 완료로 변경한다.
   1. 한 번 방문된 정점의 distance원소 값은 변하지 않는다.
3. minVertex에 인접한 방문 하지 않은 정점들에 대한 간선완화를 수행한다.
4. 모든 정점을 방문할 때까지 2 , 3번을 반복한다.

<br/>
> 간선 완화 : minVertex가 선택된 후 start로부터 minVertex를 경유하여 정점 w까지의 경로 길이가 현재 distance[w]보다 더 짧아지면 짧은 길이로 distance[w]를 갱신하는 것

<br/>

``` swift 

struct Edge: Comparable, CustomStringConvertible {
    let start : Int
    let end : Int
    var weight : Int
    
    init(start: Int, end : Int, weight: Int) {
        self.start = start
        self.end = end
        self.weight = weight
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
           lhs.weight < rhs.weight
    }
    
    var description: String {
        return "(\(start), \(end), \(weight))"
    }
}

struct Dijkstra {
    private var count = 0
    // 최단 경로 상 이전 정점을 기록하기 위해
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
    
    mutating func dikstra(_ start: Int) -> [Int] {
        // 초기화
        var visited = Array(repeating: false, count: count)
        var distance = Array(repeating: Int.max, count: count)
        prev = Array(repeating: -1, count: count)
        
        // 시작점 start 관련 정보 설정
        prev[start] = 0
        distance[start] = 0
        
        for k in 0..<count {
            var minVertex = -1
            var minDist = Int.max
            
            // 미방문 정점 중 distance 값이 최소인 minVertex 찾기
            for j in 0..<count {
                if !visited[j] && distance[j] < minDist {
                    minDist = distance[j]
                    minVertex = j
                }
            }

            // 방문 표시
            visited[minVertex] = true
            
            // minVertex에 인접한 각 정점 중
            for edge in graph[minVertex] {
                // 미방문된 정점에 대해
                if !visited[edge.end] {
                    let curDist = distance[edge.end]
                    let newDist = distance[minVertex] + edge.weight
                    
                    if newDist < curDist {
                        distance[edge.end] = newDist // 간선 완화
                        prev[edge.end] = minVertex // 최종 최단 경로를 역방향으로 추출
                    }
                }
            }
        }
        return distance
    }
}

```

## Dijkstra 알고리즘 - 우선순위 큐

---

- O(mlogn)의 시간 복잡도를 가진다.
- minVertex를 찾기 위해 간선의 거리를 우선순위로 저장하는 우선순위 큐 사용
- 희소 그래프일 경우 우선순위 큐를 사용하는 것이 매우 효율적이다.
- 음수 간선을 포함하는 사이클이 있을 경우, 사이클을 순회할수록 계속해서 작은 값이 갱신 되기 때문에 최단 경로 찾기에 실패할 수 있다.

### Dijkstra 알고리즘 - 우선순위 큐 구현

1. distance를 초기화 시키고, 출발 정점을 설정하고, 우선순위 큐에 출발 정점을 삽입한다. (가중치 0)
2. 정점 cur을 우선순위 큐에서 추출하고, 인접한 정점 edge 들에 대한 간선 완화를 수행한다.
   1. (cur -> edge 거리 + cur 가중치 값) > distance[edge]
   2. distance[edge]를 업데이트 해주고, 해당 값을 큐에 삽입한다.
3. 2번을 우선순위 큐가 빌 때까지 반복한다.


<br/>

``` swift 

struct MinHeap<T: Comparable> {
    private var heap : [T?] = Array(repeating: nil, count: 1)
    private var size = 0
    
    mutating func creatHeap() {
        for i in stride(from: size/2, to: 0, by: -1) {
            downheap(i)
        }
    }
    
    mutating func dequeue() -> T? {
        if size == 0 {
            return nil
        }
        var min = heap[1]
        heap.swapAt(1, size)
        heap.removeLast()
        size -= 1
        downheap(1)
        return min
    }
        
    private mutating func downheap(_ i: Int) {
        var i = i
        while(2 * i <= size) {
            var k = 2 * i
            if k < size && (heap[k]! > heap[k + 1]!) { k += 1 }
            if heap[i]! < heap[k]! { break }
            heap.swapAt(i, k)
            i = k
        }
    }
    
    mutating func enqueue(_ value: T) {
        heap.append(value)
        size += 1
        upheap(size)
    }

    private mutating func upheap(_ j: Int) {
        var j = j
        while(j > 1 && heap[j/2]! > heap[j]!) {
            heap.swapAt(j/2, j)
            j = j/2
        }
    }
}

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

struct Dijkstra {
    private var count = 0
     // 최단 경로 상 이전 정점을 기록하기 위해
    private var prev = [Int]()
    private var minHeap = MinHeap<Edge>()
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
    
    mutating func dikstra(_ start: Int) -> [Int] {
        // 초기화
        var distance = Array(repeating: Int.max, count: count)
        prev = Array(repeating: -1, count: count)

        // 시작점 start 관련 정보 설정 (시작점이기 때문에 가중치 0)
        distance[start] = 0
        // 시작점 우선순위 큐에 삽입
        minHeap.enqueue(Edge(adjvertex: start, weight: 0))
        
        while true {
            guard let cur = minHeap.dequeue() else { break }
            
            // 현재 distance가 기존 distance보다 길다면 무시
            if distance[cur.adjvertex] < cur.weight {
                continue
            }
            
            // 현재 정점에 인접한 각 정점에 대해
            for edge in graph[cur.adjvertex] {
                // 해당 정점을 거쳐 갈 때 거리
                let newDist = edge.weight + cur.weight
                
                if newDist < distance[edge.adjvertex] {
                    
                    distance[edge.adjvertex] = newDist // 간선 완화
                    prev[edge.adjvertex] = cur.adjvertex // 최종 최단 경로를 역방향으로 추출

                    // 다음 인접 거리를 계산하기 위에 enqueue
                    minHeap.enqueue(Edge(adjvertex: edge.adjvertex, weight: newDist))
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
