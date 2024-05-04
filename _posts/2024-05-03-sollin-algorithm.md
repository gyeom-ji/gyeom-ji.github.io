---
title: "솔린 알고리즘 Sollin Algorithm"
author: gyeomji
date: 2024-05-03 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Sollin, MST, 최소 신장 트리]
pin: false
math: true
mermaid: true
---

<br/> 
<span style="color:#9fb584">**최소 신장 트리 알고리즘**</span>으로 크루스칼, 프림 알고리즘처럼 간선을 하나씩 선택하는 것이 아닌 <span style="color:#9fb584">**동시에 여러 개의 간선을 선택**</span>한다. 프림 알고리즘은 매 순간 최선의 조건을 선택하는 탐욕 알고리즘을 바탕으로, <span style='background-color:#c8d8b4'>가중치가 가장 가벼운 간선을 선택</span>하여 <span style="color:#9fb584">**Forest**</span>를 만들어가며 하나의 트리를 만드는 방식의 알고리즘이다. 

<br/> 
<br/>

## Forest

: <span style="color:#9fb584">**서로 연결되지 않은**</span> 트리들의 집합 또는 연결되지 않은 비순환 그래프의 집합

![forest](/assets/img/forest.png){: width="300" height="350"}

위의 네 그래프는 <span style="color:#9fb584">**서로 순환하지 않은 그래프이면서 서로 연결되어 있지 않다**</span>. 이런 자료 구조를 포레스트라고 한다. <span style="color:#9fb584">**정점만 있는 그래프나 하나의 트리도 포레스트가 될 수 있다.**</span>

<br/>

## MST(최소 비용 신장 트리) 조건

---

1. 최소 비용의 간선으로 구성됨

2. 사이클을 포함하지 않음

: **각 단계에서 사이클을 이루지 않는 최소 비용 간선 선택**

<br/>
<br/>

## Sollin 알고리즘 

---

- O(mlogn)의 시간 복잡도를 가진다.
  - 단계마다 각 쌍의 트리가 서로 연결된 간선을 선택하는 경우 최대 logN번 수행
  - 루프 내에서는 각 트리가 자신에게 연결된 모든 간선들을 검사하여 최소 가중치를 가진 간선을 선택하므로 O(m) 시간 소요

### Sollin 알고리즘 동작 방식

1. 초기에 n개의 포레스트부터 시작한다. (각 정점을 독립적인 트리로 간주)
2. 각 트리에 연결된 간선들 중 가장 가중치가 작은 간선을 선택하여 트리를 합친다.
   1. 이때 선택된 간선은 2개의 트리를 1개의 트리로 만든다.
3. 위의 과정을 하나의 트리만 남을 때 까지 반복한다.

<br/>

![sollin](/assets/img/sollin.png)

<br/>

``` swift 

struct SolinMST {
    private var graph = [Edge]()
    private var minCost = 0
    private var parent = [Int]()
    
    init(graph: [Edge]) {
        self.graph = graph
    }
    
    func getMinCost() -> Int {
        return minCost
    }
    
    mutating func makeSet(count: Int){
        self.parent = (0..<count).map{ $0 }
    }

    func findParent(_ child: Int) -> Int {
        return parent[child] == child ? child : findParent(parent[child])
    }
    
    mutating func unionParent(_ oldParent: Int, _ newParent: Int) {
        parent.indices.filter{parent[$0] == oldParent}.forEach{parent[$0] = newParent}
    }

    mutating func solin(count: Int) -> [Edge] {
        
        var treeCount = count
        // 간선 저장
        var edgeArr = [Edge]()
        
        // 트리를 합치기 위해 (unionFind) 초기화
        makeSet(count: count)
        
        // 트리가 하나만 남을 때 까지 반복
        while treeCount > 1 {
            // 최소 가중치 간선을 저장하는 배열 초기화
            var minEdgeArr = Array(repeating: -1, count: count)
            
            for i in 0..<graph.count {
                // 양쪽 정점 부모 찾기
                var first = findParent(graph[i].start)
                var second = findParent(graph[i].end)
                
                // 두 정점이 한 트리에 속할 경우 무시
                if first == second {
                    continue
                }
   
                // 각 정점에 연결된 간선 중 최소 가중치를 가진 간선을 저장한다.
                if minEdgeArr[first] == -1 || graph[minEdgeArr[first]].weight > graph[i].weight {
                    minEdgeArr[first] = i
                }
                
                if minEdgeArr[second] == -1 || graph[minEdgeArr[second]].weight > graph[i].weight {
                    minEdgeArr[second] = i
                }
            }
            
            // 연산할 필요 없는 간선은 무시
            for j in 0..<count where minEdgeArr[j] != -1 {
                
                // 양쪽 정점 부모 찾기
                var first = findParent(graph[minEdgeArr[j]].start)
                var second = findParent(graph[minEdgeArr[j]].end)
                
                // 엣지로 연결된 두 정점이 이미 한 트리에 속하면 무시
                if first == second {
                    continue
                }
                
                // 그렇지 않을 경우 MST 간선
                minCost += graph[minEdgeArr[j]].weight
                edgeArr.append(graph[minEdgeArr[j]])
                
                // 트리를 합친다
                unionParent(first, second)
                // 트리가 합쳐졌으므로 트리 개수 -1
                treeCount -= 1
            }
        }

        return edgeArr
    }
}

```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
