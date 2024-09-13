---
title: "트랜잭션"
author: gyeomji
date: 2024-08-02 10:00:00 +0900
categories: [Database]
tags: [트랜잭션, DeadLock, Locking, Recovery]
pin: false
math: true
mermaid: true
---

<br/> 

## 트랜잭션 Transaction

---

- 인가받지 않은 사용자로부터 데이터를 보장하기 위해 DBMS가 가져야 하는 특성이다.
- 데이터베이스 시스템에서 하나의 논리적 기능을 정상적으로 수행하기 위한 작업의 기본 단위이다.
- 데이터를 다룰 때 장애가 일어난 경우 데이터를 복구하는 작업의 단위가 된다.
  - 트랜잭션 실행 중 failure가 발생하면 데이터베이스는 일관되지 않은 상태(inconsistent state)가 될 수 있다.
  - 장애 발생 시 트랜잭션 실행 이전의 일관된 상태(consistent state)로 회복(recovery)시켜야 한다.
- 여러 작업이 동시에 같은 데이터를 다룰 때 이 작업을 서로 분리하는 단위가 된다.
  - 동시에 여러 트랜잭션이 실행될 경우 그 실행이 섞일 수 있다. (interleaved)
  - 즉, 각 트랜잭션의 실행은 다른 트랜잭션에 의해 간섭(interference)을 받을 수 있다.
- 동시에 동일한 자원을 접근하는 트랜잭션들의 실행은 서로 격리(isolation)되어야 한다.

![transaction](/assets/img/transaction.png)

<br/>

### 트랜잭션의 성질 ACID

---

- 원자성 Atomicity - “All or Noting” propetry
  - 트랜잭션은 단위 작업(indivisible unit)이다. 
  - 전부가 실행되거나 또는 실행되지 않아야 한다. 
  - 원자성은 recovery subsystem의 책임이다.
- 일관성 Consistency
  - 트랜잭션의 실행은 데이터베이스를 일관된 상태로 유지해야 한다.
  - 시스템이 가지고 있는 고정요소는 트랜잭션 수행 전과 트랜잭션 수행 완료 후 상태가 같아야 한다.
  - 일관성은 DBMS(스키마의 무결정 제약조건 보장)와 개발자(논리적 무오류)의 책임이다.
- 고립성 Isolation
  - 동시에 실행되는, 동일 자원을 접근하는 트랜잭션들은 서로 독립적으로 실행되어야 한다.
  - 즉, 한 트랙잭션이 완료되기 전 중간 결과는 다른 트랜잭션이 접근할 수 없어야 한다. 
  - 고립성은 concurrency control subsystem의 책임이다.
    - 트랜잭션의 직렬화 수준(Levels of serializability of transactions)
- 지속성 Durability
  - 성공적으로 완료한(committed) 트랜잭션 결과는 반드시 데이터베이스에 저장해야 한다
  - 지속성은 recovery subsystem의 책임이다.


<br/>

### DBMS 트랜젝션 하위 시스템

---

![transaction](/assets/img/transaction1.png)

#### Transaction manager

- 트랜잭션을 조정한다.

#### Scheduler

- 동시성 제어 전략(concurrency control strategy)을 구현한다.
    > Concurrency Control : DB에서 동시 옵션을 서로 간섭하지 않고 관리하는 프로세스

#### Recovery manager

- 장애 발생시 데이터베이스의 일관성 유지를 책임진다.
- 즉, DB를 트랜잭션 실행 이전의 consistent state로 회복(recovery)시킨다.

#### Buffer manager

- 메모리와 디스크 사이의 효율적인 데이터 전송을 책임진다.

<br/>

### 트랜잭션의 상태 변화

---

![transaction](/assets/img/transaction2.png)

- 활동 상태 : 초기상태로 트랜잭션이 실행 중일 때 가지는 상태
- 부분 완료 상태 : 트랜잭션의 마지막 문장을 실행한 상태
  - 트랜잭션의 실행 결과는 메모리 버퍼에 저장되어 있다.
  - COMMIT 문이 실행된 상태이다.
- 완료 상태 : 메모리 버퍼의 내용이 실제로 데이터베이스에 기록된 상태
  - 트랜잭션이 성공적으로 완료된 상태이다.
- 실패 상태 : 정상적인 실행이 더 이상 진행될 수 없을 때 가지는 상태
- 철회 상태 : 트랜잭션이 취소되고 데이터베이스가 트랜잭션 시작 전 상태로 환원된 상태

<br/>

### 트랜잭션 제어 명령어 TCL(Transation Control Language)

---

- START TRANSACTION : 트랜잭션 시작
- COMMIT : 트랜잭션을 메모리에 영구적으로 저장한다.(성공적인 종료)
  - 트랜잭션이 성공적으로 끝났다.
  - 데이터베이스는 새로운 일관된 상태를 가진다.
  - 트랜잭션이 수행한 갱신을 데이터베이스에 반영해야 한다.
- ROLLBACK : 트랜잭션을 전체 또는 SAVEPOINT 까지 무효화 시킨다.(비성공적인 종료)
  - 트랜잭션의 일부를 성공적으로 끝내지 못했다.
  - 데이터베이스가 불일치 상태를 가질 수 있다.
  - 트랜잭션이 수행한 갱신이 데이터베이스에 일부 반영되었다면 취소해야 한다.
- SAVEPOINT : SAVEPOINT를 만든다.

<br/>

### 트랜잭션 장애 failure 원인 

---

- H/W 또는 S/W 오류로 인해 시스템이 충돌하여 메모리가 손실된 경우
- 디스크 헤드 크래시 또는 판독 불가능한 미디어와 같은 미디어 장애로 인해 보조 스토리지의 일부가 손실된 경우
- 하나 이상의 트랜잭션을 실패하게 하는 프로그램의 논리적 오류와 같은 응용 프로그램 오류가 발생한 경우
- 정전, 화재, 홍수 또는 지진과 같은 자연적인 물리적 재해가 발생한 경우
- 운영자 또는 사용자에 의한 부주의 또는 의도하지 않은 데이터/시설 파괴가 발생한 경우
- 데이터, H/W 또는 S/W 시설의 방해 또는 의도적인 손상/파괴가 발생한 경우

<br/>

## 동시성 제어 Concurrency Control

---

- 여러 사용자들이 다수의 트랜잭션들을 동시에 수행하는 환경에서 부정확한 결과를 생성할 수 있는 트랜잭션들 간의 간섭이 생기지 않도록 하는 기법이다.
- 데이터베이스의 일관성 유지를 위해 상호 작용을 제어한다.

<br/> 

### 동시성 제어 종류

---

- 직렬 스케줄(serial schedule)
  - 여러 트랜잭션들의 집합을 한 번에 한 트랜잭션씩 차례대로 수행한다.
- 비직렬 스케줄(non-serial schedule) 
  - 여러 트랜잭션들을 동시에 수행한다.
- 직렬가능(serializable)
  - 비직렬 스케줄의 결과가 어떤 직렬 스케줄의 수행 결과와 동등하다.


<br/> 

### 동시성 제어를 하지 않고 다수의 트랜잭션을 동시에 수행할 때 생길 수 있는 문제

---

#### 갱신 손실 Lost Update

- 수행 중인 트랜잭션 T1이 갱신한 내용을 다른 트랜잭션 T2가 덮어쓸 때 T1의 갱신이 무효(손실)되는 오류이다.

![transaction](/assets/img/lostUpdate.png)

> [ 데이터베이스 연산 ]<br/>
> Input(X) : 데이터베이스 항목 X를 포함하고 있는 블록을 주기억 장치의 버퍼로 읽어들인다.<br/>
> Output(X) : 데이터베이스 항목 X를 포함하고 있는 블록을 디스크에 기록한다.<br/>
> read_item(X) : 주기억 장치 버퍼에서 데이터베이스 항목 X의 값을 프로그램 변수 X로 복사한다.<br/>
> write_item(X) : 프로그램 변수 X의 값을 주기억 장치 내의 데이터베이스 항목 X에 기록한다.<br/>
> ![transaction](/assets/img/transaction3.png)


#### 오손 데이터 읽기 Dirty Read

- 트랜잭션 T1이 완료되지 않은 다른 트랜잭션 T2(아직 COMMIT 하지 않은 상태)가 갱신한 데이터를 읽어 발생하는 오류이다.

![transaction](/assets/img/dirtyRead.png)

#### 반복할 수 없는 읽기 Unrepeatable Read

- 한 트랜잭션이 동일한 데이터를 두 번 읽을 때 서로 다른 값을 읽어 발생하는 오류이다.
- T2의 갱신 작업(COMMIT된 변경 사항)으로 인해 T1은 동일한 데이터를 읽었을 때 이전과 다른 값을 얻게 된다.

![transaction](/assets/img/unrepeatableRead.png)


####  유령 데이터 읽기 Phantom Read

- 한 트랜잭션이 동일한 데이터를 두 번 읽을 때 서로 다른 값을 읽어 발생하는 오류이다.
- T2의 삽입 작업으로 인해 T1은 동일한 데이터를 읽었을 때 이전에 존재하지 않은 행을 얻게 된다.

![transaction](/assets/img/phantomRead.png)

<br/> 

### 동시성 제어 기법

---

#### Locking

- 동시에 수행되는 트랜잭션들의 동시성을 제어하기 위한 기법이다.
- 데이터 항목 접근 전에 <span style="color:#9fb584">**lock을 요청**</span>한다.
  > <span style="color:#9fb584">**로크 Lock**</span> : 데이터베이스 내의 각 항목과 연관된 하나의 변수
- 허용되면 실행을 계속하고, 허용되지 않으면 실행을 일시 중지하고 <span style="color:#9fb584">**대기(wait)한다.**</span>
- 트랜잭션이 데이터 항목에 대한 접근을 끝낸 후 <span style="color:#9fb584">**lock을 해제(unlock)**</span>한다.

![transaction](/assets/img/locking.png)


<br/> 

#### 공유 로킹 방법 Shared Locking Protocol

- 트랜잭션에서 갱신을 목적으로 데이터 항목을 접근할 때는 <span style="color:#9fb584">**독점 로크(X-lock, eXclusive lock)**</span>을 요청한다.
  - 다른 트랜잭션은 읽기, 쓰기 불가

![transaction](/assets/img/exclusiveLocking.png)


- 트랜잭션에서 읽을 목적으로 데이터 항목을 접근할 때는 <span style="color:#9fb584">**공유 로크(S-lock, Shared lock)**</span>를 요청한다.
  - 다른 트랜잭션은 읽기 가능, 쓰기 불가

![transaction](/assets/img/sharedLocking.png)

- 공유 로킹 방법은 직렬가능성(serializability)을 보장하지 못한다.
  > <span style="color:#9fb584">**Serializable transactions**</span> : 여러 개의 동시 트랜잭션들(concurrent transactions) <br/>
  > 독립적으로 실행한 경우와 동일한 결과를 산출해야 한다. <br/> 

![transaction](/assets/img/sharedLocking1.png)

- 현재 A = 100, B = 200이라고 할 경우 (A, B) 값은 T1, T2 순서로 실행될 경우 (202, 402) T2, T1 순서로 실행될 경우 (201, 401)이 되어야 한다.
- 실행결과 : (A, B) = (202, 401)
  - T1이 너무 일찍 UNLOCK(A) 하여 T2가 일관되지 않은 데이터(inconsistent data)를 접근했기 때문이다.



<br/> 

#### 2단계 로킹 프로토콜 2-Phase Locking Protocol

- 로크를 요청하는 것과 로크를 해제하는 것이 2단계로 이루어진다.
- 로크 확장 단계가 지난 후 로크 수축 단계에 들어간다.
- 로크를 한 개라도 해제하면 로크 수축 단계에 들어간다.
- 로크 확장 단계(1단계)
  - 로크 확장 단계에서는 트랜잭션이 데이터 항목에 대하여 새로운 로크를 요청할 수 있지만, 보유하고 있던 로크는 하나도 해제할 수 없다.
- 로크 수축 단계(2단계)
  - 로크 수축 단계에서는 보유하고 있던 로크를 해제할 수 있지만 새로운 로크를 요청할 수 없다.
  - 로크 수축 단계에서는 로크를 조금씩 해제할 수도 있고 트랜잭션이 완료 시점에 이르렀을 때 한꺼번에 모든 로크를 해제할 수도 있다.
  - 일반적으로 한꺼번에 해제하는 방식이 사용된다.
- 로크 포인트(lock point)는 한 트랜잭션에서 필요로 하는 모든 로크를 걸어놓은 시점이다.
- 데드록이 발생할 수 있다.

![transaction](/assets/img/2phaseLocking.png)

<br/> 

#### 데드록 DeadLock

- 두 개 이상의 트랜잭션들이 서로 상대방이 보유하고 있는 로크를 요청하며 기다리는 상태를 말한다.
- 데드록을 해결하기 위해서는 데드록을 방지하는 기법이나 데드록을 탐지하고 희생자를 선정하여 데드록을 푸는 기법 등을 사용한다.

![transaction](/assets/img/deadLock.png)

① T1이 X에 대해 독점 로크를 요청하여 허가를 받는다.<br/> 
② T2이 Y에 대해 독점 로크를 요청하여 허가를 받는다.<br/> 
③ T1이 Y에 대해 공유 로크나 독점 로크를 요청하면 로크가 해제될 때까지 기다린다.<br/> 
④ T2가 X에 대해 공유 로크나 독점 로크를 요청하면 로크가 해제될 때까지 기다린다.

<br/> 

## 회복 Recovery

---

- 장애로 인해 DB의 상태가 부정확하거나 의심스러운 경우 올바른 상태로 복원시키는 것이다.
- 가능한 빨리 사용 가능한 상태 usable state로 복구해야 한다.
- 복구 유형은 Transaction Recovery, System Recovery, Media Recovery가 있다.

<br/> 

### 장애 Failure

- Local Failure
  - 특정 트랜잭션에서 발생하는 고장 
  - ex) overflow  
- System failure (or Soft crash)
  - 현행 TR들에만 영향을 미치는 고장 
  - ex) power failure
- Media failure (or Hard crash)
  - DB 일부와 그 부분을 사용하던 TR에 영향을 미치는 고장
  - ex) disk head failure
- Global Failure
  - System Failure + Media Failure

<br/> 

### Log (or Journal)

- 복구는 정보의 중복 저장을 통해 이루어지는데, 이때 사용하는 중복 정보를 log 또는 journal이라 한다.
- 모든 <span style="color:#9fb584">**갱신 연산에 대한 정보**</span>를 시간 순서로 기록하고 있는 파일이다.

![transaction](/assets/img/recoveryLog.png)

- Pointer : 동일한 TR의 변경 사항을 가리킨다. (0은 List 끝을 의미한다.)
- START, COMMIT/ROLLBACK : TR의 시작과 끝을 나타낸다.
- Before/After Image : 변경 전과 후의 값이다.

<br/> 

### Checkpoint 회복 기법

- Checkpoint : DB와 log가 <span style="color:#9fb584">**동기화**</span> 되는 시점이다.
  - 메모리버퍼 내용을 DB에 기록(force-writing)한 시점이다.
    - <span style="color:#9fb584">**현행 TR 리스트**</span>를 checkpoint record에 기록한다.
- Checkpoint는 복구 잡업을 빠르게 한다.
- 대부분 DBMS 제품은 자동적으로 Checkpoint 작업을 수행한다.
- System failure 발생 시 영향을 받는 트랜잭션
  - Commit을 했지만 기록(force-written)하지 않은 TR ➡️ REDO
  - 현재 진행 중인 TR ➡️ UNDO
- <span style="color:#9fb584">**Checkpoint record**</span> : <span style="color:#9fb584">**Checkpoint 시 진행 중**</span>인 모든 트랜잭션의 리스트이다.
  - undo, redo 할 트랜잭션 식별에 사용된다.

![transaction](/assets/img/checkpoint.png)

<br/> 

#### [ Checkpoint 설정하는 법 (DBMS) ]

1. 새 요청을 거절 한다.
2. 현재 요청을 종료한다.
3. <span style="color:#9fb584">**버퍼 내용을 디스크에 쓴다.**</span>
4. 쓰기가 완료될 때까지 기다린다. ⇨ log와 database가 동기화 된다.

<br/>

#### REDO/UNDO 트랜잭션 식별 절차

1. UNDO_LIST : checkpoint record, REDO_LIST : empty 
2. chekpoint 시점에서 <span style="color:#9fb584">**전진방향으로 Log를 탐색**</span>한다.<br/>
   ① 트랜잭션에 대한 `begin transaction` log entry를 발견하면 UNDO_LIST에 추가한다.<br/>
   ② 트랜잭션에 대한 `commit` 항목을 발견하면 UNDO_LIST에서 REDO_LIST로 옮긴다.<br/>
3. <span style="color:#9fb584">**log를 역방향 (Backward recovery)**</span>으로 따라가면서 UNDO_LIST에 있는 트랜잭션을 <span style="color:#9fb584">**취소**</span>시킨다. 
4. <span style="color:#9fb584">**log를 전진방향 (Forward recovery)**</span>으로 따라가면서 REDO_LIST에 있는 트랜잭션을 <span style="color:#9fb584">**재수행**</span>시킨다.

##### [ 예시 ]

![transaction](/assets/img/checkpoint.png)

1. UNDO_LIST = { T2, T3 }, REDO_LIST = { };
2. ① UNDO_LIST = UNDO_LIST ∪ { T4, T5 } <br/>
   ② UNDO_LIST = UNDO_LIST - { T2, T4 } REDO_LIST = REDO_LIST ∪ { T2, T4 }<br/>
3. Undo T3 and T5
4. Redo T2 and T4

<br/> 

## 고립 수준 Isolation Levels

---

- 한 트랜잭션이 다른 트랜잭션과 고립되어야 하는 정도를 나타낸다.
- 고립 수준이 낮으면 동시성은 높아지지만 데이터의 정확성이 떨어진다.
- 고립 수준이 높으면 데이터가 정확해지지만 동시성이 저하된다.
- 고립 수준에 따라 DBMS가 사용하는 로킹 동작이 달라진다.
- 한 트랜잭션이 명시한 고립 수준에 따라 그 트랜잭션이 읽을 수 있는 데이터에만 차이가 있다.


|고립 수준|오손 데이터 읽기|반복할 수 없는 읽기|유령 데이터 읽기|
|:------|:------:|:------:|:------:|
|READ UNCOMMITTED|⭕️|⭕️|⭕️|
|READ COMMITED|❌|⭕️|⭕️|
|REPEATABLE READ|❌|❌|⭕️|
|SERIALIZABLE|❌|❌|❌|

<br/>

### READ UNCOMMITED

- 가장 낮은 고립 수준이다.
- 트랜잭션 내 연산들이 공유 로크를 걸지 않고 데이터를 읽는다.
  - 한 트랜잭션에서 연산(갱신) 중인(아직 커밋되지 않은) 데이터를 다른 트랜잭션이 읽는 것을 허용한다.
- 오손 데이터를 읽을 수 있다.
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유한다.

<br/>

### READ UNCOMMITED

- 트랜잭션 내 연산들이 읽으려는 데이터에 대해 공유 로크를 걸고 읽기가 끝나자마자 해제한다.
  - 한 트랜잭션에서 연산(갱신)을 수행할 때, 연산이 완료될 때까지 연산 대상 데이터에 대한 읽기를 제한한다.
- 동일한 데이터를 다시 읽기 위해 공유 로크를 다시 걸고 데이터를 읽으면, 이전에 읽은 값과 다른 값을 읽는 경우가 생길 수 있다.
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유한다.

<br/>

### REPEATABLE READ

- 트랜잭션 내 연산에서 검색되는 데이터에 대해 공유 로크를 걸고, 트랜잭션이 끝날 때까지 보유한다.
  - 선행 트랜잭션이 특정 데이터를 읽을 때, 트랜잭션 종료 시까지 해당 데이터에 대한 갱신, 삭제를 제한한다.
- 한 트랜잭션 내에서 동일한 질의를 두 번 이상 수행할 때 매번 같은 값을 포함한 결과를 검색하게 된다.
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유한다.

<br/>

### SERIALIZABLE

- 가장 높은 고립 수준이다.
- 트랜잭션 내 연산에서 검색되는 투플들 뿐만 아니라 인덱스에 대해서도 공유 로크를 걸고 트랜잭션이 끝날 때까지 보유한다.
  - 선행 트랜잭션이 특정 데이터 영역을 순차적으로 읽을 때, 해당 데이터 영역 전체에 대한 접근을 제한단다.
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유한다.

<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source