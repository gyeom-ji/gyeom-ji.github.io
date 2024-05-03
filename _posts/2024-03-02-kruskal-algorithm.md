---
title: "크루스칼 알고리즘 Kruskal Algorithm"
author: gyeomji
date: 2024-03-02 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Greedy, Kruskal, 크루스칼, MST, 최소 신장 트리]
pin: false
math: true
mermaid: true
---

<br/> 
대표적인 **최소 신장 트리 알고리즘**으로<br /> 
탐욕적인 방법을 이용해 가중치를 간선에 할당한 그래프의 <span style='background-color:#c8d8b4'>모든 정점을 최소 비용으로 연결</span>하는 최적 해답을 구하는 알고리즘 

> 신장트리<br />
> \: 하나의 그래프가 있을 때 모든 노드를 포함하면서 사이클이 존재하지 않는 부분 그래프 <br />
>
> 최소 신장 트리 알고리즘 <br />
> \: 신장 트리 중 최소 비용으로 만들 수 있는 신장 트리를 찾는 알고리즘

<br/> 
<br/>

## MST(최소 비용 신장 트리) 조건

---

1. 최소 비용의 간선으로 구성됨

2. 사이클을 포함하지 않음

: **각 단계에서 사이클을 이루지 않는 최소 비용 간선 선택**

<br/>
<br/>

## 크루스칼 알고리즘 동작 과정

---

1. 그래프의 간선들을 비용에 따라 <span style='background-color:#c8d8b4'>오름차순으로 정렬</span>한다.

2. 정렬된 간선 리스트에서 순서대로 <span style='background-color:#c8d8b4'>사이클을 형성하지 않는 간선을 선택</span>한다.
    - <span style="color:#9fb584">**가장 낮은 가중치를 가진 간선을 먼저 선택**</span>한다
    - <span style="color:#9fb584">**사이클을 형성하는 간선을 제외**</span>하고 MST에 추가한다.

3. 모든 간선에 대해 2번 과정을 반복한다. (n - 1개의 간선이 선택되었을 때 종료)

<br/>


![kruskal](/assets/img/kruskal1.png)
![kruskal](/assets/img/kruskal2.png)
![kruskal](/assets/img/kruskal3.png)

<br/>

<span style="color:#9fb584">**[ 주의 사항 ]**</span><br/>
- 간선을 MST에 추가할 때 사이클을 생성하는지 체크해야 한다
- 새로운 간선의 **양끝 정점이 같은 집합에 속해 있으면 사이클이 형성** 됨

<br/>

<span style="color:#9fb584">**[ 사이클 생성 여부 확인 방법 ]**</span><br/>
- 추가할 새로운 간선의 양끝 정점이 같은 집합에 속해 있는지 먼저 검사해야 한다.
- Union-Find 알고리즘 사용
<br/>

<br/> 
<br/>

## 크루스칼 알고리즘 시간 복잡도 - O(eloge)

---

- Union-Find 알고리즘 이용 시 크루스칼 알고리즘의 시간 복잡도는 간선들을 정렬하는 시간에 좌우된다.
- 크루스칼 내부에서 사용되는 집합 알고리즘의 시간 복잡도는 정렬 알고리즘의 시간 복잡도 보다 작으므로 무시한다.
- 간선 e개를 퀵 정렬과 같은 알고리즘으로 정렬 시 O(eloge)

<br/> 
<br/>

## Union-Find 알고리즘

---

- Disjoint-Set 구현시 사용하는 알고리즘
    - 집합을 구현하는데 비트 벡터, 배열, 연결 리스트를 이용할 수 있으나, 가장 효율적인 <span style='background-color:#c8d8b4'>트리 구조</span>를 이용하여 구현

- 크루스칼 알고리즘에서 그래프의 최소 신장 트리 MST 를 찾는데 활용 ( 정점 연결 및 사이클 형성 여부 확인 )<br />

>💡 Disjoint Set 서로소 집합 자료구조
>: **서로 중복되지 않는 부분 집합**들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조 <br />=> 공통 원소가 없는 부분 집합들로 나눠진 원소들에 대한 자료구조


<br />
<br />

### Union-Find 기능
- `make-set(x)` - 초기화
: x를 유일한 원소로 하는 새로운 집합을 만든다 
 
- `union(x, y)` - 합하기, 연결하기
 : x가 속한 집합과 y가 속한 집합을 합친다 
 
- `find(x)` - 부모(속한 그룹) 찾기
 : x가 속한 집합의 부모를 반환한다

<br />
<br />

## 크루스칼 알고리즘 사용해 해결한 문제

---

#### Programmers - 섬 연결하기

\- n개의 섬 사이에 다리를 건설하는 비용이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능할 수 있는 비용을 구하는 문제<br />

> 💡 가장 값이 낮은 연결로 정렬 후 사이클 구간을 제외시키기<br /> 　 　left를 부모로 설정해 연결 시 부모를 left의 부모로 변경

<br />

```swift
import Foundation

func solution(_ n:Int, _ costs:[[Int]]) -> Int {
    var costs = costs.sorted(by: { $0[2] < $1[2] })
    //초기화
    var parent : [Int] = (0..<n).map{ $0 }
    var anw = 0

    // 부모 찾기
    func findParent(_ child: Int) -> Int {
        return parent[child] == child ? child : findParent(parent[child])
    }
    func unionParent(_ oldParent: Int, _ newParent: Int) {
        parent.indices.filter{parent[$0] == oldParent}.forEach{parent[$0] = newParent}
    }

    func connectIsland(_ left: Int, _ right: Int) {
        // right 부모를 가진 것들을 left의 부모로 바꿔줌
        unionParent(findParent(right), findParent(left))
    }

    for cost in costs {
        //연결하는 다리가 사이클 형성하는지 확인
        if parent[cost[0]] != parent[cost[1]] {
            connectIsland(cost[0], cost[1])
            anw += cost[2]
        }
    }

    return anw
}
```

- 배운점
  :처음에는 kruskal 알고리즘을 몰라서 문제를 해결하지 못했다<br />검색을 통해 kruskal, union-find 알고리즘에 대해 알게 되었고 문제를 풀며 알고리즘에 대한 이해를 높일 수 있었던 것 같다

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
