---
title: "코딩테스트 연습"
author: gyeomji
date: 2024-10-03 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, 코딩테스트]
pin: false
math: true
mermaid: true
---

## [연결 리스트](https://gyeom-ji.github.io/posts/list/)

---

#### [Add Two Numbers](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0002-add-two-numbers)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.<br/>

![addTwoNumbers](/assets/img/addTwoNumbers.jpeg)

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -ListNode? {
        var l1 = l1, l2 = l2
        var head = ListNode()
        var cur = head
        var temp = 0

        while true {
            let val = getVal(l1) + getVal(l2) + temp
            cur.val = val % 10
            temp = val / 10

            l1 = l1?.next
            l2 = l2?.next

            if l1 != nil || l2 != nil || temp != 0 {
                cur.next = ListNode()
                cur = cur.next!
            } else {
                break
            }
        }

        return head
    }

    func getVal(_ node: ListNode?) -Int {
        guard let node = node else {return 0}
        return node.val
    }
}
```

</div>
</details>


#### [Remove Nth Node From End of List](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0019-remove-nth-node-from-end-of-list)

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.<br/>
![removeNthNodeFromEndOfList](/assets/img/removeNthNodeFromEndOfList.jpeg)

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
        var dummy = ListNode(0, head)
        var left : ListNode? = dummy
        var right : ListNode? = dummy

        for _ in 0..<n {
            right = right?.next
        } 

        while right?.next != nil {
            right = right?.next
            left = left?.next
        }

        left?.next = left?.next?.next

        return dummy.next
    }
}
```

</div>
</details>


#### [Merge k Sorted Lists](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0023-merge-k-sorted-lists)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.<br/>
Merge all the linked-lists into one sorted linked-list and return it.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
        var dummy = ListNode(0)
        var lists = lists
        var cur : ListNode? = dummy
        
        if lists.count == 0 { return nil }
        
    loop:
        while true {
            var min = Int.max
            var cnt = 0
            
            for i in 0..<lists.count {
                if let value = getVal(lists[i]) {
                    if value < min {
                        min = value
                    }
                } else {
                    cnt += 1
                    if cnt == lists.count {
                        break loop
                    }
                    continue
                }
                
            }
            
            for i in 0..<lists.count {
                if let value = getVal(lists[i]) {
                    if value == min {
                        cur?.next = ListNode(min)
                        cur = cur?.next
                        lists[i] = lists[i]?.next
                    }
                }
            }
        }
        
        return dummy.next
    }
    
    func getVal(_ node: ListNode?) -> Int? {
        guard let node = node else {return nil}
        return node.val
    }
}
```

</div>
</details>

<br/>

## 누적합 Prefix Sum

---

: <span style="color:#9fb584">**나열된 수의 누적된 합**</span>으로, 수열 An에 대해서 각 인덱스까지의 구간의 합을 구하는 것이다. Prefix Sum의 <span style="color:#9fb584">**각 요소는 해당 인덱스까지의 부분합(Partial Sum)**</span>을 의미한다. 시간복잡도 측면에서 이득을 취하는 알고리즘이다.

$$sum[i] \ = \ sum[i-1] \ + \ arr[i]$$

``` ex
배열 [1,2,3,4,5,0]으로 각 구간까지 합을 구하는 배열을 구할 때 

[ 각 인덱스까지 값을 반복하여 구하는 방식 (최악의 경우 O(n^2)) ]

1
1 + 2
1 + 2 + 3
1 + 2 + 3 + 4
1 + 2 + 3 + 4 + 5
1 + 2 + 3 + 4 + 5 + 0

[ 누적합 O(n) ]

1
1 + 2
3 + 3
6 + 4
10 + 5
15 + 0

```

||||||||
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|An|1|2|3|4|5|0|
|Prefix Sum|1|3|6|10|15|15|




### 구간합 Range Sum

: 누적합을 이용하여 O(N + M)의 시간으로 구간합을 구할 수 있다.

$$sum[j] \ - \ sum[i-1] \ = \ 구간 [i, \ j]의 합$$


``` ex

[ 단순 반복을 이용해 구간합을 구하는 방식 ]

구간의 길이가 M인 경우 O(M)
N개의 구간의 길이가 M인 구간합을 구하는 경우 O(NM)

[ 누적합을 이용하여 구간합을 구하는 방식 ]

Sn까지의 구간합을 모두 구하는 시간 복잡도 (누적합) O(M) 
구간합 연산 (sum[j] - sum[i-1] = 구간 [i, j]의 합) O(1)
총 O(N + M)

```

#### [Product of Array Except Self](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0238-product-of-array-except-self)

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.<br/>
The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.<br/>
You must write an algorithm that runs in `O(n)` time and without using the division operation.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var anw = [Int]()
        var cnt = nums.count
        var left = Array(repeating: 1, count: cnt)
        var right = Array(repeating: 1, count: cnt)

        for i in 1..<cnt {
            left[i] = left[i - 1] * nums[i - 1]
        }

        for i in (0..<cnt - 1).reversed() {
            right[i] = right[i + 1] * nums[i + 1]
        }

        for i in 0..<cnt {
            anw.append(left[i] * right[i])
        }

        return anw
    }
}
```

</div>
</details>


#### [Contiguous Array](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0525-contiguous-array)

Given a binary array `nums`, return the maximum length of a contiguous subarray with an equal number of `0` and `1`.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

class Solution {
    func findMaxLength(_ nums: [Int]) -> Int {
        var anw = 0
        var cnt = 0
        var dict = [0 : -1]

        for i in 0..<nums.count {
            cnt += nums[i] == 1 ? 1 : -1 

            if let prevIndex = dict[cnt] {
                anw = max(anw, i - prevIndex)
            } else {
                dict[cnt] = i
            }
        }
        return anw
    }
}

```

</div>
</details>

#### [Binary Subarrays With Sum](https://github.com/gyeom-ji/codingtest/tree/main/LeetCode/0930-binary-subarrays-with-sum)

Given a binary array `nums` and an integer `goal`, return the number of non-empty **subarrays** with a sum `goal`.<br/>
A subarray is a contiguous part of the array.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

class Solution {
    func numSubarraysWithSum(_ nums: [Int], _ goal: Int) -> Int {
        var anw = 0
        var left = 0, right = 0
        var sum = 0, cnt = nums.count

        while left < cnt {
            sum += nums[right]

            if sum == goal {
                anw += 1
            } 

            right += 1
            
            if sum > goal || right == cnt {
                sum = 0
                left += 1
                right = left 
            }
        }
        return anw
    }
}

```

</div>
</details>

#### [부분합](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1806.%E2%80%85%EB%B6%80%EB%B6%84%ED%95%A9)

10,000 이하의 자연수로 이루어진 길이 N짜리 수열이 주어진다. 이 수열에서 연속된 수들의 부분합 중에 그 합이 S 이상이 되는 것 중, 가장 짧은 것의 길이를 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{ $0 == " "}.map{Int($0)!}
var arr = readLine()!.split{ $0 == " "}.map{Int($0)!}
var cnt = Int.max, sum = 0
var left = 0, right = 0

while left < input[0] {
    if sum < input[1] {
        if right < input[0] {
            sum += arr[right]
            right += 1
        } else {
            break
        }
    } else {
        cnt = min(cnt, right - left)
        sum -= arr[left]
        left += 1
    }
}

print(cnt == Int.max ? 0 : cnt)
```

</div>
</details>


#### [수들의 합 4](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/2015.%E2%80%85%EC%88%98%EB%93%A4%EC%9D%98%E2%80%85%ED%95%A9%E2%80%854)

A[1], A[2], ..., A[N]의 N개의 정수가 저장되어 있는 배열이 있다. 이 배열 A의 부분합이란 1 ≤ i ≤ j ≤ N인 정수 i와 j에 대해 A[i]부터 A[j]까지의 합을 말한다.<br/>

N과 A[1], A[2], ..., A[N]이 주어졌을 때, 이러한 N×(N+1)/2개의 부분합 중 합이 K인 것이 몇 개나 있는지를 구하는 프로그램을 작성하시오.


``` Tip
💡 누적합을 구한다. 
 　 구간합 공식 sum[j] - sum[i - 1] = target(구간합)을 이용한다.
  　sum[j] - target = sum[i - 1]
  　구간합 K인 구간의(sum[i - 1]) 개수를 anw에 더한다.
```


<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{ $0 == " "}.map{Int($0)!}
var arr = readLine()!.split{ $0 == " "}.map{Int($0)!}
var sum = Array(repeating: 0, count: arr.count)

// sum[i] == k 일 경우를 위해 초기값 설정
var sumDict = [0 : 1]
var anw = 0
sum[0] = arr[0]

// 누적합
for i in 1..<input[0] {
    sum[i] = sum[i - 1] + arr[i]
}

// 구간합
// sum[j] - sum[i - 1] = target(구간합)
// sum[j] - target = sum[i - 1]
// sum[i - 1]의 개수를 anw에 +

for j in 0..<input[0] {
    anw += sumDict[sum[j] - input[1], default: 0]
    sumDict[sum[j], default: 0] += 1
}

print(anw)
```

</div>
</details>

<br/>

## [위상 정렬](https://gyeom-ji.github.io/posts/graph/)

---

#### [ACM Craft](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1005.%E2%80%85ACM%E2%80%85Craft)

서기 2012년! 드디어 2년간 수많은 국민들을 기다리게 한 게임 ACM Craft (Association of Construction Manager Craft)가 발매되었다.<br/>

이 게임은 지금까지 나온 게임들과는 다르게 ACM크래프트는 다이나믹한 게임 진행을 위해 건물을 짓는 순서가 정해져 있지 않다. 즉, 첫 번째 게임과 두 번째 게임이 건물을 짓는 순서가 다를 수도 있다. 매 게임시작 시 건물을 짓는 순서가 주어진다. 또한 모든 건물은 각각 건설을 시작하여 완성이 될 때까지 Delay가 존재한다.<br/>

![star](/assets/img/star.jpeg)<br/>

위의 예시를 보자.<br/>

이번 게임에서는 다음과 같이 건설 순서 규칙이 주어졌다. 1번 건물의 건설이 완료된다면 2번과 3번의 건설을 시작할수 있다. (동시에 진행이 가능하다) 그리고 4번 건물을 짓기 위해서는 2번과 3번 건물이 모두 건설 완료되어야지만 4번건물의 건설을 시작할수 있다.<br/>

따라서 4번건물의 건설을 완료하기 위해서는 우선 처음 1번 건물을 건설하는데 10초가 소요된다. 그리고 2번 건물과 3번 건물을 동시에 건설하기 시작하면 2번은 1초뒤에 건설이 완료되지만 아직 3번 건물이 완료되지 않았으므로 4번 건물을 건설할 수 없다. 3번 건물이 완성되고 나면 그때 4번 건물을 지을수 있으므로 4번 건물이 완성되기까지는 총 120초가 소요된다.<br/>

프로게이머 최백준은 애인과의 데이트 비용을 마련하기 위해 서강대학교배 ACM크래프트 대회에 참가했다! 최백준은 화려한 컨트롤 실력을 가지고 있기 때문에 모든 경기에서 특정 건물만 짓는다면 무조건 게임에서 이길 수 있다. 그러나 매 게임마다 특정건물을 짓기 위한 순서가 달라지므로 최백준은 좌절하고 있었다. 백준이를 위해 특정건물을 가장 빨리 지을 때까지 걸리는 최소시간을 알아내는 프로그램을 작성해주자.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let T = file.readInt()

var str = ""
var dp = [Int]()
var queue = [Int]()

/// dfs + dp
for _ in 0..<T {
    let N = file.readInt()
    let K = file.readInt()
    var sequence = Array(repeating:Array(repeating: 0, count: 0), count: N + 1)
    var timeArr = [0]
    dp = Array(repeating: Int.max, count: N + 1)
    
    for _ in 0..<N {
        timeArr.append(file.readInt())
    }
 
    for _ in 0..<K {
        let element = [file.readInt(), file.readInt()]
        sequence[element[1]].append(element[0])
    }
    
    let W = file.readInt()
   
    str += "\(dfs(W, sequence, timeArr))\n"
}

print(str)

func dfs(_ cur: Int,_ sequence: [[Int]], _ timeArr: [Int]) -> Int {
    var maxTime = 0
    for next in sequence[cur] {
        if dp[next] == Int.max {
            maxTime = max(dfs(next, sequence, timeArr), maxTime)
        } else {
            maxTime = max(dp[next], maxTime)
        }
    }
    // cur 최소 건설 시간 = 자신을 짓기 위한 건물들의 최대 건설 시간 + 자기 건설 시간
    dp[cur] = min(dp[cur], maxTime + timeArr[cur])
    return dp[cur]
}

/// 위상 정렬 Kahn’s Algorithms
////선행자 개수
//var inDegree = [0]
//
//for _ in 0..<T {
//    let N = file.readInt()
//    let K = file.readInt()
//    var sequence = Array(repeating:Array(repeating: 0, count: 0), count: N + 1)
//    var timeArr = [0]
//    
//    dp = Array(repeating: 0, count: N + 1)
//    inDegree = Array(repeating: 0, count: N + 1)
//    
//    for _ in 0..<N {
//        timeArr.append(file.readInt())
//    }
//    
//    for _ in 0..<K {
//        let element = [file.readInt(), file.readInt()]
//        sequence[element[0]].append(element[1])
//        // 진입 차수 계산
//        inDegree[element[1]] += 1
//    }
//    
//    // 진입 차수 0인 빌딩 큐에 삽입
//    for i in 1...N {
//        if inDegree[i] == 0 {
//            queue.append(i)
//        }
//    }
//
//    let W = file.readInt()
//    bfs(sequence, timeArr)
//
//    // W 건물 짓는 시간 + W를 짓기 위한 건물들의 최소 건설 시간
//    str += "\(dp[W] + timeArr[W])\n"
//}
//
//print(str)
//
//func bfs(_ sequence: [[Int]], _ timeArr: [Int]) {
//    while !queue.isEmpty {
//        let now = queue.removeFirst()
//
//        for next in sequence[now] {
//            // 자신을 짓기 위한 건물들의 최소 건설 시간 갱신
//            dp[next] = max(dp[next], dp[now] + timeArr[now])
//            
//            // 다음 빌딩의 선행자 제거
//            inDegree[next] -= 1
//
//            // 진입 차수 0이면 큐에 삽입
//            if inDegree[next] == 0 {
//                queue.append(next)
//            }
//        }
//    }
//}
```

</div>
</details>


#### [게임 개발](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1516.%E2%80%85%EA%B2%8C%EC%9E%84%E2%80%85%EA%B0%9C%EB%B0%9C)

숌 회사에서 이번에 새로운 전략 시뮬레이션 게임 세준 크래프트를 개발하기로 하였다. 핵심적인 부분은 개발이 끝난 상태고, 종족별 균형과 전체 게임 시간 등을 조절하는 부분만 남아 있었다.<br/>

게임 플레이에 들어가는 시간은 상황에 따라 다를 수 있기 때문에, 모든 건물을 짓는데 걸리는 최소의 시간을 이용하여 근사하기로 하였다. 물론, 어떤 건물을 짓기 위해서 다른 건물을 먼저 지어야 할 수도 있기 때문에 문제가 단순하지만은 않을 수도 있다. 예를 들면 스타크래프트에서 벙커를 짓기 위해서는 배럭을 먼저 지어야 하기 때문에, 배럭을 먼저 지은 뒤 벙커를 지어야 한다. 여러 개의 건물을 동시에 지을 수 있다.<br/>

편의상 자원은 무한히 많이 가지고 있고, 건물을 짓는 명령을 내리기까지는 시간이 걸리지 않는다고 가정하자.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

let N = Int(readLine()!)!
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var inDegree = Array(repeating: 0, count: N + 1)
var time = Array(repeating: 0, count: N + 1)
var queue = [Int]()
var dp = Array(repeating: 0, count: N + 1)

for i in 1...N {
    let input = readLine()!.split{$0 == " "}.map{Int($0)!}
    if input.count > 2 {
        for j in 1..<input.count - 1 {
            inDegree[i] += 1
            graph[input[j]].append(i)
        }
    }
    time[i] = input[0]
}

for i in 1...N {
    if inDegree[i] == 0 {
        queue.append(i)
        dp[i] = time[i]
    }
}
bfs()

for i in 1...N {
    print(dp[i])
}

func bfs() {
    while !queue.isEmpty {
        let cur = queue.removeFirst()
        
        for next in graph[cur] {
            inDegree[next] -= 1
            
            if inDegree[next] == 0 {
                queue.append(next)
            }
            // dp[next] 값과 dp[cur] + time[next] 값 중 더 큰 값을 dp[next]에 저장
            dp[next] = max(dp[next], dp[cur] + time[next])
        }
    }
}

/// ex
/// 3
/// 5 -1
/// 2 -1
/// 1 1 2 -1
/// 의 경우 5 2 6 이 정답이 되어야 함
```

</div>
</details>


#### [문제집](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1766.%E2%80%85%EB%AC%B8%EC%A0%9C%EC%A7%91)

민오는 1번부터 N번까지 총 N개의 문제로 되어 있는 문제집을 풀려고 한다. 문제는 난이도 순서로 출제되어 있다. 즉 1번 문제가 가장 쉬운 문제이고 N번 문제가 가장 어려운 문제가 된다.<br/>

어떤 문제부터 풀까 고민하면서 문제를 훑어보던 민오는, 몇몇 문제들 사이에는 '먼저 푸는 것이 좋은 문제'가 있다는 것을 알게 되었다. 예를 들어 1번 문제를 풀고 나면 4번 문제가 쉽게 풀린다거나 하는 식이다. 민오는 다음의 세 가지 조건에 따라 문제를 풀 순서를 정하기로 하였다.<br/>

1. N개의 문제는 모두 풀어야 한다.<br/>
2. 먼저 푸는 것이 좋은 문제가 있는 문제는, 먼저 푸는 것이 좋은 문제를 반드시 먼저 풀어야 한다.<br/>
3. 가능하면 쉬운 문제부터 풀어야 한다.<br/>
4. 
예를 들어서 네 개의 문제가 있다고 하자. 4번 문제는 2번 문제보다 먼저 푸는 것이 좋고, 3번 문제는 1번 문제보다 먼저 푸는 것이 좋다고 하자. 만일 4-3-2-1의 순서로 문제를 풀게 되면 조건 1과 조건 2를 만족한다. 하지만 조건 3을 만족하지 않는다. 4보다 3을 충분히 먼저 풀 수 있기 때문이다. 따라서 조건 3을 만족하는 문제를 풀 순서는 3-1-4-2가 된다.<br/>

문제의 개수와 먼저 푸는 것이 좋은 문제에 대한 정보가 주어졌을 때, 주어진 조건을 만족하면서 민오가 풀 문제의 순서를 결정해 주는 프로그램을 작성하시오.

``` Tip
💡 가장 쉬운(낮은 숫자)문제 부터 풀어야하기 때문에 queue를 정렬해준다.
```

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt(), M = file.readInt()

var graph : [[Int]] = Array(repeating: [], count: N + 1)
var queue = [Int]()
var preArr = Array(repeating: 0, count: N + 1)
var order = ""

for _ in 0..<M {
    let pre = file.readInt(), suc = file.readInt()
    graph[pre].append(suc)
    preArr[suc] += 1
}

for i in 1...N {
    if preArr[i] == 0 {
        queue.append(i)
    }
}

bfs()

print(order)

func bfs() {
    while !queue.isEmpty {
        // 가장 쉬운(낮은 숫자)문제 부터
        queue.sort(by: <)
        let now = queue.removeFirst()

        order += "\(now) "

        for next in graph[now] {
            preArr[next] -= 1
            if preArr[next] == 0 {
                queue.append(next)
            }
        }
    }
}
```

</div>
</details>


#### [작업](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/2056.%E2%80%85%EC%9E%91%EC%97%85)

수행해야 할 작업 N개 (3 ≤ N ≤ 10000)가 있다. 각각의 작업마다 걸리는 시간(1 ≤ 시간 ≤ 100)이 정수로 주어진다.<br/>

몇몇 작업들 사이에는 선행 관계라는 게 있어서, 어떤 작업을 수행하기 위해 반드시 먼저 완료되어야 할 작업들이 있다. 이 작업들은 번호가 아주 예쁘게 매겨져 있어서, K번 작업에 대해 선행 관계에 있는(즉, K번 작업을 시작하기 전에 반드시 먼저 완료되어야 하는) 작업들의 번호는 모두 1 이상 (K-1) 이하이다. 작업들 중에는, 그것에 대해 선행 관계에 있는 작업이 하나도 없는 작업이 반드시 하나 이상 존재한다. (1번 작업이 항상 그러하다)<br/>

모든 작업을 완료하기 위해 필요한 최소 시간을 구하여라. 물론, 서로 선행 관계가 없는 작업들은 동시에 수행 가능하다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt()
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var timeArr = Array(repeating: 0, count: N + 1)
var queue = [Int]()
var inDegree = Array(repeating: 0, count: N + 1)
var dp = Array(repeating: 0, count: N + 1)
var check = Array(repeating: false, count: N + 1)
        

for i in 1...N {
    let time = file.readInt()
    let cnt = file.readInt()
    
    if cnt != 0 {
        for _ in 0..<cnt {
            let pre = file.readInt()
            graph[pre].append(i)
            check[pre] = true
            check[i] = true
            inDegree[i] += 1
        }
    }
    timeArr[i] = time
}

for i in 1...N {
    if inDegree[i] == 0 {
        queue.append(i)
    }
}

bfs()

print(dp.max()!)

func bfs() {
    while !queue.isEmpty {
        let now = queue.removeFirst()
        // 현재 작업 시간 +
        dp[now] += timeArr[now]

        for next in graph[now] {
            // 다음 작업 시간, 현재 작업 시간 중 max 값 저장
            dp[next] = max(dp[next], dp[now])
            inDegree[next] -= 1
            
            if inDegree[next] == 0 {
                queue.append(next)
            }
        }
    }
}

```

</div>
</details>


#### [줄세우기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/2252.%E2%80%85%EC%A4%84%E2%80%85%EC%84%B8%EC%9A%B0%EA%B8%B0)

N명의 학생들을 키 순서대로 줄을 세우려고 한다. 각 학생의 키를 직접 재서 정렬하면 간단하겠지만, 마땅한 방법이 없어서 두 학생의 키를 비교하는 방법을 사용하기로 하였다. 그나마도 모든 학생들을 다 비교해 본 것이 아니고, 일부 학생들의 키만을 비교해 보았다.<br/>

일부 학생들의 키를 비교한 결과가 주어졌을 때, 줄을 세우는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt()
let M = file.readInt()
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var inDegree = Array(repeating: 0, count: N + 1)
var queue = [Int]()
var order = ""

for _ in 0..<M {
    let pre = file.readInt()
    let suc = file.readInt()
    
    inDegree[suc] += 1
    graph[pre].append(suc)
}

for i in 1...N {
    if inDegree[i] == 0 {
        queue.append(i)
    }
}

bfs()
print(order)

func bfs(){
    while !queue.isEmpty {
        let now = queue.removeFirst()
        order += "\(now) "
        for next in graph[now] {
            inDegree[next] -= 1
            if inDegree[next] == 0 {
                queue.append(next)
            }
        }
    }
}
```

</div>
</details>


#### [음악프로그램](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/2623.%E2%80%85%EC%9D%8C%EC%95%85%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8)

인터넷 방송 KOI(Korea Open Internet)의 음악 프로그램 PD인 남일이는 자기가 맡은 프로그램 '뮤직 KOI'에서 가수의 출연 순서를 정하는 일을 매우 골치 아파한다. 순서를 정하기 위해서는 많은 조건을 따져야 한다. 그래서 오늘 출연 예정인 여섯 팀의 가수에 대해서 남일이가 보조 PD 세 명에게 각자 담당한 가수의 출연 순서를 정해오게 하였다. 보조 PD들이 가져온 것은 아래와 같다.<br/>

1 4 3<br/>
6 2 5 4<br/>
2 3<br/>

첫 번째 보조 PD는 1번 가수가 먼저, 다음에 4번 가수, 다음에 3번 가수가 출연하기로 순서를 정했다. 두 번째 보조 PD는 6번, 2번, 5번, 4번 순으로 자기 담당 가수들의 순서를 정했다. 한 가수를 여러 보조 PD가 담당할 수도 있다. 마지막으로, 세 번째 보조 PD는 2번 먼저, 다음에 3번으로 정했다.<br/>

남일이가 할 일은 이 순서들을 모아서 전체 가수의 순서를 정하는 것이다. 남일이는 잠시 생각을 하더니 6 2 1 5 4 3으로 순서를 정했다. 이렇게 가수 순서를 정하면 세 보조 PD가 정해온 순서를 모두 만족한다. 물론, 1 6 2 5 4 3으로 전체 순서를 정해도 괜찮다.<br/>

경우에 따라서 남일이가 모두를 만족하는 순서를 정하는 것이 불가능할 수도 있다. 예를 들어, 세 번째 보조 PD가 순서를 2 3 대신에 3 2로 정해오면 남일이가 전체 순서를 정하는 것이 불가능하다. 이번에 남일이는 우리 나라의 월드컵 4강 진출 기념 음악제의 PD를 맡게 되었는데, 출연 가수가 아주 많다. 이제 여러분이 해야 할 일은 보조 PD들이 가져 온 순서들을 보고 남일이가 가수 출연 순서를 정할 수 있도록 도와 주는 일이다.<br/>

보조 PD들이 만든 순서들이 입력으로 주어질 때, 전체 가수의 순서를 정하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt()
let M = file.readInt()
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var inDegree = Array(repeating: 0, count: N + 1)
var queue = [Int]()
var order = "", cnt = 0

for _ in 0..<M {
    let cnt = file.readInt()
    var pre = file.readInt()
    
    if cnt > 1 {
        for i in 1..<cnt {
            let suc = file.readInt()
            graph[pre].append(suc)
            inDegree[suc] += 1
            pre = suc
        }
    }
}

for i in 1...N {
    if inDegree[i] == 0 {
        queue.append(i)
    }
}

bfs()

if cnt == N {
    print(order)
} else {
    print("0")
}

func bfs(){
    while !queue.isEmpty {
        let now = queue.removeFirst()
        order += "\(now)\n"
        cnt += 1
        for next in graph[now] {
            inDegree[next] -= 1
            if inDegree[next] == 0 {
                queue.append(next)
            }
        }
    }
}
```

</div>
</details>


#### [선수과목](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/14567.%E2%80%85%EC%84%A0%EC%88%98%EA%B3%BC%EB%AA%A9%E2%80%85%EF%BC%88Prerequisite%EF%BC%89)

올해 Z대학 컴퓨터공학부에 새로 입학한 민욱이는 학부에 개설된 모든 전공과목을 듣고 졸업하려는 원대한 목표를 세웠다. 어떤 과목들은 선수과목이 있어 해당되는 모든 과목을 먼저 이수해야만 해당 과목을 이수할 수 있게 되어 있다. 공학인증을 포기할 수 없는 불쌍한 민욱이는 선수과목 조건을 반드시 지켜야만 한다. 민욱이는 선수과목 조건을 지킬 경우 각각의 전공과목을 언제 이수할 수 있는지 궁금해졌다. 계산을 편리하게 하기 위해 아래와 같이 조건을 간소화하여 계산하기로 하였다.<br/>

1. 한 학기에 들을 수 있는 과목 수에는 제한이 없다.<br/>
2. 모든 과목은 매 학기 항상 개설된다.<br/>

모든 과목에 대해 각 과목을 이수하려면 최소 몇 학기가 걸리는지 계산하는 프로그램을 작성하여라.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt()
let M = file.readInt()
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var inDegree = Array(repeating: 0, count: N + 1)
var queue = [Int]()
var dp = Array(repeating: 1, count: N + 1)

for _ in 0..<M {
    let pre = file.readInt()
    let suc = file.readInt()
    
    graph[pre].append(suc)
    inDegree[suc] += 1
}

for i in 1...N {
    if inDegree[i] == 0 {
        queue.append(i)
    }
}

bfs()

for i in 1...N {
    print(dp[i], terminator: " ")
}

func bfs(){
    while !queue.isEmpty {
        let now = queue.removeFirst()
        for next in graph[now] {
            dp[next] = dp[now] + 1
            
            inDegree[next] -= 1
            if inDegree[next] == 0 {
                queue.append(next)
            }
        }
    }
}
```

</div>
</details>


#### [최소비용 구하기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1916.%E2%80%85%EC%B5%9C%EC%86%8C%EB%B9%84%EC%9A%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0)

N개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 M개의 버스가 있다. 우리는 A번째 도시에서 B번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 한다. A번째 도시에서 B번째 도시까지 가는데 드는 최소비용을 출력하여라. 도시의 번호는 1부터 N까지이다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

let N = Int(readLine()!)!
let M = Int(readLine()!)!
var graph = Array(repeating: Array(repeating: (0,0), count: 0), count: N + 1)

for _ in 0..<M {
    let input = readLine()!.split{$0 == " "}.map{Int($0)!}
    graph[input[0]].append((input[1], input[2]))
}

let target = readLine()!.split{$0 == " "}.map{Int($0)!}

print(dijkstra(target[0])[target[1]])

func dijkstra(_ cur: Int) -> [Int] {
    var visited = Array(repeating: false, count: N + 1)
    var distance = Array(repeating: Int.max, count: N + 1)
    
    distance[cur] = 0
    
    for _ in 1...N {
        var minVertex = -1
        var minDist = Int.max

        // 미방문 정점 중 distance 값이 최소인 minVertex 찾기
        for j in 1...N {
            if !visited[j] && distance[j] < minDist {
                minDist = distance[j]
                minVertex = j
            }
        }
        
        // 도시가 연결되지 않은 경우 제외
        if minVertex != -1 {
            // 방문 표시
            visited[minVertex] = true
            
            // minVertex에 인접한 각 정점 중
            for (next, cost) in graph[minVertex] {
                // 미방문된 정점에 대해
                if !visited[next] {
                    let curDist = distance[next]
                    let newDist = distance[minVertex] + cost
                    
                    if newDist < curDist {
                        distance[next] = newDist // 간선 완화
                    }
                }
            }
        }
    }
    return distance
}
```

</div>
</details>


#### [Strahler 순서](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/9470.%E2%80%85Strahler%E2%80%85%EC%88%9C%EC%84%9C)

지질학에서 하천계는 유향그래프로 나타낼 수 있다. 강은 간선으로 나타내며, 물이 흐르는 방향이 간선의 방향이 된다. 노드는 호수나 샘처럼 강이 시작하는 곳, 강이 합쳐지거나 나누어지는 곳, 바다와 만나는 곳이다.<br/>

![strahler](/assets/img/strahler.png)<br/>

네모 안의 숫자는 순서를 나타내고, 동그라미 안의 숫자는 노드 번호를 나타낸다.<br/>

하천계의 Strahler 순서는 다음과 같이 구할 수 있다.<br/>

- 강의 근원인 노드의 순서는 1이다.<br/>
- 나머지 노드는 그 노드로 들어오는 강의 순서 중 가장 큰 값을 i라고 했을 때, 들어오는 모든 강 중에서 Strahler 순서가 i인 강이 1개이면 순서는 i, 2개 이상이면 순서는 i+1이다.<br/>

하천계의 순서는 바다와 만나는 노드의 순서와 같다. 바다와 만나는 노드는 항상 1개이며, 위의 그림의 Strahler 순서는 3이다.<br/>

하천계의 정보가 주어졌을 때, Strahler 순서를 구하는 프로그램을 작성하시오.<br/>

실제 강 중에서 Strahler 순서가 가장 큰 강은 아마존 강(12)이며, 미국에서 가장 큰 값을 갖는 강은 미시시피 강(10)이다.<br/>

노드 M은 항상 바다와 만나는 노드이다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let T = Int(readLine()!)!
var graph = [[Int]]()
var inDegree = [Int]()
var queue = [Int]()
var dp = [(Int, Int)]()
var anw = ""

for _ in 0..<T {
    let input = readLine()!.split{$0 == " "}.map{Int($0)!}
    graph = Array(repeating: Array(repeating: 0, count: 0), count: input[1] + 1)
    inDegree = Array(repeating: 0, count: input[1] + 1)
    dp = Array(repeating: (0, 0), count: input[1] + 1)
    
    for _ in 0..<input[2] {
        let edge = readLine()!.split{$0 == " "}.map{Int($0)!}
        graph[edge[0]].append(edge[1])
        inDegree[edge[1]] += 1
    }
    
    for i in 1...input[1] {
        if inDegree[i] == 0 {
            dp[i] = (1, 1)
            queue.append(i)
        }
    }
    bfs()
    anw += "\(input[0]) \(dp[input[1]].0)\n"
}

print(anw)

func bfs() {
    while !queue.isEmpty {
        let cur = queue.removeFirst()
        
        for next in graph[cur] {
            inDegree[next] -= 1
            if dp[next].0 == dp[cur].0 {
                dp[next].1 += 1
            } else if dp[next].0 < dp[cur].0 {
                dp[next] = (dp[cur].0, 1)
            }
            
            if inDegree[next] == 0 {
                if dp[next].1 > 1 {
                    dp[next].0 += 1
                }
                queue.append(next)
            }
        }
    }
}
```

</div>
</details>


<br/>

## 최단 경로

---

### [다익스트라](https://gyeom-ji.github.io/posts/dijkstra-algorithm/)

---

#### [발전소 설치](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1277.%E2%80%85%EB%B0%9C%EC%A0%84%EC%86%8C%E2%80%85%EC%84%A4%EC%B9%98)

엄청난 벼락을 맞아 많은 전선들이 끊어져 현재 전력 공급이 중단된 상태이다. 가장 심각한 문제는 1번 발전소에서 N번 발전소로 가는 중간의 전선이 끊어진 것이기에 일단 이 두 발전소를 다시 연결하는게 현재 해결해야할 첫 번째 과제이다.<br/>

발전소는 1번부터 N번까지 번호로 매겨져 2차원 격자 좌표 위에 있다. 그리고 몇몇 전선은 보존된 채 몇몇 발전소를 잇고 있다. 문제는 현재 전선과 발전소의 위치가 주어졌을 때 최소의 전선 길이를 추가로 사용하여 1번 발전소와 N번 발전소를 연결짓는 것이다. 물론 연결 짓는 중간에 다른 발전소를 거쳐갈 수 있다. 단, 안정성 문제로 어떠한 두 발전소 사이의 전선의 길이가 M을 초과할 수는 없다.<br/>

![1277](/assets/img/1277.png)<br/>

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

struct MinHeap<T: Comparable> {
    var heap: [T?] = Array(repeating: nil, count : 1)
    var size = 0
    
    mutating func dequeue() -> T? {
        if size == 0 {return nil}
        let min = heap[1]
        heap.swapAt(1, size)
        heap.removeLast()
        size -= 1
        downheap(1)
        
        return min
    }
    
    private mutating func downheap(_ i : Int) {
        var i = i
        
        while (i * 2 <= size) {
            var k = i * 2
            if k < size && heap[k]! > heap[k + 1]! {k += 1}
            if heap[i]! < heap[k]! {break}
            heap.swapAt(i, k)
            i = k
        }
    }
    
    mutating func enqueue(_ value: T) {
        heap.append(value)
        size += 1
        upheap(size)
    }
    
    private mutating func upheap(_ i : Int) {
        var i = i
        
        while (i > 1 && heap[i/2]! > heap[i]!) {
            heap.swapAt(i, i/2)
            i = i / 2
        }
    }
}

struct Edge: Comparable {
    var adv : Int
    var cost: Double
    
    init(adv: Int, cost: Double) {
        self.adv = adv
        self.cost = cost
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
        lhs.cost < rhs.cost
    }
}

let file = FileIO()
let N = file.readInt(), W = file.readInt()
let M = Double(file.readString())!
var map = Array(repeating:(0,0), count: N + 1)
var distance = Array(repeating:Array(repeating:  Double(Int.max), count: N + 1), count: N + 1)
var minHeap = MinHeap<Edge>()

for i in 1...N {
    let x = file.readInt(), y = file.readInt()
    map[i] = (x,y)
}

for i in 1...N {
    for j in 1...N {
        let newDist = sqrt(pow(Double(abs(map[i].0 - map[j].0)), 2.0) + pow(Double(abs(map[i].1 - map[j].1)), 2.0))
        if newDist <= M {
            distance[i][j] = newDist
            distance[j][i] = newDist
        }
    }
}

for _ in 0..<W {
    let start = file.readInt(), end = file.readInt()
    distance[start][end] = 0
    distance[end][start] = 0
}

print(Int(dikstra(1)[N] * 1000))


func dikstra(_ start: Int) -> [Double] {
    var dp = Array(repeating: Double(Int.max), count: N + 1)
    minHeap.enqueue(Edge(adv: 1, cost: 0))
    dp[start] = 0
    
    while true {
        guard let cur = minHeap.dequeue() else {break}
        
        if dp[cur.adv] < cur.cost { continue }
        
        for next in 1...N {
            if distance[cur.adv][next] == M {continue}
            let newCost = distance[cur.adv][next] + cur.cost
            
            if newCost < dp[next] {
                dp[next] = newCost
                minHeap.enqueue(Edge(adv: next, cost: newCost))
            }
        }
    }
    return dp
}
```

</div>
</details>

#### [특정 거리의 도시 찾기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/18352.%E2%80%85%ED%8A%B9%EC%A0%95%E2%80%85%EA%B1%B0%EB%A6%AC%EC%9D%98%E2%80%85%EB%8F%84%EC%8B%9C%E2%80%85%EC%B0%BE%EA%B8%B0)

어떤 나라에는 1번부터 N번까지의 도시와 M개의 단방향 도로가 존재한다. 모든 도로의 거리는 1이다.<br/>

이 때 특정한 도시 X로부터 출발하여 도달할 수 있는 모든 도시 중에서, 최단 거리가 정확히 K인 모든 도시들의 번호를 출력하는 프로그램을 작성하시오. 또한 출발 도시 X에서 출발 도시 X로 가는 최단 거리는 항상 0이라고 가정한다.<br/>

예를 들어 N=4, K=2, X=1일 때 다음과 같이 그래프가 구성되어 있다고 가정하자.<br/>

![18352](/assets/img/18352.avif)<br/>

이 때 1번 도시에서 출발하여 도달할 수 있는 도시 중에서, 최단 거리가 2인 도시는 4번 도시 뿐이다. 2번과 3번 도시의 경우, 최단 거리가 1이기 때문에 출력하지 않는다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

struct MinHeap<T: Comparable> {
    var heap: [T?] = Array(repeating: nil, count: 1)
    var size = 0
    
    mutating func dequeue() -> T? {
        if size == 0 {return nil}
        let min = heap[1]
        heap.swapAt(1, size)
        heap.removeLast()
        size -= 1
        downheap(1)
        return min
    }
    
    private mutating func downheap(_ i: Int) {
        var i = i
        while (i * 2 <= size) {
            var k = i * 2
            if k < size && heap[k]! > heap[k + 1]! { k += 1 }
            if heap[i]! < heap[k]! {break}
            heap.swapAt(i, k)
            i = k
        }
    }
    
    mutating func enqueue(_ value: T) {
        heap.append(value)
        size += 1
        upheap(size)
    }
    
    private mutating func upheap(_ i: Int) {
        var i = i
        while (i > 1 && heap[i]! < heap[i/2]! ) {
            heap.swapAt(i, i/2)
            i = i/2
        }
    }
}

struct Edge: Comparable {
    var adj : Int
    var cost : Int
    init(adj: Int, cost: Int) {
        self.adj = adj
        self.cost = cost
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
        lhs.cost < rhs.cost
    }
}

let file = FileIO()
let N = file.readInt(), M = file.readInt(), K = file.readInt(), X = file.readInt()
var graph = Array(repeating: Array(repeating: 0, count: 0), count: N + 1)
var minHeap = MinHeap<Edge>()

for _ in 0..<M {
    let A = file.readInt(), B = file.readInt()
    graph[A].append(B)
}

var distance = dijkstra(X)
var check = false

for i in 1...N {
    if distance[i] == K {
        print(i)
        check = true
    }
}

if !check {
    print(-1)
}

func dijkstra(_ start: Int) -> [Int] {
    var distance = Array(repeating: Int.max, count: N + 1)
    distance[start] = 0
    minHeap.enqueue(Edge(adj: start, cost: 0))
    
    while true {
        guard let cur = minHeap.dequeue() else {break}
        
        if distance[cur.adj] < cur.cost {continue}
        
        for next in graph[cur.adj] {
            if cur.cost + 1 < distance[next] {
                distance[next] = cur.cost + 1
                minHeap.enqueue(Edge(adj: next, cost: cur.cost + 1))
            }
        }
    }
    return distance
}
```

</div>
</details>


#### [파티](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1238.%E2%80%85%ED%8C%8C%ED%8B%B0)

N개의 숫자로 구분된 각각의 마을에 한 명의 학생이 살고 있다.<br/>

어느 날 이 N명의 학생이 X (1 ≤ X ≤ N)번 마을에 모여서 파티를 벌이기로 했다. 이 마을 사이에는 총 M개의 단방향 도로들이 있고 i번째 길을 지나는데 Ti(1 ≤ Ti ≤ 100)의 시간을 소비한다.<br/>

각각의 학생들은 파티에 참석하기 위해 걸어가서 다시 그들의 마을로 돌아와야 한다. 하지만 이 학생들은 워낙 게을러서 최단 시간에 오고 가기를 원한다.<br/>

이 도로들은 단방향이기 때문에 아마 그들이 오고 가는 길이 다를지도 모른다. N명의 학생들 중 오고 가는데 가장 많은 시간을 소비하는 학생은 누구일지 구하여라.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -> UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -> Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -> String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

struct MinHeap<T: Comparable> {
    private var heap : [T?] = Array(repeating: nil, count: 1)
    private var size = 0
    
    mutating func creatHeap() {
        for i in stride(from: size/2, to: 0, by: -1) {
            downheap(i)
        }
    }
    
    mutating func dequeue() -> T? {
        if size == 0 {return nil}
        
        let min = heap[1]
        heap.swapAt(1, size)
        heap.removeLast()
        size -= 1
        downheap(1)
        return min
    }
    
    private mutating func downheap(_ i: Int) {
        var i = i
        while (2 * i <= size) {
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

struct Edge: Comparable {
    let adjvertex : Int
    let cost : Int
    
    init(adjvertex: Int, cost: Int) {
        self.adjvertex = adjvertex
        self.cost = cost
    }
    
    static func < (lhs: Edge, rhs: Edge) -> Bool {
           lhs.cost < rhs.cost
    }
}

let file = FileIO()
let N = file.readInt(), M = file.readInt(), X = file.readInt()

var graph = Array(repeating: Array(repeating: Edge(adjvertex: 0, cost: 0), count: 0), count: N + 1)
var anw = Array(repeating: 0, count: N + 1)
var minHeap = MinHeap<Edge>()

for _ in 0..<M {
    let start = file.readInt(), end = file.readInt(), cost = file.readInt()
    graph[start].append(Edge(adjvertex: end, cost: cost))
}

anw = dikstra(X)

for i in 1...N where i != X {
    anw[i] += dikstra(i)[X]
}

print(anw[1...].max()!)

func dikstra(_ start: Int) -> [Int] {
    // 초기화
    var distance = Array(repeating: Int.max, count: N + 1)
    
    // 시작점 start 관련 정보 설정 (시작점이기 때문에 가중치 0)
    distance[start] = 0

    // 시작점 우선순위 큐에 삽입
    minHeap.enqueue(Edge(adjvertex: start, cost: 0))

    while true {
        guard let cur = minHeap.dequeue() else {break}
        
        if distance[cur.adjvertex] < cur.cost {
            continue
        }
        
        // 현재 정점에 인접한 각 정점에 대해
        for edge in graph[cur.adjvertex] {
            // 해당 정점을 거쳐 갈 때 거리
            let newCost = edge.cost + cur.cost
            
            if newCost < distance[edge.adjvertex] {
                // 간선 완화
                distance[edge.adjvertex] = newCost
                // 다음 인접 거리를 계산하기 위에 enqueue
                minHeap.enqueue(Edge(adjvertex: edge.adjvertex, cost: newCost))
            }
        }
    }

    return distance
}
```

</div>
</details>

<br/>

### [플로이드 와샬](https://gyeom-ji.github.io/posts/floydWarshall-algorithm/)

---

#### [호석이 두 마리 치킨](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/21278.%E2%80%85%ED%98%B8%EC%84%9D%EC%9D%B4%E2%80%85%EB%91%90%E2%80%85%EB%A7%88%EB%A6%AC%E2%80%85%EC%B9%98%ED%82%A8)

컴공 출신은 치킨집을 하게 되어있다. 현실을 부정하지 말고 받아들이면 마음이 편하다. 결국 호석이도 2050년에는 치킨집을 하고 있다. 치킨집 이름은 "호석이 두마리 치킨"이다.<br/>

이번에 키친 도시로 분점을 확보하게 된 호석이 두마리 치킨은 도시 안에 2개의 매장을 지으려고 한다. 도시는 N 개의 건물과 M 개의 도로로 이루어져 있다. 건물은 1번부터 N번의 번호를 가지고 있다. i 번째 도로는 서로 다른 두 건물 Ai 번과 Bi 번 사이를 1 시간에 양방향으로 이동할 수 있는 도로이다.<br/>

키친 도시에서 2개의 건물을 골라서 치킨집을 열려고 한다. 이 때 아무 곳이나 열 순 없어서 모든 건물에서의 접근성의 합을 최소화하려고 한다. 건물 X 의 접근성은 X 에서 가장 가까운 호석이 두마리 치킨집까지 왕복하는 최단 시간이다. 즉, "모든 건물에서 가장 가까운 치킨집까지 왕복하는 최단 시간의 총합"을 최소화할 수 있는 건물 2개를 골라서 치킨집을 열려고 하는 것이다.<br/>

컴공을 졸업한 지 30년이 넘어가는 호석이는 이제 코딩으로 이 문제를 해결할 줄 모른다. 알고리즘 퇴물 호석이를 위해서 최적의 위치가 될 수 있는 건물 2개의 번호와 그 때의 "모든 건물에서 가장 가까운 치킨집까지 왕복하는 최단 시간의 총합"을 출력하자. 만약 이러한 건물 조합이 여러 개라면, 건물 번호 중 작은 게 더 작을수록, 작은 번호가 같다면 큰 번호가 더 작을수록 좋은 건물 조합이다.

``` Tip
💡 floydWarshall 알고리즘으로 모든 정점에서 모든 정점으로 가는 최단 경로를 구한다. 
 　 2개 건물 선택 후 모든 건물을 방문하며 최소 왕복거리를 구한다.
```
<br/>

> ex) <br/>
 ![21278](/assets/img/21278.avif){: width="400" height="400"}


``` ex
  [ distance ]

  [9223372036854775807, 0, 2, 1, 3, 3]
  [9223372036854775807, 2, 0, 1, 1, 1]
  [9223372036854775807, 1, 1, 0, 2, 2]
  [9223372036854775807, 3, 1, 2, 0, 2]
  [9223372036854775807, 3, 1, 2, 2, 0]
  
  [ i = 1, j = 2 일 때 ]

  k = 1 : sum += min(0, 2) * 2
  k = 2 : sum += min(2, 0) * 2
  k = 3 : sum += min(1, 1) * 2
  k = 4 : sum += min(3, 1) * 2
  k = 5 : sum += min(3, 1) * 2

  [ 0, 0, 2, 2, 2 ]
```

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
import Foundation

// 라이노님의 오픈소스  - FileIO
final class FileIO {
    private var buffer:[UInt8]
    private var index: Int
    
    init(fileHandle: FileHandle = FileHandle.standardInput) {
        buffer = Array(fileHandle.readDataToEndOfFile())+[UInt8(0)] // 인덱스 범위 넘어가는 것 방지
        index = 0
    }
    
    @inline(__always) private func read() -UInt8 {
        defer { index += 1 }
        
        return buffer.withUnsafeBufferPointer { $0[index] }
    }
    
    @inline(__always) func readInt() -Int {
        var sum = 0
        var now = read()
        var isPositive = true
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        if now == 45{ isPositive.toggle(); now = read() } // 음수 처리
        while now >= 48, now <= 57 {
            sum = sum * 10 + Int(now-48)
            now = read()
        }
        
        return sum * (isPositive ? 1:-1)
    }
    
    @inline(__always) func readString() -String {
        var str = ""
        var now = read()
        
        while now == 10
                || now == 32 { now = read() } // 공백과 줄바꿈 무시
        
        while now != 10
                && now != 32 && now != 0 {
            str += String(bytes: [now], encoding: .ascii)!
            now = read()
        }
        
        return str
    }
}

let file = FileIO()
let N = file.readInt(), M = file.readInt()
var distance = Array(repeating: Array(repeating: Int.max, count: N + 1), count: N + 1)
var anw = (N, N, Int.max)

for _ in 0..<M {
    let A = file.readInt(), B = file.readInt()
    distance[B][A] = 1
    distance[A][B] = 1
}

floydWarshall()

func floydWarshall() {
    // k = 거쳐가는 노드
    for k in 1...N {
        // i = 출발 노드
        for i in 1...N {
            // j = 도착 노드
            for j in 1...N {
                if i == j {
                    distance[i][j] = 0
                }
                if distance[k][j] == Int.max || distance[i][k] == Int.max { continue }
                distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])
            }
        }
    }
}

for i in 1..<N {
    for j in i + 1...N { // 건물 2개 선택
        var sum = 0
        for k in 1...N { // 모든 건물 방문
            // 건물 i ↔️ k, j ↔️ k 중 최단 왕복 거리를 더함
            sum += min(distance[i][k], distance[j][k]) * 2
        }
        if sum < anw.2 {
            anw = (i, j, sum)
        } else if sum == anw.2 && i <= anw.0 && j <= anw.1 {
            anw = (i, j, sum)
        }
    }
}

print(anw.0, anw.1, anw.2)
```

</div>
</details>


#### [경로 찾기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/11403.%E2%80%85%EA%B2%BD%EB%A1%9C%E2%80%85%EC%B0%BE%EA%B8%B0)

가중치 없는 방향 그래프 G가 주어졌을 때, 모든 정점 (i, j)에 대해서, i에서 j로 가는 길이가 양수인 경로가 있는지 없는지 구하는 프로그램을 작성하시오.


<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var map = Array(repeating: Array(repeating: 0, count: 0), count: N)

for i in 0..<N {
    let arr = readLine()!.split{$0 == " "}.map{Int($0)!}
    map[i] = arr
}

floydWarshall()

for i in 0..<N {
    for j in 0..<N {
        print(map[i][j], terminator: " ")
    }
    print()
}

func floydWarshall() {
    for k in 0..<N{
        for i in 0..<N {
            for j in 0..<N {
                if map[k][j] == 0 || map[i][k] == 0 { continue }
                map[i][j] = 1
            }
        }
    }
}
```
</div>
</details>

#### [친구](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1058.%E2%80%85%EC%B9%9C%EA%B5%AC)

지민이는 세계에서 가장 유명한 사람이 누구인지 궁금해졌다. 가장 유명한 사람을 구하는 방법은 각 사람의 `2-친구`를 구하면 된다. 어떤 사람 A가 또다른 사람 B의 2-친구가 되기 위해선, 두 사람이 친구이거나, A와 친구이고, B와 친구인 C가 존재해야 된다. 여기서 가장 유명한 사람은 2-친구의 수가 가장 많은 사람이다. 가장 유명한 사람의 2-친구의 수를 출력하는 프로그램을 작성하시오.<br/>
A와 B가 친구면, B와 A도 친구이고, A와 A는 친구가 아니다.

💡 floydWarshall 알고리즘으로 최단 경로를 구한 뒤 거리가 2 이하인 친구의 최대 개수를 구한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift

let N = Int(readLine()!)!
var distance = Array(repeating: Array(repeating: Int.max, count:N), count: N)

for i in 0..<N {
    let info = Array(readLine()!)
    for j in 0..<N {
        if info[j] == "Y" {
            distance[i][j] = 1
        }
    }
}

floydWarshall()

print(countTwoFriends())

func floydWarshall() {
    for k in 0..<N {
        for i in 0..<N {
            for j in 0..<N {
                if i == j { continue }
                if distance[k][j] == Int.max || distance[i][k] == Int.max { continue }
                distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])
            }
        }
    }
}


func countTwoFriends() -Int {
    var maxCnt = 0
    for i in 0..<N {
        maxCnt = max(maxCnt, distance[i].filter{$0 < 3}.count)
    }
    return maxCnt
}

```

</div>
</details>


<br/>

## [DP](https://gyeom-ji.github.io/posts/dynamic-programming/)

---

#### [다리 놓기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1010.%E2%80%85%EB%8B%A4%EB%A6%AC%E2%80%85%EB%86%93%EA%B8%B0)

재원이는 한 도시의 시장이 되었다. 이 도시에는 도시를 동쪽과 서쪽으로 나누는 큰 일직선 모양의 강이 흐르고 있다. 하지만 재원이는 다리가 없어서 시민들이 강을 건너는데 큰 불편을 겪고 있음을 알고 다리를 짓기로 결심하였다. 강 주변에서 다리를 짓기에 적합한 곳을 사이트라고 한다. 재원이는 강 주변을 면밀히 조사해 본 결과 강의 서쪽에는 N개의 사이트가 있고 동쪽에는 M개의 사이트가 있다는 것을 알았다. (N ≤ M)<br/>

재원이는 서쪽의 사이트와 동쪽의 사이트를 다리로 연결하려고 한다. (이때 한 사이트에는 최대 한 개의 다리만 연결될 수 있다.) 재원이는 다리를 최대한 많이 지으려고 하기 때문에 서쪽의 사이트 개수만큼 (N개) 다리를 지으려고 한다. 다리끼리는 서로 겹쳐질 수 없다고 할 때 다리를 지을 수 있는 경우의 수를 구하는 프로그램을 작성하라.<br/>

![1010](/assets/img/1010.jpeg)<br/>

``` Tip
💡 중복이 없고 순서와 상관 없이 선택 하는 경우의 수 = 조합 nCr
```

$${_nC_r} \ = \ {_{n-1}C_{r-1}} \ + \ {_{n-1}C_r}$$

$${_nC_0} = 1, \ {_nC_n} = 1$$

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
/// 중복이 없고 순서와 상관 없이 선택 하는 경우의 수 = 조합 nCr

let T = Int(readLine()!)!
/// dp[n][r] = nCr
var dp = Array(repeating: Array(repeating: 0, count: 31), count: 31)

for n in 0...30 {
    for r in 0...n {
        /// 초기값
        /// nC0 = 1, nCn = 1
        if n == r || r == 0 {
            dp[n][r] = 1
            continue
        }
        /// 점화식
        /// nCr = n-1Cr-1 + n-1Cr
        dp[n][r] = dp[n - 1][r - 1] + dp[n - 1][r]
    }
}

for _ in 0..<T {
    let info = readLine()!.split{$0 == " "}.map{Int($0)!}
    let n = info[1], r = info[0]
    print(dp[n][r])
}
```

</div>
</details>


#### [1로 만들기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1463.%E2%80%851%EB%A1%9C%E2%80%85%EB%A7%8C%EB%93%A4%EA%B8%B0)

정수 X에 사용할 수 있는 연산은 다음과 같이 세 가지 이다.<br/>

1. X가 3으로 나누어 떨어지면, 3으로 나눈다.<br/>
2. X가 2로 나누어 떨어지면, 2로 나눈다.<br/>
3. 1을 뺀다.<br/>

정수 N이 주어졌을 때, 위와 같은 연산 세 개를 적절히 사용해서 1을 만들려고 한다. 연산을 사용하는 횟수의 최솟값을 출력하시오.

``` Tip
💡 각 숫자별 최소 연산 횟수
 　 1 = 0, 2 = 1,  3 = 1
 　 4 (2 -> 1) = 2
 　 5 (4 -> 2 -> 1) = 3
 　 6 (3 -> 1) = 2
 　 7 (6 -> 3 -> 1) = 3
 　 8 (4 -> 2 -> 1) = 2
 　 4 부터는 연산(-1, /2, /3) 후 나온 숫자의 최솟값이 존재
 　 첫 연산 후 나온 숫자의 최솟값 + 1
 　 -1 연산과 나누기 연산 (2, 3으로 나눠진다면) 의 값 중 최솟값을 dp[i]에 저장
```

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var dp = Array(repeating: 0, count: N + 1)

if N == 1 {
    print(0)
} else {
    for i in 2...N {
        dp[i] = dp[i - 1] + 1
        
        if i % 2 == 0 {
            dp[i] = min(dp[i/2] + 1, dp[i])
        }
        if i % 3 == 0 {
            dp[i] = min(dp[i/3] + 1, dp[i])
        }
    }
    
    print(dp[N])
}
```

</div>
</details>


#### [안녕](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1535.%E2%80%85%EC%95%88%EB%85%95)

세준이는 성형수술을 한 후에 병원에 너무 오래 입원해 있었다. 이제 세준이가 병원에 입원한 동안 자기를 생각해준 사람들에게 감사하다고 말할 차례이다.<br/>

세준이를 생각해준 사람은 총 N명이 있다. 사람의 번호는 1번부터 N번까지 있다. 세준이가 i번 사람에게 인사를 하면 L[i]만큼의 체력을 잃고, J[i]만큼의 기쁨을 얻는다. 세준이는 각각의 사람에게 최대 1번만 말할 수 있다.<br/>

세준이의 목표는 주어진 체력내에서 최대한의 기쁨을 느끼는 것이다. 세준이의 체력은 100이고, 기쁨은 0이다. 만약 세준이의 체력이 0이나 음수가 되면, 죽어서 아무런 기쁨을 못 느낀 것이 된다. 세준이가 얻을 수 있는 최대 기쁨을 출력하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var powerArr = readLine()!.split{$0 == " "}.map{Int($0)!}
var joyArr = readLine()!.split{$0 == " "}.map{Int($0)!}
// dp[i][j] = 체력이 j일때 i번째 사람까지 최대 기쁨
var dp = Array(repeating: Array(repeating:0,count:100), count: N + 1)

for i in 1...N {
    for j in 1...99 {
        if j < powerArr[i - 1] {
            dp[i][j] = dp[i - 1][j]
        } else {
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - powerArr[i - 1]] + joyArr[i - 1])
        }
    }
}

print(dp[N][99])
```

</div>
</details>


#### [거스름돈](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/14916.%E2%80%85%EA%B1%B0%EC%8A%A4%EB%A6%84%EB%8F%88)

춘향이는 편의점 카운터에서 일한다.<br/>

손님이 2원짜리와 5원짜리로만 거스름돈을 달라고 한다. 2원짜리 동전과 5원짜리 동전은 무한정 많이 가지고 있다. 동전의 개수가 최소가 되도록 거슬러 주어야 한다. 거스름돈이 n인 경우, 최소 동전의 개수가 몇 개인지 알려주는 프로그램을 작성하시오.<br/>

예를 들어, 거스름돈이 15원이면 5원짜리 3개를, 거스름돈이 14원이면 5원짜리 2개와 2원짜리 2개로 총 4개를, 거스름돈이 13원이면 5원짜리 1개와 2원짜리 4개로 총 5개를 주어야 동전의 개수가 최소가 된다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var dp = Array(repeating: 0, count: N + 1)

if N == 1 {
    print(-1)
} else {
    for i in 2...N {
        if i >= 5 && (i % 5 == 0 || (i - 5) % 2 == 0) {
            dp[i] = dp[i - 5] + 1
        } else if i % 2 == 0 {
            dp[i] = dp[i - 2] + 1
        } else {
            dp[i] = -1
        }
    }
    print(dp[N])
}
```

</div>
</details>


#### [투자의 귀재 배주형](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/19947.%E2%80%85%ED%88%AC%EC%9E%90%EC%9D%98%E2%80%85%EA%B7%80%EC%9E%AC%E2%80%85%EB%B0%B0%EC%A3%BC%ED%98%95)

2020년에 학교로 복학한 주형이는 월세를 마련하기 위해서 군 적금을 깨고 복리 투자를 하려고 한다.<br/>

주형이가 하려는 투자에는 3가지 방법의 투자 방식이 있다.<br/>

- 1년마다 5%의 이율을 얻는 투자 (A)<br/>
- 3년마다 20%의 이율을 얻는 투자 (B)<br/>
- 5년마다 35%의 이율을 얻는 투자 (C)<br/>
투자를 할 때에는 다음과 같은 주의점이 있다.<br/>
- 투자의 기한(1년, 3년, 5년)을 채우는 시점에 이율이 반영되며, 그 사이에는 돈이 늘어나지 않는다.<br/>
- 투자 방식은 매년 바꿀 수 있다.<br/>
- 매번 이율은 소수점 이하를 버림 해서 받는다.<br/>

예를 들어서, 지금 가진 돈이 11111원이면, A 방식이면 1년 후에 555원, B 방식이면 3년 후에 2,222원, C 방식이면 5년 후에 3,888원을 이자로 받을 수 있다. 만약 C 방식으로 투자했지만 4년이 지난 시점이라면 받을 수 있는 이자는 0원이다.<br/>

주형이의 초기 비용이 H원일 때, Y년이 지난 시점에 가장 많은 금액을 얻을 수 있는 투자 패턴을 분석하고 그 금액을 출력하자.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var dp = Array(repeating: 0, count: input[1] + 1)
dp[0] = input[0]

for i in 1...input[1] {

    dp[i] = Int(Double(dp[i - 1]) * 1.05)

    if i >= 3 {
        dp[i] = max(dp[i], Int(Double(dp[i - 3]) * 1.2))
    }
    if i >= 5 {
        dp[i] = max(dp[i], Int(Double(dp[i - 5]) * 1.35))
    }
}

print(dp[input[1]])
```

</div>
</details>

#### [수열](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/2491.%E2%80%85%EC%88%98%EC%97%B4)

0에서부터 9까지의 숫자로 이루어진 N개의 숫자가 나열된 수열이 있다. 그 수열 안에서 연속해서 커지거나(같은 것 포함), 혹은 연속해서 작아지는(같은 것 포함) 수열 중 가장 길이가 긴 것을 찾아내어 그 길이를 출력하는 프로그램을 작성하라.<br/>

예를 들어 수열 1, 2, 2, 4, 4, 5, 7, 7, 2 의 경우에는 1 ≤ 2 ≤ 2 ≤ 4 ≤ 4 ≤ 5 ≤ 7 ≤ 7 이 가장 긴 구간이 되므로 그 길이 8을 출력한다.<br/>

수열 4, 1, 3, 3, 2, 2, 9, 2, 3 의 경우에는 3 ≥ 3 ≥ 2 ≥ 2 가 가장 긴 구간이 되므로 그 길이 4를 출력한다.<br/>

또 1, 5, 3, 6, 4, 7, 1, 3, 2, 9, 5 의 경우에는 연속해서 커지거나 작아지는 수열의 길이가 3 이상인 경우가 없으므로 2를 출력하여야 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = Int(readLine()!)!
var arr = readLine()!.split{$0 == " "}.map{Int($0)!}
var smaller = Array(repeating: 0, count: input)
var bigger = Array(repeating: 0, count: input)

for i in 1..<input {
    smaller[i] = smaller[i - 1] + 1
    bigger[i] = bigger[i - 1] + 1
    
    if arr[i - 1] < arr[i] {
        bigger[i] = 0
    } else if arr[i] < arr[i - 1]{
        smaller[i] = 0
    }
}

var smallMax = smaller.max()!
var bigMax = bigger.max()!

print(max(smallMax, bigMax) + 1)

```

</div>
</details>


#### [피보나치 함수](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1003.%E2%80%85%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98%E2%80%85%ED%95%A8%EC%88%98)

다음 소스는 N번째 피보나치 수를 구하는 C++ 함수이다.<br/>

``` C++

int fibonacci(int n) {
   if (n == 0) {
       printf("0");
       return 0;
   } else if (n == 1) {
       printf("1");
       return 1;
   } else {
       return fibonacci(n‐1) + fibonacci(n‐2);
   }
}

```

fibonacci(3)을 호출하면 다음과 같은 일이 일어난다.<br/>

- fibonacci(3)은 fibonacci(2)와 fibonacci(1) (첫 번째 호출)을 호출한다.<br/>
- fibonacci(2)는 fibonacci(1) (두 번째 호출)과 fibonacci(0)을 호출한다.<br/>
- 두 번째 호출한 fibonacci(1)은 1을 출력하고 1을 리턴한다.<br/>
- fibonacci(0)은 0을 출력하고, 0을 리턴한다.<br/>
- fibonacci(2)는 fibonacci(1)과 fibonacci(0)의 결과를 얻고, 1을 리턴한다.<br/>
- 첫 번째 호출한 fibonacci(1)은 1을 출력하고, 1을 리턴한다.<br/>
- fibonacci(3)은 fibonacci(2)와 fibonacci(1)의 결과를 얻고, 2를 리턴한다.<br/>

1은 2번 출력되고, 0은 1번 출력된다. N이 주어졌을 때, fibonacci(N)을 호출했을 때, 0과 1이 각각 몇 번 출력되는지 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let T = Int(readLine()!)!
var anw = ""

for _ in 0..<T {
    let N = Int(readLine()!)!
    var zero = Array(repeating: 0, count: N + 1)
    var one = Array(repeating: 0, count: N + 1)
    
    zero[0] = 1
    
    if N >= 1 {
        one[1] = 1
    }
    if N >= 2 {
        for i in 2...N{
            zero[i] = zero[i - 1] + zero[i - 2]
            one[i] = one[i - 1] + one[i - 2]
        }
    }

    anw += "\(zero[N]) \(one[N])\n"
}
print(anw)
```

</div>
</details>

#### [1, 2, 3 더하기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/9095.%E2%80%851%EF%BC%8C%E2%80%852%EF%BC%8C%E2%80%853%E2%80%85%EB%8D%94%ED%95%98%EA%B8%B0)

정수 4를 1, 2, 3의 합으로 나타내는 방법은 총 7가지가 있다. 합을 나타낼 때는 수를 1개 이상 사용해야 한다.<br/>

- 1+1+1+1<br/>
- 1+1+2<br/>
- 1+2+1<br/>
- 2+1+1<br/>
- 2+2<br/>
- 1+3<br/>
- 3+1<br/>

정수 n이 주어졌을 때, n을 1, 2, 3의 합으로 나타내는 방법의 수를 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let T = Int(readLine()!)!
var anw = ""

for _ in 0..<T {
    let N = Int(readLine()!)!
    let count = N < 3 ? 4 : N + 1
    var dp = Array(repeating: 0, count:count)
    
    dp[1] = 1
    dp[2] = 2
    dp[3] = 4
    
    if N > 3 {
        for i in 4...N {
            dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3]
        }
    }
    
    anw += "\(dp[N])\n"
}

print(anw)
```

</div>
</details>

#### [파도반 수열](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/9461.%E2%80%85%ED%8C%8C%EB%8F%84%EB%B0%98%E2%80%85%EC%88%98%EC%97%B4)

![pandovan](/assets/img/pandovan.png)<br/>

그림과 같이 삼각형이 나선 모양으로 놓여져 있다. 첫 삼각형은 정삼각형으로 변의 길이는 1이다. 그 다음에는 다음과 같은 과정으로 정삼각형을 계속 추가한다. 나선에서 가장 긴 변의 길이를 k라 했을 때, 그 변에 길이가 k인 정삼각형을 추가한다.<br/>

파도반 수열 P(N)은 나선에 있는 정삼각형의 변의 길이이다.<br/>

P(1)부터 P(10)까지 첫 10개 숫자는 1, 1, 1, 2, 2, 3, 4, 5, 7, 9이다.<br/>

N이 주어졌을 때, P(N)을 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let T = Int(readLine()!)!
var anw = ""

for _ in 0..<T {
    let N = Int(readLine()!)!
    let count = N < 3 ? 4 : N + 1
    var dp = Array(repeating: 0, count:count)
    
    dp[1] = 1
    dp[2] = 1
    dp[3] = 1
    
    if N > 3 {
        for i in 4...N {
            dp[i] = dp[i - 2] + dp[i - 3]
        }
    }
    
    anw += "\(dp[N])\n"
}

print(anw)
```

</div>
</details>

<br/>
 
## [스택](https://gyeom-ji.github.io/posts/stackAndQueue/)

---

#### [에디터](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1406.%E2%80%85%EC%97%90%EB%94%94%ED%84%B0)

한 줄로 된 간단한 에디터를 구현하려고 한다. 이 편집기는 영어 소문자만을 기록할 수 있는 편집기로, 최대 600,000글자까지 입력할 수 있다.<br/>

이 편집기에는 '커서'라는 것이 있는데, 커서는 문장의 맨 앞(첫 번째 문자의 왼쪽), 문장의 맨 뒤(마지막 문자의 오른쪽), 또는 문장 중간 임의의 곳(모든 연속된 두 문자 사이)에 위치할 수 있다. 즉 길이가 L인 문자열이 현재 편집기에 입력되어 있으면, 커서가 위치할 수 있는 곳은 L+1가지 경우가 있다.<br/>

이 편집기가 지원하는 명령어는 다음과 같다.<br/>

![1406](/assets/img/1406.png)<br/>

초기에 편집기에 입력되어 있는 문자열이 주어지고, 그 이후 입력한 명령어가 차례로 주어졌을 때, 모든 명령어를 수행하고 난 후 편집기에 입력되어 있는 문자열을 구하는 프로그램을 작성하시오. 단, 명령어가 수행되기 전에 커서는 문장의 맨 뒤에 위치하고 있다고 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
var left = readLine()!
let MAX = Int(readLine()!)!
var right = ""

for _ in 0..<MAX {
    let command = readLine()!
    executeCommand(command)
}

func executeCommand(_ command : String){
    switch(command) {
    case "L" :
        if let last = left.popLast() {
            right.append(last)
        }
        
    case "D" :
        if let last = right.popLast() {
            left.append(last)
        }
        
    case "B" :
        left.popLast()
        
    default :
        left.append(command.last!)
    }
}

print(left + right.reversed())

```

</div>
</details>


<br/>

## 백트래킹

---

- 모든 가능한 경우의 수 중 <span style="color:#9fb584">**특정한 조건을 만족하는 경우만 탐색**</span>한다.
  - 완전 탐색 기법인 DFS(깊이우선탐색), BFS(너비우선탐색)로 모두 구현이 가능하다.
  - 조건에 부합하지 않으면 이전 수행으로 돌아가야 하는 특성 때문에 BFS보다 DFS를 주로 사용한다.
- 답이 될 수 있는지 판단하고 그렇지 않으면 그 부분을 탐색하지 않고 가지치기한다.
    가지치기 : 더 이상 탐색할 필요가 없는 상태를 제외하는 것

<br/>

#### [N과 M (1)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15649.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%881%EF%BC%89)

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var anw = ""

backTracking(1, [])
print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    for i in 1...input[0] {
        if !arr.contains(i) {
            backTracking(i + 1, arr + [i])
        }
    }
}
```

</div>
</details>

#### [N과 M (2)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15650.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%882%EF%BC%89)

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
- 고른 수열은 오름차순이어야 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var anw = ""

backTracking(1, [])
print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    if cur > input[0] {return}
    for i in cur...input[0] {
        if !arr.contains(i) {
            backTracking(i + 1, arr + [i])
        }
    }
}
```

</div>
</details>

#### [N과 M (3)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15651.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%883%EF%BC%89)

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var anw = ""

backTracking(1, [])

print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    for i in 1...input[0] {
        backTracking(i + 1, arr + [i])
    }
}
```

</div>
</details>

#### [N과 M (4)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15652.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%884%EF%BC%89)

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.
- 고른 수열은 비내림차순이어야 한다.
- 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var anw = ""

backTracking(1, [])

print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    if cur > input[0] {return}
    
    for i in cur...input[0] {
        backTracking(i, arr + [i])
    }
}
```

</div>
</details>

#### [N과 M (5)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15654.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%885%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = ""

backTracking(0, [])

print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    
    for i in 0..<input[0] {
        if !arr.contains(inputArray[i]){
            backTracking(i + 1, arr + [inputArray[i]])
        }
    }
}
```

</div>
</details>

#### [N과 M (6)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15655.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%886%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열
- 고른 수열은 오름차순이어야 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = ""

backTracking(0, [])

print(anw)

func backTracking(_ cur: Int, _ arr: [Int]) {
    if arr.count == input[1] {
        anw += "\(arr.map{ String($0) }.joined(separator: " "))\n"
        return
    }
    
    if cur > input[0] - 1 {return}
    
    for i in cur..<input[0] {
        if !arr.contains(inputArray[i]){
            backTracking(i + 1, arr + [inputArray[i]])
        }
    }
}
```

</div>
</details>

#### [N과 M (7)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15656.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%887%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = ""

backTracking(0, "", 0)

print(anw)

func backTracking(_ cur: Int, _ str: String, _ cnt: Int) {
    if cnt == input[1] {
        anw += "\(str)\n"
        return
    }
    
    for i in 0..<input[0] {
        backTracking(i, str + "\(inputArray[i]) ", cnt + 1)
    }
}
```

</div>
</details>

#### [N과 M (8)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15657.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%888%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.
- 고른 수열은 비내림차순이어야 한다.
- 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = ""

backTracking(0, "", 0)

print(anw)

func backTracking(_ cur: Int, _ str: String, _ cnt: Int) {
    if cnt == input[1] {
        anw += "\(str)\n"
        return
    }
    
    if cur > input[0] - 1 {return}
    
    for i in cur..<input[0] {
        backTracking(i, str + "\(inputArray[i]) ", cnt + 1)
    }
}
```

</div>
</details>

#### [N과 M (9)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15663.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%889%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- N개의 자연수 중에서 M개를 고른 수열

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = Set<String>()
var visited = Array(repeating: false, count: input[0] + 1)

backTracking(0, [])

func backTracking(_ cur: Int, _ arr: [String]) {
    if arr.count == input[1] {
        let checkStr = arr.joined(separator: " ")
        if !anw.contains(checkStr) {
            anw.insert(checkStr)
            print(checkStr)
        }
        return
    }
    
    for i in 0..<input[0] where !visited[i]{
        visited[i] = true
        backTracking(i + 1, arr + ["\(inputArray[i])"])
        visited[i] = false
    }
}
```

</div>
</details>

#### [N과 M (10)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15664.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%8810%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- N개의 자연수 중에서 M개를 고른 수열
- 고른 수열은 비내림차순이어야 한다.
- 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = Set<String>()
var visited = Array(repeating: false, count: input[0] + 1)

backTracking(0, [])

func backTracking(_ cur: Int, _ arr: [String]) {
    if arr.count == input[1] {
        let checkStr = arr.joined(separator: " ")
        if !anw.contains(checkStr) {
            anw.insert(checkStr)
            print(checkStr)
        }
        return
    }
    if cur > input[0] - 1 {
        return
    }
    
    for i in cur..<input[0] where !visited[i]{
        visited[i] = true
        backTracking(i + 1, arr + ["\(inputArray[i])"])
        visited[i] = false
    }
}
```

</div>
</details>

#### [N과 M (11)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15665.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%8811%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- N개의 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = Set<String>()

backTracking(0, [])

func backTracking(_ cur: Int, _ arr: [String]) {
    if arr.count == input[1] {
        let checkStr = arr.joined(separator: " ")
        if !anw.contains(checkStr) {
            anw.insert(checkStr)
            print(checkStr)
        }
        return
    }
    
    for i in 0..<input[0] {
        backTracking(i, arr + ["\(inputArray[i])"])
    }
}
```

</div>
</details>

#### [N과 M (12)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15666.%E2%80%85N%EA%B3%BC%E2%80%85M%E2%80%85%EF%BC%8812%EF%BC%89)

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- N개의 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.
- 고른 수열은 비내림차순이어야 한다.
- 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var inputArray = readLine()!.split{$0 == " "}.map{Int($0)!}.sorted(by: <)
var anw = Set<String>()

backTracking(0, [])

func backTracking(_ cur: Int, _ arr: [String]) {
    if arr.count == input[1] {
        let checkStr = arr.joined(separator: " ")
        if !anw.contains(checkStr) {
            anw.insert(checkStr)
            print(checkStr)
        }
        return
    }
    
    if cur > input[0] - 1 {
        return
    }
    
    for i in cur..<input[0] {
        backTracking(i, arr + ["\(inputArray[i])"])
    }
}
```

</div>
</details>

#### [미술가 미미](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/20950.%E2%80%85%EB%AF%B8%EC%88%A0%EA%B0%80%E2%80%85%EB%AF%B8%EB%AF%B8)

미미는 미적 감각이 뛰어난 미술가이다. 미미는 때때로 여러 물감을 섞어 새로운 색의 물감을 만들고는 한다. 어느 날 그림을 그리던 미미는 놀라 자빠질 수밖에 없었다. 미미가 가장 아끼는 곰두리색 물감이 다 떨어졌기 때문이다. 하지만 미미는 새 물감을 살 돈이 없다. 물감은 역시 섞어 써야 제맛이다. 미미는 남은 물감들을 섞어 곰두리색 물감을 만들기로 결심하였다.<br/>
먼저 RGB 표기법에 대하여 알아보자. RGB 표기법은 빨간색(Red), 초록색(Green), 파란색(Blue)을 혼합하여 색을 나타내는 방법으로, 각각의 색은 밝기에 따라 0부터 255까지의 정수로 표현한다. 예를 들어, 분홍색은 rgb(255, 192, 203)과 같이 표현한다. 이는 빨간색을 255만큼, 초록색을 192만큼, 파란색을 203만큼 혼합하였다는 의미이다.<br/>
새로운 물감을 만들기 위해서는 남아 있는 물감 중 혼합할 물감들을 선택한 후 이들을 동일한 비율로 섞는다. P1, P2, ..., PK번 물감을 섞어 새로 만들어지는 색은 RGB 표기법으로 다음과 같다.<br/>
![20950](/assets/img/20950.png)<br/>
즉, 새로운 R 값은 혼합할 모든 물감의 R 값을 더한 후 이를 물감의 개수로 나누어 구한다. 이때 소수점은 버린다. G와 B 값도 동일한 방법으로 구한다.<br/>
색 i와 색 j의 차이는 다음과 같다.<br/>
![20950](/assets/img/20950(1).png)<br/>
물감들을 섞어서 만들 수 있는 색 중 곰두리색에 가장 가까운, 즉 곰두리색과의 차이가 가장 작은 색을 문두리색이라고 한다. N개의 물감과 곰두리색이 주어졌을 때, 곰두리색과 문두리색의 차이를 구하는 프로그램을 작성하시오. 단, 미미는 아직 실력이 부족하여 최대 7개의 색만을 혼합할 수 있다. 또한 물감을 섞지 않고 단독으로 사용할 수 없다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
let maxCnt = N > 7 ? 7 : N
var rgbArray = [[Int]]()

for _ in 0..<N {
    rgbArray.append(readLine()!.split{$0 == " "}.map{Int($0)!})
}

let gomdury = readLine()!.split{$0 == " "}.map{Int($0)!}
var mundury = [0, 0, 0]
var anw = Int.max

// 물감은 최소 2개 색 혼합
for i in 2...N {
    // 최대 7개 색 혼합 가능
    if i > 7 {
        break
    }
    backTracking(0, i, 0)
}

print(anw)

func backTracking(_ cur: Int,_ maxCnt: Int,_ cnt: Int) {
    // 현재 물감 갯수 == 최대 물감 갯수
    if cnt == maxCnt {
        // 혼합된 물감의 r, g, b 값을 물감의 개수로 나눠 새로운 r, g, b 값 구함
        // 새로운 r, g, b 값과 곰두리 값의 차이를 구함
        let r = abs((mundury[0] / maxCnt) - gomdury[0])
        let g = abs((mundury[1] / maxCnt) - gomdury[1])
        let b = abs((mundury[2] / maxCnt) - gomdury[2])
        
        // 최솟값으로 anw를 갱신
        anw = min(r + g + b, anw)
        return
    }
    
    for i in cur..<N {
        // 문두리색에 i번째 물감 rgb값 +
        mundury[0] += rgbArray[i][0]
        mundury[1] += rgbArray[i][1]
        mundury[2] += rgbArray[i][2]
        
        backTracking(i + 1, maxCnt, cnt + 1)
        
        // 문두리색에 i번째 물감 rgb값 -
        mundury[0] -= rgbArray[i][0]
        mundury[1] -= rgbArray[i][1]
        mundury[2] -= rgbArray[i][2]
    }
}
```

</div>
</details>

#### [영재의 시험](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/19949.%E2%80%85%EC%98%81%EC%9E%AC%EC%9D%98%E2%80%85%EC%8B%9C%ED%97%98)

컴퓨터공학과 학생인 영재는 이번 학기에 알고리즘 수업을 수강한다.<br/>

평소에 자신의 실력을 맹신한 영재는 시험 전날까지 공부를 하지 않았다.<br/>

당연하게도 문제를 하나도 풀지 못하였지만 다행히도 문제가 5지 선다의 객관식 10문제였다.<br/>

찍기에도 자신 있던 영재는 3개의 연속된 문제의 답은 같지 않게 한다는 자신의 비법을 이용하여 모든 문제를 찍었다.<br/>

이때 영재의 점수가 5점 이상일 경우의 수를 구하여라.<br/>

문제의 점수는 1문제당 1점씩이다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let answerArr = readLine()!.split{$0 == " "}.map{Int($0)!}
var solution = Array(repeating: 0, count: 10)
var anw = 0

backTracking(0, 0)

print(anw)

func backTracking(_ cur: Int, _ correct: Int) {
    var invalid = 0
    
    // 남은 문제를 다 풀어도 정답 갯수가 5보다 적은 경우
    if correct + (10 - cur) < 5 {
        return
    }
    
    if cur == 10 {
        if correct >= 5 {
            anw += 1
        }
        return
    }
    
    // 2문제 연속 답
    if cur >= 2 && solution[cur - 1] == solution[cur - 2] {
        invalid = solution[cur - 1]
    }
    
    for i in 1...5 {
        // 3문제 연속 답이 아닐 경우
        if invalid != i {
            solution[cur] = i
            
            // 정답일 경우 correct + 1
            if answerArr[cur] == i {
                backTracking(cur + 1, correct + 1)
            } else {
                backTracking(cur + 1, correct)
            }
            // 초기화
            solution[cur] = 0
        }
    }
}
```

</div>
</details>

#### [연산자 끼워넣기](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/14888.%E2%80%85%EC%97%B0%EC%82%B0%EC%9E%90%E2%80%85%EB%81%BC%EC%9B%8C%EB%84%A3%EA%B8%B0)

N개의 수로 이루어진 수열 A1, A2, ..., AN이 주어진다. 또, 수와 수 사이에 끼워넣을 수 있는 N-1개의 연산자가 주어진다. 연산자는 덧셈(+), 뺄셈(-), 곱셈(×), 나눗셈(÷)으로만 이루어져 있다.<br/>

우리는 수와 수 사이에 연산자를 하나씩 넣어서, 수식을 하나 만들 수 있다. 이때, 주어진 수의 순서를 바꾸면 안 된다.<br/>

예를 들어, 6개의 수로 이루어진 수열이 1, 2, 3, 4, 5, 6이고, 주어진 연산자가 덧셈(+) 2개, 뺄셈(-) 1개, 곱셈(×) 1개, 나눗셈(÷) 1개인 경우에는 총 60가지의 식을 만들 수 있다. 예를 들어, 아래와 같은 식을 만들 수 있다.<br/>

- 1+2+3-4×5÷6<br/>
- 1÷2+3+4-5×6<br/>
- 1+2÷3×4-5+6<br/>
- 1÷2×3-4+5+6<br/>

식의 계산은 연산자 우선 순위를 무시하고 앞에서부터 진행해야 한다. 또, 나눗셈은 정수 나눗셈으로 몫만 취한다. 음수를 양수로 나눌 때는 C++14의 기준을 따른다. 즉, 양수로 바꾼 뒤 몫을 취하고, 그 몫을 음수로 바꾼 것과 같다. 이에 따라서, 위의 식 4개의 결과를 계산해보면 아래와 같다.<br/>

- 1+2+3-4×5÷6 = 1<br/>
- 1÷2+3+4-5×6 = 12<br/>
- 1+2÷3×4-5+6 = 5<br/>
- 1÷2×3-4+5+6 = 7<br/>

N개의 수와 N-1개의 연산자가 주어졌을 때, 만들 수 있는 식의 결과가 최대인 것과 최소인 것을 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var numArr = readLine()!.split{$0 == " "}.map{Int($0)!}
var operation = readLine()!.split{$0 == " "}.map{Int($0)!}
var minValue = Int.max, maxValue = Int.min
var cur = numArr[0]

backTracking(1)

print(maxValue)
print(minValue)

func backTracking(_ index: Int) {
    if index == N {
        minValue = min(minValue, cur)
        maxValue = max(maxValue, cur)
        return
    }
    
    for i in 0..<4 {
        if operation[i] >= 1 {
            operation[i] -= 1
            let beforeCur = cur
            
            switch(i) {
            case 0:
                cur += numArr[index]
            case 1:
                cur -= numArr[index]
            case 2:
                cur *= numArr[index]
            case 3:
                if numArr[index] < 0 {
                    cur /= abs(numArr[index])
                    cur *= -1
                } else {
                    cur /= numArr[index]
                }
            default:
                break
            }
            backTracking(index + 1)
            cur = beforeCur
            operation[i] += 1
        }
    }
}
```

</div>
</details>

#### [연산자 끼워넣기 (2)](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/15658.%E2%80%85%EC%97%B0%EC%82%B0%EC%9E%90%E2%80%85%EB%81%BC%EC%9B%8C%EB%84%A3%EA%B8%B0%E2%80%85%EF%BC%882%EF%BC%89)

N개의 수로 이루어진 수열 A1, A2, ..., AN이 주어진다. 또, 수와 수 사이에 끼워넣을 수 있는 연산자가 주어진다. 연산자는 덧셈(+), 뺄셈(-), 곱셈(×), 나눗셈(÷)으로만 이루어져 있다. 연산자의 개수는 N-1보다 많을 수도 있다. 모든 수의 사이에는 연산자를 한 개 끼워넣어야 하며, 주어진 연산자를 모두 사용하지 않고 모든 수의 사이에 연산자를 끼워넣을 수도 있다.<br/>

우리는 수와 수 사이에 연산자를 하나씩 넣어서, 수식을 하나 만들 수 있다. 이때, 주어진 수의 순서를 바꾸면 안 된다.<br/>

예를 들어, 6개의 수로 이루어진 수열이 1, 2, 3, 4, 5, 6이고, 주어진 연산자가 덧셈(+) 3개, 뺄셈(-) 2개, 곱셈(×) 1개, 나눗셈(÷) 1개인 경우에는 총 250가지의 식을 만들 수 있다. 예를 들어, 아래와 같은 식을 만들 수 있다.<br/>

- 1+2+3-4×5÷6<br/>
- 1÷2+3+4-5×6<br/>
- 1+2÷3×4-5+6<br/>
- 1÷2×3-4+5+6<br/>
- 1+2+3+4-5-6<br/>
- 1+2+3-4-5×6<br/>

식의 계산은 연산자 우선 순위를 무시하고 앞에서부터 진행해야 한다. 또, 나눗셈은 정수 나눗셈으로 몫만 취한다. 음수를 양수로 나눌 때는 C++14의 기준을 따른다. 즉, 양수로 바꾼 뒤 몫을 취하고, 그 몫을 음수로 바꾼 것과 같다. 이에 따라서, 위의 식 4개의 결과를 계산해보면 아래와 같다.<br/>

- 1+2+3-4×5÷6 = 1<br/>
- 1÷2+3+4-5×6 = 12<br/>
- 1+2÷3×4-5+6 = 5<br/>
- 1÷2×3-4+5+6 = 7<br/>
- 1+2+3+4-5-6 = -1<br/>
- 1+2+3-4-5×6 = -18<br/>

N개의 수와 연산자가 주어졌을 때, 만들 수 있는 식의 결과가 최대인 것과 최소인 것을 구하는 프로그램을 작성하시오.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var numArr = readLine()!.split{$0 == " "}.map{Int($0)!}
// 0 : +, 1 : -, 2 : *, 3 : /
var operation = readLine()!.split{$0 == " "}.map{Int($0)!}
var operationCnt = 4
var maxValue = Int.min, minValue = Int.max
var cur = numArr[0]

backTracking(1)

print(maxValue)
print(minValue)

func backTracking(_ index: Int) {
    if index == N {
        maxValue = max(maxValue, cur)
        minValue = min(minValue, cur)
        return
    }
    
    for i in 0..<operationCnt {
        if operation[i] >= 1 {
            
            operation[i] -= 1
            let beforeCur = cur
            
            switch (i) {
            case 0:
                cur += numArr[index]
            case 1:
                cur -= numArr[index]
            case 2:
                cur *= numArr[index]
            case 3:
                if numArr[index] < 0 {
                    cur /= abs(numArr[index])
                    cur *= -1
                } else {
                    cur /= numArr[index]
                }
            default:
                break
            }
            
            backTracking(index + 1)
            operation[i] += 1
            cur = beforeCur
        } else {
            continue
        }
    }
}

```

</div>
</details>

#### [컴백홈](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Silver/1189.%E2%80%85%EC%BB%B4%EB%B0%B1%ED%99%88)

한수는 캠프를 마치고 집에 돌아가려 한다. 한수는 현재 왼쪽 아래점에 있고 집은 오른쪽 위에 있다. 그리고 한수는 집에 돌아가는 방법이 다양하다. 단, 한수는 똑똑하여 한번 지나친 곳을 다시 방문하지는 않는다.<br/>

```
     cdef  ...f  ..ef  ..gh  cdeh  cdej  ...f 
     bT..  .T.e  .Td.  .Tfe  bTfg  bTfi  .Tde 
     a...  abcd  abc.  abcd  a...  a.gh  abc. 
거리 :  6     6     6     8     8    10    6
```

위 예제는 한수가 집에 돌아갈 수 있는 모든 경우를 나타낸 것이다. T로 표시된 부분은 가지 못하는 부분이다. 문제는 R x C 맵에 못가는 부분이 주어지고 거리 K가 주어지면 한수가 집까지도 도착하는 경우 중 거리가 K인 가짓수를 구하는 것이다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
// 0 : R, 1 : C, 2 : K
let input = readLine()!.split{$0 == " "}.map{Int($0)!}
var map = Array(repeating: Array(repeating: Character(" "), count:0), count: input[0])
var cur = (input[0] - 1, 0)
let target = (0, input[1] - 1)
let dx = [1, -1, 0, 0], dy = [0, 0, 1, -1]
var visited = Array(repeating: Array(repeating: false, count: input[1]), count: input[0])
var anw = 0

// 출발점 방문 설정
visited[cur.0][cur.1] = true

for i in 0..<input[0] {
    map[i] = Array(readLine()!)
}

backTracking(1)
print(anw)

func isValid(_ x: Int, _ y: Int) -> Bool {
    return x >= 0 && x < input[0] && y >= 0 && y < input[1] && map[x][y] != "T"
}

func backTracking(_ distance: Int) {
    // 거리가 K인 경우 (K안에 타겟에 도착하지 못할 경우 무시)
    if distance == input[2] {
        // 현 위치가 타겟인 경우 anw +
        if cur == target {
            anw += 1
        }
        return
    }
    
    for i in 0..<4 {
        let curX = cur.0, curY = cur.1
        let newX = cur.0 + dx[i], newY = cur.1 + dy[i]
        
        // newX, newY 방문 가능 여부 체크, 방문 이력 체크
        if isValid(newX, newY) && !visited[newX][newY] {
            // 방문 설정, cur 변경
            visited[newX][newY] = true
            cur = (newX, newY)
            
            backTracking(distance + 1)
            
            // 초기화
            cur = (curX, curY)
            visited[newX][newY] = false
        }
    }
}

```

</div>
</details>

#### [줄어드는 수](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/1174.%E2%80%85%EC%A4%84%EC%96%B4%EB%93%9C%EB%8A%94%E2%80%85%EC%88%98)

음이 아닌 정수를 십진법으로 표기했을 때, 왼쪽에서부터 자리수가 감소할 때, 그 수를 줄어드는 수라고 한다. 예를 들어, 321와 950은 줄어드는 수이고, 322와 958은 아니다.<br/>

N번째로 작은 줄어드는 수를 출력하는 프로그램을 작성하시오. 만약 그러한 수가 없을 때는 -1을 출력한다. 가장 작은 줄어드는 수가 1번째 작은 줄어드는 수이다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!

// 내림차순 배열 사용 (만들어지는 수가 줄어드는 수인지 확인할 필요 x)
var arr = [9,8,7,6,5,4,3,2,1,0]
var anw = Set<Int>()

backTracking(0, 0)

let sort = anw.sorted(by: <)

if sort.count <= N - 1 {
    print(-1)
} else {
    print(sort[N - 1])
}

func backTracking(_ num: Int, _ index: Int) {
    // 선택된 num Set에 insert
    anw.insert(num)
    
    // arr count == 10이기 때문에 index >= 10 이면 return
    if index >= 10 {
        return
    }
    
    // 줄어드는 수 만드는 법 : arr 배열의 index 값을 선택 or 선택하지 않느냐 2가지 경우 존재
    // 해당 num 선택하고 index + 1
    backTracking((num * 10) + arr[index], index + 1)
    // 해당 num 선택하지 않고 index + 1
    backTracking(num, index + 1)
}


```

</div>
</details>

#### [선발 명단](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/3980.%E2%80%85%EC%84%A0%EB%B0%9C%E2%80%85%EB%AA%85%EB%8B%A8)

![3980](/assets/img/3980.png)<br/>

챔피언스 리그 결승전을 앞두고 있는 맨체스터 유나이티드의 명장 퍼거슨 감독은 이번 경기에 4-4-2 다이아몬드 전술을 사용하려고 한다.<br/>

오늘 결승전에 뛸 선발 선수 11명은 미리 골라두었지만, 어떤 선수를 어느 포지션에 배치해야 할지 아직 결정하지 못했다.<br/>

수석코치 마이크 펠란은 11명의 선수가 각각의 포지션에서의 능력을 0부터 100까지의 정수로 수치화 했다. 0은 그 선수가 그 포지션에 적합하지 않다는 뜻이다.<br/>

이때, 모든 선수의 포지션을 정하는 프로그램을 작성하시오. 모든 포지션에 선수를 배치해야 하고, 각 선수는 능력치가 0인 포지션에 배치될 수 없다.

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let C = Int(readLine()!)!
var maxCnt = 11
var visitedPerson = Array(repeating: false, count: 11)
var visitedPosition = Array(repeating: false, count: 11)
var maxValue = Int.min
var curValue = 0

for _ in 0..<C {
    var playerInfo = [[Int]]()
    for _ in 0..<maxCnt {
        playerInfo.append(readLine()!.split{$0 == " "}.map{Int($0)!})
    }
    backTracking(0, playerInfo)
    print(maxValue)
    maxValue = Int.min
    curValue = 0
}

func backTracking(_ cnt: Int, _ playerInfo: [[Int]]){
    if cnt == maxCnt {
        maxValue = max(maxValue, curValue)
        return
    }
    
    
    for j in 0..<maxCnt where !visitedPosition[j] {
        if playerInfo[cnt][j] != 0 {
            visitedPosition[j] = true
            curValue += playerInfo[cnt][j]
            
            backTracking(cnt + 1, playerInfo)
            
            visitedPosition[j] = false
            curValue -= playerInfo[cnt][j]
        }
    }
}
```

</div>
</details>

<br/>

## Next Permutation 다음 순열

---

: <span style="color:#9fb584">**현재 순열보다 큰 바로 다음 순열**</span>을 찾는 알고리즘이다.

### 동작 방식

1. 순열의 뒤에서부터 탐색하며 뒤에서 봤을 때 오름차순이 깨지는 인덱스 i(피벗포인트)를 찾는다. (i < i + 1 인 경우)
  - ex) 2 4 `3` 8 5 1
  - i = 2
  - 피벗포인트가 존재하지 않는다면 현재 순열이 가장 큰 순열이다.
2. i보다 오른쪽에 위치한 요소들을 탐색하며 i보다 크고 가장 작은 인덱스 j를 찾는다.
  - ex) 2 4 3 8 `5` 1
  - j = 5
3. i와 j를 swap한다.
  - ex) 2 4 `5` 8 `3` 1
4. i+1부터 마지막 인덱스까지 값을 오름차순으로 정렬한다.
  - i+1부터 마지막 인덱스까지 값을 reverse 해도 된다.
    - 3번 swap 후 i의 오른쪽 요소들이 내림차순으로 정렬되어 있기 때문이다.
  - ex) 2 4 5 `1 3 8`

<br/>

#### [애너그램](https://github.com/gyeom-ji/codingtest/tree/main/%EB%B0%B1%EC%A4%80/Gold/6443.%E2%80%85%EC%95%A0%EB%84%88%EA%B7%B8%EB%9E%A8)

씬디는 애너그램(anagram) 프로그램을 만들어 줄 수 있는 남자를 좋아한다. 참고로 씬디는 매우 예쁘다.<br/>

애너그램 프로그램이란, 입력받은 영단어의 철자들로 만들 수 있는 모든 단어를 출력하는 것이다. 가령 "abc" 를 입력받았다면, "abc", "acb", "bac", "bca", "cab", "cba" 를 출력해야 한다.<br/>

입력받은 단어내에 몇몇 철자가 중복될 수 있다. 이 경우 같은 단어가 여러 번 만들어 질 수 있는데, 한 번만 출력해야 한다. 또한 출력할 때에 알파벳 순서로 출력해야 한다.

``` Tip
💡 사전 순으로 출력해야하기 때문에 입력값을 < 로 정렬한다.
 　 정렬한 값이 첫 번째 문자열이기 때문에 anw에 더한다.
 　 arr을 Next Permutation 알고리즘을 사용해 다음 문자열로 갱신하고 anw에 더한다.
```

<details>
<summary>코드</summary>
<div markdown="1">

``` swift
let N = Int(readLine()!)!
var set = Set<String>()
var visited = [Bool]()
var anw = ""

for _ in 0..<N {
    // 사전 순으로 출력해야하기 때문에 < 로 정렬
    var arr = Array(readLine()!).sorted(by: <)

    // 정렬한 값이 첫 번째 문자열이기 때문에 anw에 +
    for i in 0..<arr.count {
        anw += "\(arr[i])"
    }
    anw += "\n"
    
    // arr을 다음 문자열로 갱신하고 anw에 +
    while nextPermutation(&arr) {
        for i in 0..<arr.count {
            anw += "\(arr[i])"
        }
        anw += "\n"
    }
}

print(anw)

/// Next Permutation 알고리즘
func nextPermutation(_ arr: inout [Character]) -> Bool {
    // 순열의 뒤에서부터 탐색
    var i = arr.count - 2, j = arr.count - 1
    
    // 1. 피벗포인트 i 찾기
    // i < i + 1
    while(i >= 0 && arr[i] >= arr[i + 1]) {
        i -= 1
    }
    
    // 피벗포인트가 존재하지 않으면 return false
    // 현재 순열이 가장 큰 순열
    if i < 0 {return false}
    
    // 2. 피벗포인트보다 오른쪽에 있는 값 중 피벗 값보다 큰 가장 작은 값 찾기
    // j > i
    while j > i && arr[j] <= arr[i] {
        j -= 1
    }
    
    // 3. i와 j swap
    arr.swapAt(i, j)
    
    // 4. i + 1부터 끝까지 요소들을 오름차순으로 정렬
    j = arr.count - 1
    i += 1
    
    while i < j {
        arr.swapAt(i, j)
        j -= 1
        i += 1
    }
    return true
}

/// 시간 초과
func backTracking(_ cur: Int, _ maxCnt: Int, _ arr: [Character], _ str: String) {
    if cur == maxCnt {
        set.insert(str)
        return
    }
    
    for i in 0..<maxCnt where !visited[i] {
        visited[i] = true
        backTracking(cur + 1, maxCnt, arr, str + "\(arr[i])")
        visited[i] = false
    }
}
```

</div>
</details>

<br/>
<br/>

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source