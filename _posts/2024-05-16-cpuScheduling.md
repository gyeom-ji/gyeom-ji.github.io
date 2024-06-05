---
title: "CPU 스케줄링"
author: gyeomji
date: 2024-05-16 10:00:00 +0900
categories: [Operating System]
tags: [CPU Scheduling]
pin: false
math: true
mermaid: true
---

<br/> 

## CPU Scheduler

---

- 메모리에 있는 <span style="color:#9fb584">**준비 상태의 프로세스 중 하나를 선택하여 CPU의 자원을 할당**</span>한다.
- 프로세스의 실행은 CPU bursts와 I/O bursts의 연속이다.
    > CPU burst : CPU만 연속적으로 쓰면서 명령어를 실행하는 단계<br/>
    > I/O burst : I/O를 실행하는 단계
  - 짧은 CPU burst는 많고 긴 CPU burst는 적다.
  - CPU-bound, I/O-bound 프로그램은 스케줄링 알고리즘 선택에 영향을 준다.
    > I/O-bound 프로세스를 먼저 실행 상태로 옮기는 것이 좋다. I/O-bound 프로세스는 I/O 처리를 위하여 대기상태에 들어가는데, 이 때 CPU-bound 프로세스가 실행 상태로 옮겨질 수 있기 때문이다. (I/O 작업이 되는 동안 CPU는 기다리기만 할 뿐 자유롭다.) 이렇게 I/O-bound 프로세스가 실행 상태에 먼저 들어가는 경우를 사이클 훔치기(cycle stealing)라고 한다. 이는 I/O-bound 프로세스의 우선순위를 CPU-bound 프로세스보다 더 높게 설정하여 구현할 수 있다.

<br/> 

### Dispatcher

- CPU의 제어권을 CPU Scheduler에 의해 <span style="color:#9fb584">**해당 프로세스에게 넘기는(문맥 교환)역할**</span>을 수행한다.
- CPU Scheduler 내부에 포함되어 있다.
- 프로세스 문맥을 PCB에 저장한다(문맥 교환).
- 운영체제 모드(Kernel Mode)에서 사용자 모드(User Mode)로 전환시켜준다.
- 프로세스가 다음에 실행할 명령어의 주소를 가리키는 PC를 설정한다.

<br/> 

#### Dispatch Latency

![dispatchLatency](/assets/img/dispatchLatency.png){: width="300" height="300"}

- 현행 프로세스 중단부터 다른 프로세스 시작까지 걸리는 시간이다.

<br/> 
<br/> 

## CPU 스케줄링이 일어나는 시점

---

![processScheduling](/assets/img/processScheduling.png)

1. 실행(Running) ➡️ 대기(Waiting) 상태로 전환될 때 (I/O 요청 등): 비선점
2. 실행(Running) ➡️ 준비(Ready) 상태로 전환될 때 (인터럽트 발생 등): 선점
3. 대기(Waiting) ➡️ 준비(Ready) 상태로 전환될 때 (I/O 종료 등): 선점
4. 종료(Terminated)될 때 : 비선점


<br/> 
<br/> 

## CPU 스케줄링 종류

---

![cpuScheduling](/assets/img/cpuScheduling.png)

### 선점 스케줄링 Preemptive Scheduling

- 현재 실행 중인 프로세스가 OS에 의해 중단되고 준비 상태로 전환된다.
- 우선순위를 가지는 시분할 처리, 실시간 처리에 유용하다.
- 문맥 교환과 같은 부가적인 작업으로 인한 자원의 낭비가 있다.
- 공유 데이터 접근 중 선점시 프로세스 동기화가 필요하다.
- kernel mode에서 선점 시 시스템 호출 완료 후 선점된다.

<br/> 

### 비선점 스케줄링 Non-Preemptive Scheduling

- 프로세스가 종료(Terminated)되거나, 자체적으로 대기(Waiting)상대로 전환된 경우를 제외하고 실행 중인 프로세스가 계속 실행된다.
- 프로세스 스스로 CPU를 반환할 때 스케줄링이 일어난다.
- 짧은 작업이 긴 작업이 끝날 때까지 기다려야하는 문제점(Convoy Effect)가 발생할 수 있다.
> Convoy Effect: 하나의 큰 프로세스가 자원을 오랜 시간 독점하는 동안 작은 프로세스들이 자원을 할당받지 못하는 현상이다. 우선 순위가 같다고 가정했을 때 작은 프로세스들이 사용하고 그 다음 큰 프로세스가 사용하는 것이 성능상 유리하다.<br/>예시 ) 많은 I/O-bound 프로세스 앞에 CPU-bound 프로세스가 실행 중이다.


<br/> 
<br/> 

## CPU 스케줄링 기준

---

스케줄링 시 프로세스 선택에 영향을 주는 요소들이다.

| |스케줄링 기준||설명|
|------|------|------|------|
|전체 시스템 관점|CPU 이용률|⬆️|40~90% 사이에 있어야 한다.|
|전체 시스템 관점|처리량 Throughput|⬆️|단위 시간당 완료된 프로세스 개수|
|개별 프로세스 관점|총 처리시간(Turnaround time)|⬇️|프로세스 제출에서 완료까지 걸리는 시간|
|개별 프로세스 관점|대기 시간|⬇️|Ready queue 내에서 대기한 총 시간|
|개별 프로세스 관점|응답 시간|⬇️|하나의 요청이 제출 ~ 응답 생성(출력X)까지의 시간<br/>Interactive System에서 중요하다.<br/>응답 시간 변동폭의 최소화가 중요하다.|


<br/>
<br/>

## CPU 스케줄링 알고리즘

---

|알고리즘|프로세스 선택 기준|스케줄링 유형|
|------|------|------|
|FCFS|도착 순서|비선점|
|Round Robin|(도착)순서|선점|
|Priority|우선순위|비선점, 선점|
|SJF|다음 CPU burst time 길이|선점, 비선점|
|MLQ|고정 우선 순위|선점|
|MFQ|동적 우선 순위|선점|

<br/>

### FCFS(First-Come First-Served) Scheduling

- 프로세스가 Ready Queue에 도착한 순서대로 CPU를 할당한다.
- FIFO Ready Queue에서 가장 오랫동안 대기한 프로세스를 선택한다.
- 프로세스 자신이 CPU burst를 완료할 때 스케줄링을 시작하므로 비선점 스케줄링이다.

<br/>

### 장점

- 구현이 가장 간단하다.
- 스케줄러 구현이 용이하다. (FIFO Ready Queue 이용)


### 단점

- 평균 대기 시간이 매우 길어질 수 있다. (Convoy Effect 발생)

<br/>
<br/>

### RR(Round-Robin) Schedulin

- 프로세스가 Ready Queue에 도착한 순서대로 CPU를 할당한다. (FCFS와 동일하다)
- Time interrupt에 의해 스케줄링이 시작되므로 선점 스케줄링이다.
  - 현재 실행중인 프로세스는 ready queue의 tail에 추가된다.
  - RR의 Ready queue는 원형 큐로 설계되어 있다.
- Time-Sharing System의 CPU 공유 기법이다.
- 라운드로빈의 평균 총 처리시간 ATT(Average Turnaround Time)은 Time Quantum(Time Slice) q에 큰 영향을 받는다.
  - Time Slice → 무한대 : FCFS 스케줄링과 동일해져 해당 프로세스가 끝나야만 전환된다.(퇴보한다)
  - Time Slice → 0 : Processor Sharing 상태이고 Context Switching의 Overhead가 너무 커지게 된다. 
  - q가 문맥 교환 시간(보통 10μs 미만)보다 커야한다.
  - 평균 총 처리시간은 q에 의존하지만 정비례하지는 않는다.
  - 프로세스의 CPU burst 중 80% 이상이 q보다 짧아야 성능이 좋다.
> Time Quantum(Time Slice) : 일정 시간마다 프로세스가 교체되는데, 이 일정 시간을 Time Quantum(Time Slice)라 한다.

<br/>

### 장점

- 모든 프로세스가 공정하게 시간을 할당받기 때문에 기아 상태(Starvaion)이 발생하지 않는다. 
> 기아 상태 Starvaion : 자신 보다 우선순위가 높은 프로세스들 때문에 Ready process가 오랫동안 CPU를 할당받지 못하는 상태를 말한다.
- 프로세스의 최악의 응답시간을 아는 데에 용이하다.


### 단점

- 하드웨어 타이머가 필요하다.
- 작업 시간 할당을 너무 짧게 하면 Context Switching이 자주 일어나 오버헤드가 일어난다.

<br/>
<br/>

### 우선순위 Priority Scheduling 

- Reday queue에 있는 프로세스 중 우선순위가 높은 프로세스를 선택한다.
- 우선순위는 내부적, 외부적으로 정의할 수 있다.
  - 내부적 우선순위 : 측정치를 사용하여 프로세스의 우선순위를 계산한다.
    - time limit, 메모리 요구, open file 개수, CPU/IO burst 비율
  - 외부적 우선순위 : 운영체제 외적 기준에 의해 설정된다.
    - 프로세스의 중요도, payment, 수행 부서(주체), 정치적 요인
- 선점형과 비선점형 방법을 적용할 수 있다.
- 비선점형 : 실행 중인 프로세스가 자신의 CPU burst를 완료할 때 스케줄링을 시작한다.
- 선점형 : 우선순위가 높은 프로세스가 Ready queue에 도착하는 즉시 스케줄링을 시작한다.

<br/>

### 장점

- 각 프로세스의 상대적 중요성을 정확히 정의할 수 있다.


### 단점

- 계속해서 우선순위가 높은 프로세스가 자원을 할당 받을 수 있어 Starvaion이 생길 수 있다.
  - Aging을 적용하여 해결한다.
    > Aging : 오랫동안 ready 상태인 프로세스의 우선순위를 주기적으로 높여준다.

<br/>
<br/>

### SJF(Shortest Job First) Scheduling 

- 다음 CPU burst time(T)이 가장 짧은 프로세스 선택한다.
  - T가 같을 경우 도착 순서대로 선택한다.
- 선점형과 비선점형 방법을 적용할 수 있다.
- 비선점형 : 실행 중인 프로세스가 자신의 CPU burst를 완료할 때 스케줄링을 시작한다.
- SRTF (Shortest Remaining Time First) 선점형 : remaining T가 더 짧은 프로세스가 Ready queue에 도착하는 즉시 스케줄링을 시작한다.
- 프로세스의 next CPU burst 길이는 사전에 알 수 없지만 예측이 가능하다.
  - 지수평균법(Exponential averaging) : 최근과 과거 CPU burst time들 간에 가중치를 둔다.

<br/>

### 장점

- 주어진 프로세스 집합에 대해 최소 평균 대기 시간을 보장한다. (최적 알고리즘)
  - short process를 앞에 두면 대기시간이 단축된다.


### 단점

- 계속해서 우선순위가 높은 프로세스가 자원을 할당 받을 수 있어 Starvaion이 생길 수 있다.
  - Aging을 적용하여 해결한다.
    > Aging : 오랫동안 ready 상태인 프로세스의 우선순위를 주기적으로 높여준다.

<br/>
<br/>


### 다단계 큐 MLQ(Multi-level Queue) Scheduling

- 작업들의 여러 종류의 Ready Queue로 구분하여 스케줄링하는 기법으로 각 Ready Queue마다 독자적인 우선순위와 스케줄링 알고리즘을 사용하여 CPU를 할당한다. 
  - 프로세스 속성(유형)에 따라 특정 큐에 영구 배정한다. (Fixed priority)
- 작업시간과 대기시간만을 고려한 단순한 우선순위 할당보다 작업 자체의 우선순위를 고려하는 좀 더 고도화된 스케줄링 기법이다.
- 큐 간 스케줄링은 선점 스케줄링이다.
  - 한 레벨의 큐는 자신의 모든 상위 큐가 empty일 때만 스케줄링 된다.
  - 실행중인 하위 큐의 프로세스는 상위 큐에 프로세스가 도착하면 선점된다.

<br/>
<br/>

### 다단계 피드백 큐 MFQ (Multilevel Feedback Queue)Scheduling

- 큐 간 스케줄링은 다단계 큐 스케줄링과 동일하다.
  - process는 최우선 큐에 배정된 후 자신의 CPU burst 특성에 따라 하위 큐로 이동한다. (Dynamic Priority)

<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
