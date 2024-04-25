---
title: "Stack & Queue"
author: gyeomji
date: 2024-04-18 10:00:00 +0900
categories: [Data Structure]
tags: [Data Structure, Stack, Queue]
pin: false
math: true
mermaid: true
---

## 스택 Stack

---

: <span style="color:#9fb584">**한 쪽 끝에서만**</span> 삽입 삭제가 이뤄지는 자료구조로, <span style="color:#9fb584">**후입 선출 LIFO(Last-In First-Out)**</span>원칙하에 삽입, 삭제를 수행한다.<br/> <span style="color:#9fb584">**Array, Linked List**</span>로 구현한다.

<br />

[ 기본 동작 ]

- push : 스택 맨 위에 요소를 추가한다.
- pop : 맨 위의 요소를 꺼낸다.


## Stack 사용해 해결한 문제

---

#### Programmers - 올바른 괄호

\- '(' 또는 ')' 로만 구성된 문자열이 주어질 때 문자열이 올바른 괄호인지 아닌지 판별하는 문제<br /> 올바른 괄호란 '(' 문자로 열리면 ')' 문자로 닫아야 함을 의미

<br />

```swift
import Foundation

func solution(_ s:String) -> Bool {
    var stack = [Character]()
    let arr = Array(s)

    for s in arr {
        if s == "(" {
            stack.append(s)
        } else {
            if stack.isEmpty {
                return false
            } 
            stack.removeLast()
        }
    }
    return stack.isEmpty
}
```

- 배운점
  : ( 가 나오면 ) 로 닫아야 올바른 괄호가 되기 때문에 stack을 사용해 ( 일 때 스택에 넣고, ) 가 나오면 스택에서 빼줬다. ) 가 나왔는데 스택이 비었거나, 문자열이 끝났는데 스택이 비지 않았다면 괄호의 짝이 맞지 않아 올바른 괄호가 아니기 때문에 false를 반환했다.

<br />

## 큐 Queue

---

: <span style="color:#9fb584">**양 끝에서**</span> 삽입 삭제가 이뤄지는 자료구조로, <span style="color:#9fb584">**선입 선출 FIFO(First-In First-Out)**</span>원칙하에 삽입, 삭제를 수행한다.<br/> <span style="color:#9fb584">**Array, Linked List, Double Stack**</span>으로 구현한다.

<br />

[ 기본 동작 ]

- enqueue : 큐의 맨 뒤에 요소를 추가한다.
- dequeue : 맨 앞의 요소를 꺼낸다.


## Queue 구현 방법

---

1. Array
    - **삽입 append() : O(1)**
    - 삭제 removeFirst() : O(n)
        : removeFirst 시 i번 인덱스 위치로 i + 1번 인덱스 요소가 n번 이동해야 한다.

```swift
struct ArrayQueue<T> {
    private var arr: [T] = []

    mutating func enqueue(_ value: T) {
        arr.append(value)
    }

    mutating func dequeue() -> T? {
        return arr.isEmpty ? nil : arr.removeFirst()
    }
}
```

2. Double Stack
    - enqueue, dequeue 스택으로 구현
    - **삽입 append() : O(1)**
    - 삭제 popLast() : O(1)
        : reversed() 호출 시 O(n)

    > enqueue에 삽입, 삭제 시 enqueue를 뒤집은 dequeue에서 popLast()<br/>
    > 배열로 구현 할 시, dequeue는 O(n)의 시간복잡도를 가지게 된다.<br/>
    > 더블 스택은 배열을 뒤집어 O(1)의 시간복잡도를 가지는 popLast()를 사용하기 때문에 배열로 구현했을때 보다 빠르다.


```swift
struct DoubleStackQueue<T> {
    private var inStack: [T] = []
    private var outStack: [T] = []

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

```

3. Linked List
    - **삽입 : O(1)**
    - **삭제 : O(1)**

```swift
final class Node<T> {
    var data : T
    var next: Node<T>?
    
    init(_ data: T, _ next: Node<T>? = nil) {
        self.data = data
        self.next = next
    }
}

struct LinkedListQueue<T> {
    var head : Node<T>?
    var tail : Node<T>?
    
    mutating func enqueue(_ value: T) {
        let node = Node(value)
        if head == nil {
            head = node
            tail = node
        } else {
            tail?.next = node
            tail = node
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

> 💡 연결 리스트는 Node를 참조 타입인 클래스로 구현하고, 더블 스택은 값 타입인 구조체 array를 사용한다.<br/>
array도 Copy-on-write로 힙에 할당되고 레퍼런스 카운팅을 발생시키지만, 더블 스택은 2개의 배열을 사용하고, 연결 리스트는 N개의 노드를 사용하기 때문에 연결 리스트가 성능적으로 소모를 많이 한다고 할 수 있다.


## Queue 사용해 해결한 문제

---

#### Programmers - 프로세스

\- 현재 실행 대기 큐에 있는 프로세스의 중요도가 순서대로 담긴 배열 priorities와 몇 번째로 실행되는지 알고 싶은 프로세스의 위치를 알려주는 location이 매개변수로 주어진다. 다음 규칙에 따라 프로세스를 관리할 경우 해당 프로세스가 몇 번째로 실행되는지 구하는 문제<br />

[ 규칙 ]
1. 실행 대기 큐(Queue)에서 대기중인 프로세스 하나를 꺼냅니다.
2. 큐에 대기중인 프로세스 중 우선순위가 더 높은 프로세스가 있다면 방금 꺼낸 프로세스를 다시 큐에 넣습니다.
3. 만약 그런 프로세스가 없다면 방금 꺼낸 프로세스를 실행합니다.
  3.1 한 번 실행한 프로세스는 다시 큐에 넣지 않고 그대로 종료됩니다.

<br />

```swift
import Foundation

struct DoubleStackQueue {
    private var inStack : [Int] = []
    private var outStack : [Int] = []

    mutating func enqueue(_ data: Int) {
        inStack.append(data)
    }

    mutating func dequeue() -> Int? {
        if outStack.isEmpty {
            outStack = inStack.reversed()
            inStack.removeAll()
        }
        return outStack.popLast()
    }

    func checkPriority(_ data: Int) -> Bool {
        if outStack.contains(where: { $0 > data }) {
            return false
        }
        if inStack.contains(where: { $0 > data }) {
            return false
        }
        return true
    }
    
    func getCount() -> Int {
        return inStack.count + outStack.count
    }
}

func solution(_ priorities:[Int], _ location:Int) -> Int {
    var queue = DoubleStackQueue()
    var count = 0, location = location

    for i in 0..<priorities.count {
        queue.enqueue(priorities[i])
    }

    while true {
        let curProcess = queue.dequeue()
        
        if queue.checkPriority(curProcess!) {
            count += 1
            if location == 0 {
                return count
            }
            location -= 1
        } else {
            queue.enqueue(curProcess!)
            if location == 0 {
                location = queue.getCount() - 1
            } else {
                location -= 1
            }
        }
    }
    return count
}
```

- 배운점
  : 배열보다 빠르고, 연결리스트보다 성능이 좋은 double stack queue를 이용하여 문제를 해결했다. dequeue를 할때마다 location 위치를 -1 해주고, 현재 프로세스보다 우선순위가 높은 프로세스가 있는 경우 enqueue를 해야하기 때문에 location = queue.getCount() - 1을 해줬다. 현재 프로세스보다 우선순위가 높은 프로세스가 없을 때 location == 0 이면 count를 반환해준다.

<br />


#### Programmers - 다리를 지나는 트럭

\- 여러 대의 트럭이 일차선 다리를 정해진 순으로 건널 때 모든 트럭이 다리를 건너는데 걸리는 최소 시간을 구하는 문제<br />


```swift
import Foundation

struct DoubleStackQueue {
    private var inStack : [Int] = []
    private var outStack : [Int] = []
    
    init(){}
    
    init(_ inStack: [Int]) {
        self.inStack = inStack
    }
    
    mutating func enqueue(_ data: Int) {
        inStack.append(data)
    }

    mutating func dequeue() -> Int? {
        if outStack.isEmpty {
            outStack = inStack.reversed()
            inStack.removeAll()
        }
        return outStack.popLast()
    }
}

func solution(_ bridge_length:Int, _ weight:Int, _ truck_weights:[Int]) -> Int {
    // 다리의 길이만큼 0을 채운 큐 생성
    var bridge = DoubleStackQueue(Array(repeating: 0, count: bridge_length))
    var time = bridge_length, bridgeWeight = 0
    var index = 0

    // 트럭이 다리를 모두 건널 때까지
    while index < truck_weights.count {
        // 1초에 1씩 이동
        time += 1
        bridgeWeight -= bridge.dequeue() ?? 0
        
        if bridgeWeight + truck_weights[index] <= weight {
            bridgeWeight += truck_weights[index]
            bridge.enqueue(truck_weights[index])
            index += 1
        } else {
            bridge.enqueue(0)
        }
    }
    return time
}
```

- 배운점
  : 처음에 다리를 1초에 1씩 이동할 수 있다는 걸 제대로 이해하지 못해 시간이 많이 걸렸다.. 검색을 통해 힌트를 얻어 문제를 해결했는데, 다리를 1초에 1씩 이동하는 걸 잘 이해하는게 중요한것 같다.<br />
  1. 맨 처음에는 다리에 어떤 트럭도 존재하지 않기 때문에 다리 길이만큼 0을 채운 큐(다리)를 생성해 준다. 
  2. time = bridge_length 로 설정하고, 트럭이 다리를 모두 건널 때까지 3,4를 반복한다. (1초에 1씩 이동하기 때문에 time은 최소 bridge_length 이다.)
  3. time을 1씩 증가시키고, 다리 위에 올라간 트럭의 전체 무게에서 다리의 첫번째 요소를 빼준다. (1초에 1씩 이동) <br />
  4. 다리 위의 전체 무게와 다음 트럭의 무게 합이 최대 하중보다 작을 경우<br />
    - 다리 무게에 트럭의 무게를 더하고, 다리에 다음 트럭을 추가해준 후 인덱스를 증가시킨다. <br />
  4. 다리 위의 전체 무게와 다음 트럭의 무게 합이 최대 하중보다 클 경우<br />
    - 다리에 트럭이 올라갈 수 없으므로 0을 추가해준다.

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
