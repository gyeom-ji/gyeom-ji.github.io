---
title: "프림 알고리즘 Prim Algorithm"
author: gyeomji
date: 2024-05-01 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Prim, MST, 최소 신장 트리]
pin: false
math: true
mermaid: true
---

<br/> 
<span style="color:#9fb584">**최소 신장 트리 알고리즘**</span>으로 <span style='background-color:#c8d8b4'>정점을 하나씩 추가</span>하여 트리를 확장해 나가는 방식의 알고리즘이다. 프림 알고리즘은 매 순간 최선의 조건을 선택하는 탐욕 알고리즘을 바탕으로, 탐색 정점에 연결된 인접 정점들 중 <span style="color:#9fb584">**비용이 가장 적은 간선으로 연결된 정점을 선택**</span>한다.

<br/> 
<br/>

## MST(최소 비용 신장 트리) 조건

---

1. 최소 비용의 간선으로 구성됨

2. 사이클을 포함하지 않음

: **각 단계에서 사이클을 이루지 않는 최소 비용 간선 선택**

<br/>
<br/>

## Prim 알고리즘 - 배열

---

- O(n^2)의 시간 복잡도를 가진다.
- minVertex를 배열 distance에서 탐색하는 과정 O(n)
- minVertex에 인접한 정점들을 검사하여 distance의 해당 원소를 갱신하는 과정 O(n)

### Prim 알고리즘 - 배열 구현

1. 임의의 시작 정점에서 가장 가까운 정점을 추가하여 하나의 트리 T를 만든다.
2. 트리에 속하지 않은 각 정점과 T의 정점들에 인접한 간선들 중 가장 작은 가중치의 간선과 연결된 정점을 트리에 추가한다.
   1. 사이클을 막기 위해 방문하지 않은 정점만 추가한다.
3. 2번을 T의 개수가 그래프의 정점 개수가 될 때까지 반복한다.
   1. 간선의 가중치를 더하여 최소 신장 트리 비용을 산출한다.

<br/>

![prim](/assets/img/prim1.png)
![prim](/assets/img/prim2.png)

<br/>

```swift

struct Edge: Comparable {
    let adjvertex : Int // 간선의 다른 쪽 정점
    let weight : Int // 간선 가중치
    
    init(adjvertex: Int, weight: Int) {
        self.adjvertex = adjvertex
        self.weight = weight
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
           lhs.weight < rhs.weight
    }
}

struct PrimMST {
    private var count = 0
    private var graph = [[Edge]]()
    private var minCost = 0
    
    init(graph: [[Edge]]) {
        self.graph = graph
        self.count = graph.count
    }
    
    func getMinCost() -> Int {
        return minCost
    }
    
    mutating func prim(_ start: Int) -> [Int] {
        var visited = Array(repeating: false, count: count)
        var distance = Array(repeating: Int.max, count: count) // 각 distance 최댓값으로
        var prev = Array(repeating: -1, count: count) // 최소신장트리의 간선으로 확정될 때 간선의 다른 쪽 정점 저장
        
        prev[start] = 0
        distance[start] = 0
    
        // 방문되지 않은 정점들의 distance 원소들 중에서 최솟값 가진 정점 minVertex 찾기
        for k in 0..<count {
            var minVertex = -1
            var min = Int.max
            
            for j in 0..<count {
                if !visited[j] && distance[j] < min {
                    min = distance[j]
                    minVertex = j
                }
            }
            // 트리에 추가 (방문 표시)
            visited[minVertex] = true
            // 최소 비용
            minCost += min
            
            for edge in graph[minVertex] { // minVertex에 인접한 각 정점의 distance 갱신
                if !visited[edge.adjvertex] { // 트리에 아직 포함이 되지 않은 정점일 경우
                    var currentDist = distance[edge.adjvertex]
                    var newDist = edge.weight
                    
                    if newDist < currentDist {
                        distance[edge.adjvertex] = newDist // minVertex와 연결된 정점들의 distance 갱신
                        prev[edge.adjvertex] = minVertex // 트리 간선 추출을 위해 저장
                    }
                }
            }
        }
        return prev // 최소신장트리 간선 정보 리턴
    }
}

```

## Prim 알고리즘 - 우선순위 큐

---

- O(mlogn)의 시간 복잡도를 가진다.
- minVertex를 찾기 위해 간선의 가중치를 우선순위로 저장하는 우선순위 큐 사용
- 희소 그래프일 경우 우선순위 큐를 사용하는 것이 매우 효율적이다.


### Prim 알고리즘 - 우선순위 큐 구현

1. 임의의 시작 정점에서 가장 가까운 정점을 추가하여 우선순위 큐에 삽입한다.
2. 노드 cur를 우선순위 큐에서 제외시키고 트리에 포함시킨다.
   1. 트리에 포함되어 있는 노드일 경우 아래 작업을 거치지 않고 우선순위 큐에서 제외한다.
3. cur와 연결되어 있으면서 트리에 포함되지 않은 노드들을 우선순위 큐에 삽입한다.
4. 2, 3번을 우선순위 큐가 빌 때까지 반복한다.


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

struct PrimMST {
    private var count = 0
    private var minHeap = MinHeap<Edge>()
    private var graph = [[Edge]]()
    private var minCost = 0
    
    init(graph: [[Edge]]) {
        self.graph = graph
        self.count = graph.count
    }
    
    func getMinCost() -> Int {
        return minCost
    }
    
    mutating func prim(_ start: Int) {
        var visited = Array(repeating: false, count: count)
        
        // 시작 노드 우선순위 큐에 삽입
        // 시작 노드이기 때문에 가중치 0
        minHeap.enqueue(Edge(adjvertex: start, weight: 0))
        
        while true {
            guard let cur = minHeap.dequeue() else { break }
            // 트리에 아직 포함이 되지 않은 정점일 경우
            if !visited[cur.adjvertex] {
                // 트리에 추가 (방문 표시)
                visited[cur.adjvertex] = true
                // 최소 비용 
                minCost += cur.weight
                print("connected Nodes : ", cur.adjvertex)
                
                // cur와 연결되어 있으면서 트리에 포함되지 않은 노드들을 우선순위 큐에 삽입한다.
                for node in graph[cur.adjvertex] {
                    if !visited[node.adjvertex] {
                        minHeap.enqueue(node)
                    }
                }
            }
        }
    }
}

```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
