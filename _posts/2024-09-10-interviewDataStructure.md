---
title: "iOS 면접 준비 - 자료구조"
author: gyeomji
date: 2024-09-10 10:00:00 +0900
categories: [Interview]
tags: [자료구조]
pin: false
math: true
mermaid: true
---

## ✏️ 자료구조의 종류와 iOS 개발에서 자주 사용되는 자료구조

---

- 자료구조란 동일한 타입의 데이터를 효율적으로 저장하고 관리하기 위한 구조이다.
- 배열, 연결 리스트, 스택, 큐, 힙, 해시 테이블, 트리, 그래프가 있다.

### 그래프 

- <span style="color:#9fb584">**정점과 간선의 집합으로 이뤄진 비선형 자료구조**</span>이다.
- 하나의 간선은 두 개의 정점을 연결한다.
- 자기 간선, 자기 루프가 없으며 동일 간선의 중복이 없다.
- 방향 그래프는 일방통행, 무방향 그래프는 양방향이다.
- `인접 행렬`과 `인접 리스트`로 구현할 수 있다.
- `인접 행렬` : 2차원 n*n 배열에 저장한다.
  - 연결 관계 확인 O(1)
  - 모든 노드 방문 O(N^2)
  - 인접 리스트에 비해 간선이 많을 경우 효율적이다. (밀집 그래프) 
- `인접 리스트` : 각 정점마다 1개의 단순연결리스트를 이용해 인접한 각 정점을 노드에 저장한다.
  - 연결된 정보만 저장하기 때문에 메모리를 효율적으로 사용할 수 있다.
  - 두 노드의 연결 관계를 확인할 경우 O(n)
  - 모든 노드를 방문할 경우 O(n+e)
  - 간선이 적을 경우 효율적이다. (희소 그래프) 

[📝 Graph](https://gyeom-ji.github.io/posts/graph/)

<br/>

### 트리 

- 노드와 간선을 이용해 <span style="color:#9fb584">**싸이클을 이루지 않는 계층적 구조를 가진 비선형 자료구조**</span>이다.
- 최상위 노드(루트)를 가지고 있다.
- 상위 노드를 부모 노드, 하위 노드를 자식 노드라 한다.
- <span style="color:#9fb584">**O(logN)에 기반한 삽입, 삭제, 탐색**</span>이 가능하다.
- 일반적인 트리와 이진 트리로 구분한다.

#### 이진 트리

- 각 노드의 자식 수가 2 이하인 트리이다.
- 이전 층의 각 노드가 최대 2개의 자식노드를 가지기 때문에, 한 층에 존재할 수 있는 최대 노드 수는 이전 층에 있는 최대 노드 수의 2배이다. 
- 포화 이진 트리, 완전 이진 트리, 편향 이진 트리가 있다.
- 포화 이진 트리 : 각 내부노드가 2개의 자식노드를 가지는 트리
- 완전 이진 트리 : 마지막 레벨을 제외한 각 레벨이 노드로 꽉 차있고, 마지막 레벨에는 노드들이 왼쪽부터 채워진 트리
- 편향 이진 트리 : 트리의 노드가 한쪽 자식 노드로만 계속 연결되어 노드 개수는 적지만 트리 높이가 커지는 트리
- 배열으로 구현한다.
  - 배열의 0번 인덱스를 비워두고, 1번 인덱스에 루트노드부터 저장하면 노드의 부모노드와 자식노드가 배열의 어디에 저장되어 있는지 쉽게 알 수 있다.

<br/>

#### 이진 탐색 트리

- 이진 탐색을 트리에 접목한 자료구조이다.
- 모든 노드가 자신의 왼쪽 자식 노드에는 자기보다 작은 값 오른쪽 자식 노드에는 자기보다 큰 값이 오는 규칙을 만족해야한다. 
- 또한, 노드의 데이터 값은 유일해야하고, 항상 존재해야한다. 
- 이진 탐색 트리에서 탐색, 삽입, 삭제 연산들의 수행시간은 각각 트리의 높이(h)에 비례하여 O(h) 이다.

[📝 Tree](https://gyeom-ji.github.io/posts/tree/)

<br/>

### 힙 

- 이진 힙이라고도 하며, 부모의 우선순위가 자식의 우선순위보다 높은 완전이진트리 형태의 자료구조이다.
- 최댓값 및 최솟값을 찾아내는 연산이 빠르다.
- 이진탐색트리와 달리 중복된 값이 허용된다.
- 배열으로 구현한다.
- 최소 힙, 최대 힙이 있다.
- 최소 힙 : 루트 노드의 키 값이 트리의 최솟값으로 부모의 키 값이 자식의 키 값보다 작거나 같다.
- 최대 힙 : 루트 노드의 키 값이 트리의 최댓값으로 부모의 키 값이 자식의 키 값보다 크거나 같다.

[📝 Heap](https://gyeom-ji.github.io/posts/priorityQueue/)

<br/>

## ✏️ 배열, 연결 리스트, 스택, 큐의 특징과 iOS에서의 구현 방법

---

### 배열 

- 연속적인 메모리 공간에 할당되는 선형 자료구조이다.
- find 연산 O(1), 삽입/삭제 연산 O(N)
- capacity가 초기화될 때 일정 크기의 메모리를 할당받고, capacity를 초과하면 capacity 2배의 영역을 할당하고 해당 요소를 새 스토리지에 복사한다.
- 배열 크기가 동적으로 변경될 수 있다는 말은 메모리 힙 영역에 저장된다는 말이다.
- 힙 영역에 저장되는 것은 배열의 데이터이며, 인스턴스 자체는 스택에 할당된다.

``` swift
var array = [Int]() // capacity = 0
array.append(1) // capacity = 1

```

### 연결 리스트 

- 비연속적인 메모리 공간에 할당되는 선형 자료구조이다.
- find 연산 O(N), 삽입/삭제 연산 O(N)
- 각 요소가 노드로 구성되고, 각 노드는 다음 노드를 가리키는 포인터와, 자신이 가진 값으로 구성되어 있다.

``` swift
final class Node<T: Equatable>{
    var data : T
    var next : Node?
    
    init(data: T, next: Node? = nil) {
        self.data = data
        self.next = next
    }
}

struct SinglyLinkedList<T: Equatable> {
    private var head: Node<T>?
    
    mutating func append(_ data: T) {
        let newNode = Node(data: data)
        
        if head == nil {
            head = newNode
            return
        }
        
        var cur = head
        
        while let next = cur?.next {
            cur = next
        }
        
        cur?.next = newNode
    }
    
    mutating func delete(_ target: T) {
        var cur = head
        var pre: Node<T>?
        
        while cur != nil {
            if cur?.data == target {
                break
            }
            pre = cur
            cur = cur?.next
        }
        
        if pre == nil {
            head = cur?.next
        } else {
            pre?.next = cur?.next
        }
    }
}
```

### 스택 

- 한 쪽 끝에서만 삽입 삭제가 이뤄지는 자료구조로, 후입 선출 원칙 하에 삽입, 삭제를 수행한다.
- 순서가 보존되는 선형 데이터 구조이다.
- 삽입, 삭제 연산 O(1)
- push : 스택 맨 위에 요소를 추가한다.
- pop : 맨 위의 요소를 삭제한다.
- peek: 맨 위의 요소를 반환한다.
- Array, LinkedList로 구현한다.

``` swift
// Array로 구현
struct Stack<T> {
    private var stack = [T]()
    
    var count: Int {
        return stack.count
    }
    
    var isEmpty: Bool {
        return stack.isEmpty
    }
    
    var peek: T? {
        return stack.last
    }
    
    mutating func push(_ data: T) {
        stack.append(data)
    }
    
    mutating func pop() -> T? {
        return stack.popLast()
    }
}

// LinkedList로 구현
final class Node<T>{
    var data : T
    var next : Node?
    
    init(data: T, next: Node? = nil) {
        self.data = data
        self.next = next
    }
}

struct Stack<T> {
    private var top : Node<T>?
    private var count: Int = 0
    
    func getCount() -> Int {
        return count
    }

    var isEmpty: Bool {
        return count == 0
    }
    
    var peek: T? {
        return top?.data
    }
    
    mutating func push(_ data: T) {
        let newNode = Node(data: data)
        
        newNode.next = top
        top = newNode
        count += 1
    }
    
    mutating func pop() -> T? {
        let value = top?.data
        top = top?.next
        count -= 1
        return value
    }
}
```

### 큐 

- 양 쪽 끝에서 삽입 삭제가 이뤄지는 자료구조로, 선입 선출 원칙 하에 삽입, 삭제를 수행한다.
- 순서가 보존되는 선형 데이터 구조이다.
- Array, Linked List, Double Stack으로 구현한다.
  - 배열로 구현 할 시, dequeue는 O(n)의 시간복잡도를 가지게 된다.
  - 더블 스택은 배열을 뒤집어 O(1)의 시간복잡도를 가지는 popLast()를 사용하기 때문에 배열로 구현했을때 보다 빠르다.
- enqueue : 큐의 맨 뒤에 요소를 추가한다.
- dequeue : 맨 앞의 요소를 제거한다.

||Array|DoubleStack|LinkedList|
|:------:|:------:|:------:|:------:|
|삽입|O(1)|O(1)|O(1)|
|삭제|O(N)|O(1)<br/>reversed() 호출 시 O(N)|O(1)|

``` swift
// Array로 구현
struct Queue<T> {
    private var queue = [T]()
    
    mutating func enqueue(_ value: T) {
        queue.append(value)
    }
    
    mutating func dequeue() -> T? {
        return queue.isEmpty ? nil : queue.removeFirst()
    }
}

// DoubleStack으로 구현
struct Queue<T> {
    private var inStack = [T]()
    private var outStack = [T]()
    
    mutating func enqueue(_ value: T) {
        inStack.append(value)
    }
    
    mutating func dequeue() -> T? {
        if outStack.isEmpty {
            outStack = inStack.reversed()
            inStack.removeAll()
        }
        return outStack.popLast()
    }
}

// LinkedList로 구현
final class Node<T>{
    var data : T
    var next : Node?
    
    init(data: T, next: Node? = nil) {
        self.data = data
        self.next = next
    }
}

struct Queue<T> {
    private var head: Node<T>?
    private var tail: Node<T>?
    
    mutating func enqueue(_ value: T) {
        let newNode = Node(data: value)
        if head == nil {
            head = newNode
            tail = newNode
        } else {
            tail?.next = newNode
            tail = newNode
        }
    }
    
    mutating func dequeue() -> T? {
        let value = head?.data
        head = head?.next
        
        if head == nil {
            tail = nil
        }
        return value
    }
}
```

<br/>

## ✏️ 해시 테이블의 개념과 충돌 해결 방법

---

- 해시 테이블은 <span style="color:#9fb584">**(Key, Value)로 데이터를 저장**</span>하는 자료구조로, 빠르게 데이터를 검색할 수 있다.
- Key 값에 해시 함수를 적용해 배열의 인덱스로 사용한다.
- 해싱 구조로 데이터를 저장할 경우 Key 값으로 데이터를 찾을 때 해시 함수를 한 번만 수행하면 되기 때문에 <span style="color:#9fb584">**탐색, 삽입, 삭제 연산에 평균 O(1)**</span>시간이 소요된다.
- swift의 Dictionary가 내부적으로 해시 테이블을 사용한다.
- 아무리 우수한 해시함수를 사용해도 2개 이상의 항목을 해시 테이블의 동일한 원소에 저장해야 하는 경우가 발생하고, <span style="color:#9fb584">**서로 다른 키들이 동일한 해시값을 가질 때 충돌**</span>이 발생한다.
- 충돌 해결 방법은 `개방 주소 방식`과 `폐쇄 주소 방식`으로 구분한다.

<br/>

### 개방 주소 방식

- 해시 테이블 전체를 열린 공간으로 가정하고 <span style="color:#9fb584">**충돌된 키를 일정한 방식에 따라 찾아낸 empty 원소에 저장**</span>한다.
- <span style="color:#9fb584">**적은 양의 데이터에 효과적**</span>이다.
- 해시 테이블에 비어있는 원소가 적을 경우 <span style="color:#9fb584">**재해시**</span>가 필요하다.
- `선형 조사`, `이차 조사`, `랜덤 조사`, `이중 해싱` 방법이 있다.
- `선형 조사` : 충돌이 일어난 해시 인덱스부터 <span style="color:#9fb584">**순차적으로 검색해 처음 발견한 empty원소에 충돌이 일어난 키를 저장**</span>한다.
  - swift의 Dictionary에서 사용한다.
  - 1차 군집화 Primary Clustering 문제를 가진다.
  - 순차탐색으로 empty 원소를 찾아 충돌된 키를 저장하므로 해시테이블의 키들이 빈틈없이 뭉치는 현상이 발생한다.
  - 군집화는 탐색, 삽입, 삭제 연산 시 군집된 키들을 순차적으로 방문해야하는 문제점을 야기시킨다.
  - 군집화는 해시테이블에 empty 원소의 수가 적을수록 심화되며, 이는 해시 성능을 극단적으로 저하시킨다.
- `이차 조사` : 선형조사와 근본적으로 동일한 충돌 해결 방법으로, <span style="color:#9fb584">**해시의 저장순서 폭을 제곱으로 저장**</span>한다.
  - 처음 충돌이 발생한 경우 1만큼 이동하고, 그 다음 충돌이 발생하면 2^2, 3^2 만큼 옮기는 방식이다.
  - 이웃하는 빈 곳이 채워져 만들어지는 1차 군집화 문제를 해결한다.
  - 하지만, 2차 군집화 Secondary Clustering 문제를 가진다.
  - 같은 해시값을 가지는 서로 다른 키들이 똑같은 점프 시퀀스를 따라 empty 원소를 찾아 저장하기 때문에 또 다른 형태의 군집화를 야기한다.
  - 점프 크기가 제곱만큼 커지기 때문에 배열에 empty원소가 있어도 건너뛰어 빈공간 탐색에 실패하는 경우도 존재한다.
- `랜덤 조사` : 점프 시퀀스를 무작위화 하여 빈 원소를 찾는다.
  - 의사 난수 생성기를 사용해 다음 위치를 찾는다.
  - 랜덤 조사 방식도 동의어들이 똑같은 점프 시퀀스에 따라 empty 원소를 찾아 키를 저장하기 때문에 3차 군집화가 발생한다.
- `이중 해싱` : 2개의 해시함수를 사용한다.
  - 하나는 기본적인 해시함수로 키를 해시테이블의 인덱스로 변환한다.
  - 두번째 함수는 충돌 발생 시 다음 위치를 위한 점프 크기를 정한다.
  - 이중해싱은 동이어들이 서로 다른 두번째 해시함수를 가지기 때문에 점프 시퀀스가 일정하지 않다.
  - 이중 해싱은 모든 군집화 문제를 해결하지만, 해싱을 2번 하기 때문에 연산량이 많다.

<br/>

### 개방 주소 방식

- <span style="color:#9fb584">**키에 대한 해시값에 대응되는 곳에만 키를 저장**</span<한다.
- 충돌이 발생한 키들은 한 위치에 모여 저장된다.
- `체이닝` 기법이 있다.
- `체이닝` : 충돌이 일어날 경우 <span style="color:#9fb584">**연결 리스트를 이용해 데이터를 뒤에 추가로 연결시켜 저장**</span>한다.
  - 군집화 현상이 발생하지 않는다.
  - 구현이 간결하여 가장 많이 활용된다.
  - 개방 주소법에 비해 테이블의 확장을 늦출 수 있지만, 데이터의 수가 많아지면 연결리스트들의 길이가 너무 길어져 해시 성능이 매우 저하된다. (최악의 경우 탐색에 O(n)) 
  - 개방 주소법에 비해 데이터 양이 적을 경우 연결 리스트 자체의 메모리 오버헤드가 있다. (다음 노드를 가리키는 포인터 공간)

<br/>

[📝 HashTable](https://gyeom-ji.github.io/posts/hashTable/)

<br/>

## ✏️ Array와 List의 차이점

---

- `Array`는 동일한 타입의 원소들이 <span style="color:#9fb584">**연속적인**</span> 메모리 공간에 할당되는 선형 자료구조이다.
  - 고정된 크기를 가진다.
  - 논리적 저장 순서와 물리적 저장 순서가 일치한다.
    - 메모리 상에 데이터도 순차적으로 저장되는 순차 리스트이다.
  - Cache Hit Rate가 높다.
    - 연속적으로 데이터가 저장되어 있기 때문에 `공간 지역성`이 좋아 높은 Cache Hit Rate를 가진다.
    > 공간 지역성 : 참조된 주소와 인접한 주소의 내용이 다시 참조되는 특성
  - 추가적으로 소모되는 메모리 오버헤드가 거의 없다.
    - 고정된 크기를 가지기 때문이다.
  - 삽입 삭제의 경우 O(N)의 시간 복잡도를 가진다.
  - 원소 접근 find연산의 경우 O(1)의 시간 복잡도를 가진다.
    - 특정 위치 원소 접근 시 배열 인덱스를 사용한다.
    - 데이터의 접근이 많을 때 Array를 사용한다.
  - search연산의 경우 O(N)의 시간 복잡도를 가진다.
  - 기억 장소를 미리 확보해야 한다.
- `List`는 동일한 타입의 원소들이 <span style="color:#9fb584">**비연속적인**</span> 메모리 공간에 할당되는 선형 자료구조이다.
  - 배열은 크기가 정해져 있고, 메모리 상 데이터가 연속적으로 할당되어야 하기 때문에 메모리 낭비가 발생할 수 있다.
  - 리스트는 빈 공간을 허용하지 않기 때문에 배열과 같은 메모리 낭비가 없다.
  - 데이터들이 순차적으로 구성된 집합이지만, 데이터가 메모리 상 연속적으로 있지 않다.
    - 연결 리스트이다.
    - Cache Hit Rate가 낮다.
  - 크기가 가변적이다.
  - find, search연산의 경우 O(N)의 시간 복잡도를 가진다.
    - 노드들을 Head 부터 순차 탐색 해야한다.
  - 삽입, 삭제 연산의 경우 O(N)의 시간 복잡도를 가진다.
    - 삽입 연산의 경우 별도의 용도로 특정 노드를 가리키고 있는 포인터가 있을 경우 O(1) (제일 앞 원소)
    - 삭제, 삽입, 수정 연산 자체는 O(1)의 시간 복잡도를 가지지만, search 작업을 해야하므로 O(N)
    - 삽입, 삭제의 시간 복잡도가 O(N)이지만, Array 보다 빠르다.
      - 데이터의 삽입, 삭제가 많을 때 List를 사용한다.

> Swift의 Array는 `동적 할당 기능`을 가지고 있고, 정수/문자열/클래스 등 모든 데이터 타입을 저장할 수 있다.<br/> Array의 속성인 count는 배열에 있는 요소의 수를 나타낸다. capacity는 새 메모리를 초과하여 할당하기 전, 배열에 포함될 수 있는 요소의 총량을 나타낸다.<br/> 모든 Array는 내용을 보관하기 위해 특정 양의 메모리를 예약한다. 배열에 요소를 추가하고, 해당 배열이 예약 된 capacity를 초과하기 시작하면 배열은 더 큰 메모리 영역을 할당하고 해당 요소를 새 스토리지에 복사한다.<br/> 새 저장소는 이전 저장소 크기의 배수이다. 재할당을 발생시키는 추가 작업에는 성능 비용이 발생하지만 Array가 커질수록 발생 빈도가 점점 줄어 든다.

[📝 Array, List](https://gyeom-ji.github.io/posts/list/)

<br/>
<br/>

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source