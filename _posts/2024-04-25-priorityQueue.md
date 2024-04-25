---
title: "우선순위 큐 Priority Queue"
author: gyeomji
date: 2024-04-25 10:00:00 +0900
categories: [Data Structure]
tags: [Data Structure, Priority Queue]
pin: false
math: true
mermaid: true
---

<br/> 
큐는 FIFO 형식의 자료구조지만, 우선순위 큐는 <span style='background-color:#c8d8b4'>들어오는 순서에 상관없이 우선 순위가 높은 데이터가 먼저 나가는 형태</span>의 자료구조로, <span style="color:#9fb584">**힙 Heap으로 구현**</span>하는 것이 가장 효율적이다. 스택과 큐도 일종의 우선순위큐이다. 스택은 최근에 삽입된 항목일수록 높은 우선순위를 부여하고, 큐는 먼저 삽입된 항목에 높은 우선순위를 부여한다. 스택과 큐는 삽입되는 항목이 임의의 우선순위를 가질 경우, 삽입될때마다 저장되어 있는 항목들을 우선순위에 따라 정렬해야 하는 문제점이 발생하기 때문에 우선순위 큐가 필요하다.
<br/> 


> <center>💡  [ 우선순위 큐 속성 ]</center>
> 1. 모든 항목에는 우선순위가 있다.
> 2. 우선순위가 높은 요소는 우선순위가 낮은 요소보다 먼저 나간다.
> 3. 두 요소의 우선순위가 같을 경우 큐의 순서에 따라 나간다.


<br/> 

## 우선순위 큐 구현

: 배열, 연결 리스트, 힙으로 구현이 가능하다. 힙이 최악의 경우에도 O(logN)을 보장하기 때문에 힙으로 구현하는 것이 가장 효율적이다.

| |배열|정렬된 배열|연결 리스트|정렬된 연결 리스트|힙|
|:------:|:------:|:------:|:------:|:------:|:------:|
|enqueue()|O(1)|O(N)|O(1)|O(N)|O(logN)
|dequeue()|O(N)|O(1)|O(N)|O(1)|O(logN)

<br/> 

## 이진힙 Binary Heap

---

: 힙은 이진힙이라고도 하며, <span style='background-color:#c8d8b4'>부모의 우선순위가 자식의 우선순위보다 높은 완전이진트리 형태</span>의 자료구조이다. 여러개의 값 중 <span style='background-color:#c8d8b4'>최댓값 및 최솟값을 찾아내는 연산이 빠르다.</span>

 - 완전 이진트리이다.
 - 부모노드의 값과 자식노드의 값 사이 대소 관계가 성립된다.
   - 부모자식 간에만 성립되며 형제사이는 대소관계가 정해지지 않는다.
 - 이진탐색트리와 달리 중복된 값이 허용된다.


### 힙의 종류

#### 최대 힙 Maximum Heap

: 키 값이 <span style="color:#9fb584">**클수록**</span> 더 높은 우선순위를 가진다.

- 부모노드의 키 값이 자식 노드보다 크거나 같은 힙이다.
- 가장 <span style="color:#9fb584">**큰 값이 루트노드**</span>에 있다.

#### 최소 힙 Minimum Heap

: 키 값이 <span style="color:#9fb584">**작을수록**</span> 높은 우선순위를 가진다.

- 부모노드의 키 값이 자식 노드보다 작거나 같은 힙이다.
- 가장 <span style="color:#9fb584">**작은 값이 루트노드**</span>에 있다.

### 힙 구현

: 완전이진트리는 1차원 배열로 구현하기 때문에 힙도 일반적으로 <span style='background-color:#c8d8b4'>배열으로 구현</span>한다.

- 배열의 1번 인덱스부터 사용한다.
  - 0번째 인덱스는 사용하지 않는다.
- 완전이진트리의 노드들을 레벨 순회 순서에 따라 array[1]부터 차례로 저장한다.
- 노드를 배열에 저장했을 경우 부모와 자식 관계
  - 노드 i의 부모노드 인덱스 = i/2
  - 노드 i의 왼쪽 자식 노드 인덱스 = 2i
  - 노드 i의 오른쪽 자식 노드 인덱스 = 2i + 1
  
### 최솟값 삭제 deleteMin()

1. 힙의 가장 마지막 노드 즉, 배열의 가장 마지막 원소를 루트로 옮긴다.
2. 힙 크기를 1 감소시킨다.
3. 루트에서부터 자식들 중 가장 작은 값을 가진 자식과 키를 비교하여 이파리 방향으로 Heapify(downheap) 수행한다.
    - `Heapify` : heap 특성을 만족하도록 각 노드들의 위치를 조정하는 과정
    - 루트노드에서 아래로 내려가며 진행되기 때문에 `downheap`이라 한다.  

```swift
    
mutating func dequeue() -> Int {
    var min = heap[1]
    heap.swapAt(1, size - 1)
    heap.removeLast()
    size -= 1
    downheap(1)
    return min
}
    
private mutating func downheap(_ i: Int) {
    var i = i
    while(2 * i <= size) {
        var k = 2 * i
        if k < size && greater(k, k + 1) { k += 1 }
        if !greater(i, k) {break}
        heap.swapAt(i, k)
        i = k
    }
}
```
### 삽입 insert()

1. 힙의 가장 마지막 노드 즉, 배열의 가장 마지막 원소 바로 다음 빈 원소에 새로운 원소를 저장한다.
2. 루트 방향으로 올라가면서 부모노드의 키 값과 비교하여 Heapify(upheap) 수행한다.
    - 이파리노드에서 위로 올라가며 진행되기 때문에 `upheap`이라 한다.

```swift
    
mutating func enqueue(_ value: Int) {
    heap.append(value)
    size += 1
    upheap(size)
}

private mutating func upheap(_ j: Int) {
    var j = j
    while(j > 1 && greater(j/2, j)) {
        heap.swapAt(j/2, j)
        j = j/2
    }
}
```
### 상향식 힙 만들기 Bottom-up Heap Construction

- 상향식 방식으로 각 노드에 대해 힙속성을 만족하도록 부모와 자식노드를 서로 교환
- N개의 항목이 배열에 임의의 순서로 저장되어 있을 때, 힙을 만들기 위해 array[N/2]부터 array[1]까지 차례로 `downheap`을 각각 수행해 힙속성을 충족시킨다.
- O(N)의 수행시간
- array[N/2 + 1] ~ array[N]에 대해 downheap 수행하지 않는 이유
  - 이 노드들은 각각 이파리노드기 때문에, 각 노드 스스로가 힙 크기가 1인 최소힙이기 때문

![BottomUpHeap](/assets/img/bottomupheap.png){: width="400" height="300"}

```swift
    
mutating func creatHeap() {
    for i in stride(from: size/2, to: 0, by: -1) {
        downheap(i)
    }
}
```

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
