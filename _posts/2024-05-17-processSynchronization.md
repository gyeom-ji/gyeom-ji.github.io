---
title: "프로세스 동기화"
author: gyeomji
date: 2024-05-17 10:00:00 +0900
categories: [Operating System]
tags: [Process Synchronization, Mutex, Semaphore, Monitor]
pin: false
math: true
mermaid: true
---

<br/> 

## Cooperating Process

---

- 다른 프로세스에게 <span style="color:#9fb584">**영향을 주거나, 다른 프로세스로부터 영향을 받는 프로세스**</span>이다.
- 협력적 프로세스들은 보통 <span style="color:#9fb584">**자원을 공유하며, 동시에 접근**</span>한다.
- 협력적 프로세스들은 서로 영향을 미치기 때문에<span style="color:#9fb584">**데이터나 흐름에 대한 동기화가 매우 중요**</span>하다.
  - 프로세스 사이에 동기화 하는 것을 <span style="color:#9fb584">**프로세스 동기화 (Process Synchronization)**</span>라고 한다.
- 경쟁 상황(Race Condition)이 발생하면 공유 자원을 신뢰할 수 없게 만들 수 있다.
> 경쟁 상황(Race Condition) : 2개 이상의 프로세스가 공유 자원을 병행적으로 읽거나 쓰는 상황을 말하며, 공유 <span style="color:#9fb584">**자원 접근 순서에 따라 실행 결과가 달라지는 상황**</span>을 말한다. 즉, 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태를 말하는 것이다.

<br/> 
<br/> 

## 프로세스 동기화

---

- 협력하는 프로세스 사이에서 <span style="color:#9fb584">**공유자원의 일관성(Consistency) 유지를 위해 프로세스 간 실행 순서를 제어**</span>하는 것이다.
  - <span style="color:#9fb584">**순차 접근과 동일한 결과**</span>를 산출하도록 실행 순서를 제어해야 한다.
- 협력 프로세스가 공유 자원을 사용하는 상황에서, 경쟁 상황이 발생하면 공유 자원을 신뢰할 수 없게 만들 수 있는데, 이를 방지하기 위해 프로세스들이 공유 자원을 사용할 때 특별한 규칙을 만드는 것이다.
- 동기화 유형에는 경쟁적(Competitive), 협력적(Cooperative) 동기화가 있다.

<br/> 
<br/> 

## 동기화 관련 문제

---

1. 은행 계좌 문제 Bank Account Problem
    - 부모는 은행 계좌에 입금하고 자식은 은행 계좌에서 출금한다.
    - 입금과 출금 과정이 별도로 이루어져야 한다.
    - Critical Section : 은행 계좌
2. 독자 저자 문제 Readers Writers Problem
    - 독차는 책(공유 데이터베이스)에 쓰여진 글을 읽고, 저자는 책에 글을 써 추가한다.
    - 독자가 글을 읽고 있다면 독자는 추가적으로 글을 읽을 수 있지만 저자는 글을 쓸 수 없다.
    - 저자가 글을 쓰고 있다면 독자는 글을 읽을 수 없으며 저자도 추가적으로 글을 쓸 수 없다.
    - Critical Section : 책(공유 데이터베이스)
3. 생상자 소비자 문제 Producer Consumer Problem (Bounded Buffer Problem)
    - 생산자는 물건을 생산하여 창고(버퍼)에 넣고, 소비자는 창고에서 물건을 꺼내 소비한다.
    - 창고가 가득 차면 생산자는 물건을 넣을 수 없고, 창고가 비어 있으면 소비자는 물건을 소비할 수 없다.
    - Critical Section : 창고(버퍼)
4. 식사하는 철학자 문제 Dining Philosopher Problem
    - 원형 테이블에 철학자들이 앉아있고 철학자 수만큼 젓가락이 철학자 사이에 하나씩 놓여있다.
    - 철학자들이 식사를 하기 위해서는 양쪽에 하나씩 놓여있는 젓가락을 둘 다 들어서 사용해야 한다.
    - 어떤 철학자가 젓가락을 사용중이라면, 다른 어떤 철학자는 식사를 할 수 없다.
    - Critical Section : 젓가락

<br/> 
<br/> 

## 임계 구역 문제 (Critial Section Problem)

---

- <span style="color:#9fb584">**Critial Section**</span> : 둘 이상의 스레드가 동시 실행될 경우 경쟁 상황을 발생시킬 수 있는 코드 영역(<span style="color:#9fb584">**공유 자원을 접근하는 코드 영역**</span>)
- 한 프로세스가 자신의 임계구역 내에 있을 때 다른 프로세스들은 자신들의 임계구역으로 들어갈 수 없다.
- <span style="color:#9fb584">**Critial Section Problem**</span> : <span style="color:#9fb584">**임계구역 전후에서 프로세스간 협력 규약을 설계하는 것**</span>이다.
- Critial Section Problem의 해결 방안은 <span style="color:#9fb584">**3가지 요구 조건을 충족**</span>해야한다.

### 임계 구역 문제 해결방안 3가지 충족 조건

1. <span style="color:#9fb584">**상호 배제 Mutual Exclusion**</span>
    - 한순간 오직 하나의 프로세스만 공유의 지정 임계구역에 들어갈 수 있다. (공유자원을 접근할 수 있다.)
    - 한 프로세스가 임계구역에서 수행 중인 상태에서는 다른 프로세스는 이 구역에 절대 접근 불가능하다.
2. <span style="color:#9fb584">**진행 Progress**</span>
    - 임계구역을 실행 중인 프로세스가 없고, 임계구역을 진입하려는 프로세스들이 존재할 경우, 다음 프로세스의 선정은 무한히 연기되지 않아야 한다.
    - Deadlock 방지
    > Dealock 교착상태 : 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 이벤트를 무한히 기다리는 현상이다.
3. <span style="color:#9fb584">**유한 대기 Bounded Waiting**</span>
    - 프로세스의 진입 요청은 유한한 시간 내에 허락되어야 한다.
    - Starvation 방지

<br/> 
<br/> 

## 동기화 장치

---

|하드웨어 장치|소프트웨어 장치|
|------|------|
|• Interrupt Disabling<br/>　 　- H/W의 Interrupt Enable/Disable bit를 설정한다.<br/>• Special Atomic Instruction<br/>　 　- Test-and-Set instruction<br/>　 　- Compare-and-Swap instruction|• Mutex lock<br/>• Semaphore<br/>• Monitor|
|동기화 프로그래밍이 복잡해진다.|-|

<br/> 
<br/> 

## Mutex Lock

---

- 공유된 자원의 데이터 혹은 임계영역 등에 <span style="color:#9fb584">**하나의 Process 혹은 Thread가 접근하는 것을 막아준다.(즉, 동기화 대상이 하나)**</span>
- 임계구역을 가진 스레드들의 실행시간(Running Time)이 서로 겹치지 않고 각각 <span style="color:#9fb584">**단독으로 실행**</span>되도록 한다.
  - 서로 간 공유 자원의 동시 접근을 허용하지 않는다.
- Locking 메커니즘으로 <span style="color:#9fb584">**오직 하나의 스레드만이 동일한 시점에 뮤텍스를 얻어 임계 영역(Critical Section)에 들어올 수 있다.**</span>
- 오직 <span style="color:#9fb584">**해당 쓰레드만이 임계 영역에서 나갈 때 뮤텍스를 해제**</span>할 수 있다.
- 뮤텍스는 2가지 연산을 지원한다.
  - lock: 현재의 임계 구역에 들어갈 권한을 얻어온다. 만일 다른 프로세스/스레드가 임계 구역을 수행 중이라면 종료할때까지 대기한다(entry section).
  - unlock: 현재의 임계 구역을 모두 사용했음을 알린다. 대기중인 다른 프로세스/스레드가 임계 구역에 진입할 수 있다(exit section).
- 자원에 접근하기 위해 각 프로세스는 락을 획득하여 접근 허용 권한을 얻어야 한다.
- 한 프로세스가 접근 권한을 허용 받으면 다른 프로세스는 락이 반환되기 전까지 자원 사용이 불가능하다.
  - 프로세스는 lock이 반환될 때까지 spin한다.
    > Spin : 다른 스레드가 lock을 소유하고 있다면 그 lock이 반환될 때까지 계속 확인하며 기다리는 것이다. (Busy waiting)
- 프로세스들이 짧게 lock을 소유하는 경우에 적용한다.
- 다중 처리기 시스템에 적용한다.
  - 한 처리기에서 스레드가 임계구역을 실행하는 동안 다른 처리기에서 다른 스레드는 spin한다.

<br/> 

``` swift

// Mutex Lock

var available = Bool() // lock의 가용 여부를 나타낸다

func acquire() {  // lock 획득 atomic operation
    while !available {} // busy waiting – 계속 while
    available = false
}

func release() { // lock 반환 atomic operation
    available = true
}
```

``` swift

// 동기화 사용 방법
do {
    ...
    acquire( ) // lock 획득
    임계 구역
    release( ) // lock 반환
    ...
}

```

<br/>
<br/>

## Semaphore

---

- 공유된 자원의 데이터 혹은 임계영역 등에 <span style="color:#9fb584">**여러 Process 혹은 Thread가 접근하는 것을 막아준다.(즉, 동기화 대상이 하나 이상)**</span>
- 교착 상태(DeadLock)에 대한 해법으로 두개의 원자적(Atomic) 함수로 제어되는 정수 변수로 멀티프로그래밍 환경에서 공유자원에 대한 접근 제어를 하는 방법으로 사용되며, 1개의 공유되는 자원에 제한된 개수의 프로세스 또는 스레드만 접근할 수 있도록 한다.
    > 원자성(Atomicity) : 여러 개의 스레드가 존재할 때, 특정 시점에서 어떤 메소드를 2개 이상의 스레드가 동시에 호출하지 못하는 것을 말한다.
- 세마포어 S는 원자적으로 제어되는 정수 변수로 가용한 자원의 개수를 나타낸다.
  - S == 0 : 가용한 자원이 없음 (자원에 접근할 수 없도록 Block)
  - S > 0 : S개 자원이 가용함 (접근함과 동시에 세마포어의 값을 1 감소)
  - S < 0 : 기다리고 있는 프로세스의 개수
- 세마포어의 종류는 이진형 세마포어 Binary Semaphore와 계수형 세마포어 Counting Semaphore가 있다.
  - Counting Semaphore : unbounded domain 유한 개의 공유 자원 접근시 사용한다.
  - Binary Semaphore (≈ Mutex): domain = {0, 1} 1개의 공유 자원을 상호배제한다.
- 세마포어는 변수 S와 P연산, V연산으로 구성되어 있다.
  - S는 일반적으로 정수형 변수를 사용한다.
  - S는 P, V 명령에 의해서만 접근할 수 있다.
  - P는 임계 영역을 사용하려는 프로세스들의 진입 여부를 결정하는 조작으로 Wait 동작이라고도 한다.
  - V는 대기 중인 프로세스를 깨우는 신호(Wake-up)로 Signal 동작이라고도 한다.
  - P는 임계 구역에 들어가기 전 수행되고, V는 임계구역에서 나올 때 수행된다.
  - 이때 <span style="color:#9fb584">**변수 값을 수정하는 연산은 모두 원자성을 만족**</span>해야 한다.
  - 즉, <span style="color:#9fb584">**한 프로세스(스레드)에서 세마포어 값을 변경하는 동안 다른 프로세스가 동시에 이 값을 변경해서는 안된다.**</span>

<br/> 

### Busy Waiting 방식 (SpinLock)

``` swift

// Semaphore

var s = Bool() // 가용한 자원의 개수를 나타냄

func P(s: Int) {  // 공유 자원 획득 atomic operation
    while s <= 0 {} // busy waiting 
    s -= 1
}

func V(s: Int) { // 공유 자원 반환 atomic operation
    s += 1
}
```

``` swift

// 동기화 사용 방법
do {
    ...
    P(s) // 공유 자원 획득
    임계 구역
    V(s) // 공유 자원 반환
    ...
}

```

- 아무것도 하지 않는 빈 반복문을 계속 돌다가 임계 구역에 진입할 수 있을 때 진입하는 방식이다.
- 빈 반복문을 반복하기 때문에 계속적으로 Context Switching이 발생하며 이로 인하여 단일처리 다중프로세스 환경에서 처리 효율이 떨어진다.
- 어떤 프로세스가 먼저 임계 구역에 진입을 할 수 있을지에 대한 처리를 할 수 없다는 단점도 존재한다.

<br/>

### Block-WakeUp 방식 (Wait queue)

``` swift

// Semaphore

func P(s: Int) { 
    s -= 1
    if s < 0 {
        // 해당 프로세스를 대기 큐에 추가
        block() // 프로세스 일시 중지
    }
}

func V(s: Int) { 
    s += 1
    if s <= 0 {
        // 대기 큐에서 한 프로세스(P) 제거
        wakeup(P) // 프로세스 P 실행 재개
    }
}
```

- Busy Waiting의 단점을 보완한 방식으로, 프로세스를 대기 큐(wait queue)에 넣어 공유 자원을 관리한다.
- block()
  - 커널은 block을 호출한 프로세스를 Suspend(지연)시킨다.
  - 프로세스의 PCB(Process Control Block)를 Semaphore에 대한 wait queue에 넣는다.
- Wakeup()
  - block된 프로세스 p를 WakeUp 시킨다.
  - 이 프로세스의 PCB를 Ready queue로 옮긴다.

<br/>


### Busy Waiting vs Block-WakeUp

- Busy Waiting : multi-processor 환경에서 유리하다.
- Block-WakeUp : uni-processor 환경에서 유리하다.
  - wait(), signal() 내부 코드가 실행되는 도중 문맥교환이 발생하면 상호배제와 유한대기 조건을 보장하지 못한다.
  - wait, signal 내부코드는 분리 실행되지 않고 완전히 실행되게 해야 한다. (즉, atomicity가 보장되어야 한다)
  - wait(), signal()의 atomicity는 interrupt disabling으로 해결 가능하다.
  - starvation과 deadlock이 발생할 수 있다.
- 일반적으로 CPU 소모를 줄이는 Block-WakeUp 방식이 좋지만, Block-WakeUp 방식은 프로세스를 Wakeup하는 과정에서 발생하는 오버헤드가 존재한다.
- 임계 구역 길이가 길 경우 Block-WakeUp
- 임계 구역 길이가 매우 짧을 경우 Busy Waiting 

<br/>
<br/>

## Monitor

---

- 세마포어의 가장 큰 문제는 잘못된 사용으로 인해 임계구역이 보호받지 못한다는 것이다.<br/> 
    1) 세마포어를 사용하지 않고 임계구역에 들어갈 때 임계구역이 보호받지 못한다.(상호 배제가 보장되지 않아 race condition이 발생한다.)<br/> 
    2) P()만 두 번 사용하고 V()를 사용하지 않아 wake_up 신호가 발생하지 않은 경우 무한대기상태가 발생한다.<br/> 
    3) P()와 V()를 반대로 사용하여 상호배제가 보장되지 않는다.<br/> 
- 공유 자원을 사용할 때 모든 프로세스가 세마포어 알고리즘을 따른다면 P(), V()를 사용하지 않고 자동으로 처리하도록 하면 되는데 이를 실제로 구현한 것이 모니터이다.
- <span style="color:#9fb584">**임계 구역으로 지정된 변수, 자원에 접근하고자 하는 프로세스는 직접 P(), V()를 사용하지 않고 모니터에 작업을 요청**</span>한다.
- 모니터는 임계구역 보호와 동기화를 위해 내부적으로 조건 변수 Condition Variable를 사용한다.
  - 조건 변수는 특정 조건이 완료될 때까지 block상태로 만든다는 의미이다.
  - 조건 변수의 연산은 wait(), signal()이 있다.
  - wait() : 모니터 큐에서 자신의 차례가 올 때까지 기다린다. (세마포어 P()의 block과 같다)
  - signal() : 모니터 큐에서 기다리는 다음 프로세스에 순서를 넘겨준다. (세마포어의 V()에 해당한다. 오직 한 프로세스만이 모니터내에서 활성화가 가능하다.)
- 모니터는 요청받은 작업을 모니터 큐에 저장한 후 순서대로 처리하고 그 결과만 해당 프로세스에 알려준다.
- <span style="color:#9fb584">**모니터는 공유 자원을 내부적으로 숨기고 공유 자원에 접근하기 위한 인터페이스만 제공함으로써 자원을 보호하고 프로세스 간 동기화**</span>를 시킨다.
  - abstract data type의 안전 공유를 보장한다.
  - 상호 배제를 자동 보장한다.
- <span style="color:#9fb584">**하나의 프로세스 내 다른 스레드 간 동기화에 사용**</span>된다.


<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
