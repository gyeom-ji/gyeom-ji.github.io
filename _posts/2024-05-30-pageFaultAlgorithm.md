---
title: "페이지 교체 알고리즘"
author: gyeomji
date: 2024-05-30 10:00:00 +0900
categories: [Operating System]
tags: [PageFaultAlgorithm, Operating System]
pin: false
math: true
mermaid: true
---


<span style="color:#9fb584">**재배치 기법으로 주기억장치에 있는 프로세스 중 어떤 프로세스를 제거할 것인지 결정**</span>하는 기법이다. 메모리 교체 대상(Who)을 결정한다.


## 희생 페이지 Victim Page

---

- 메모리에 적재된 페이지 중 CPU에 수정(Modify)되지 않은 페이지를 고르는 것이 효율적이다.
  - 수정되지 않은 페이지는 swap-out될 때 스왑 공간에 쓰기 연산을 할 필요가 없기 때문이다.
  - 스왑 공간은 읽는 시간도 느리지만, 쓰기 작업까지 더해지면 더욱 비효율적이다.
- 페이지 테이블에 Modified Bit(Dirty Bit)을 추가하여 검사한다.

<br/>

## 1. FIFO (First In First Out)

---

- 각 페이지가 주기억장치에 적재될 때마다 <span style="color:#9fb584">**가장 먼저 들어온 페이지**</span>를 교체하는 기법
- Victim Page = <span style="color:#9fb584">**가장 먼저 적재된 페이지**</span>
- FIFO Queue를 사용하여 구현한다.
- 구현이 쉽지만, 성능이 항상 좋지는 않다.
- 할당된 프레임 수가 증가해도 페이지 부재율이 증가하는 경우가 발생할 수 있다. (Belady's Anomaly)

![pageFaultFIFO](/assets/img/pageFaultFIFO.png)


## 2. LRU (Least Recently Used)

---

- <span style="color:#9fb584">**가장 오랫동안 사용되지 않은 페이지**</span>를 교체하는 방법
- 프로그램의 지역성의 원리에 따라 최근에 참조된 페이지는 앞으로도 참조될 가능성이 크고, 최근에 참조되지 앟은 페이지는 그렇지 않을 가능성이 크다는 전제로 구현된 알고리즘
- Victim Page = <span style="color:#9fb584">**가장 오래 전에 사용된 페이지**</span>
- 각 페이지마다 <span style="color:#9fb584">**마지막 사용 시각을 유지**</span>하는 것이 요구된다.
    - Counter, Stack과 같은 H/W 지원이 필요하다.
- 여러 OS에서 사용되며, 좋은 알고리즘으로 간주된다.


[ Counter(in a Page table) 사용 방법 ]
  
- 페이지 참조때 마다 마지막 사용 시각으로 설정된다.
- Victim = Counter 값이 가장 작은 페이지
- 메모리 참조때 마다 counter field 갱신이 필요하다. (추가적인 memory write 1회)
- 희생 페이지 선정시 페이지 테이블 검색이 필요하다.


[ Stack 사용 방법 ]

- 새 페이지 참조 시 top에 저장하고, 기존 페이지 참조 시 top으로 이동한다.
- Victim = Bottom entry
- stack 갱신 비용이 올라간지
- 희생 페이지 선정이 효율적이다.
- Belady's anomaly를 야기하지 않는다.

![pageFaultLRU](/assets/img/pageFaultLRU.png)


## 3. LFU (Least Frequently Used)

---

- <span style="color:#9fb584">**참조 횟수가 가장 적은 페이지**</span>를 교체하는 방법


## 4. OPT (OPTimal Replacement)

---

- <span style="color:#9fb584">**앞으로 가장 오랫동안 사용하지 않을 페이지**</span>를 교체하는 방법
- <span style="color:#9fb584">**페이지 부재 횟수가 가장 적게 발생**</span>하는 효율적인 알고리즘
- future reference string에 대한 정보가 필요하다.
- 프레임 수가 고정된 경우 가장 낮은 페이지 부재율을 보장한다.

![pageFaultOPT](/assets/img/pageFaultOPT.png)

## 5. NUR (Not Used Recently)

---

- LRU와 유사한 알고리즘으로, <span style="color:#9fb584">**최근에 사용하지 않은 페이지**</span>를 교체하는 방법
- LRU에서 나타나는 시간적인 오버헤드를 줄일 수 있다.
- 최근 사용 여부를 확인하기 위해 페이지마다 참조 비트와 변형 비트를 사용한다


## 6. SCR (Second Chance Replacement)

---

- <span style="color:#9fb584">**가장 오랫동안 주기억장치에 있던 페이지 중, 자주 사용되는 페이지의 교체를 방지**</span>하기 위한 기법으로 FIFO의 단점을 보완한다.

<br/>

> Page Fault에 영향을 주는 요소<br/>
> 1. 페이지 교체 알고리즘 : 페이지 부재율이 낮은 알고리즘이 좋다. (교체 알고리즘 별 페이지 부재 횟수 * 참조 스트링)<br/>
> 2. 프레임 할당 알고리즘 : 프로세스에게 프레임을 할당하는 숫자(방법)에 따라 페이지 부재율이 영향을 받는다. (보통 프레임이 많을수록 페이지 부재율이 낮다.)

<br/>

## 페이지 교체 동작 과정

---

1. 디스크에서 요구된 페이지 위치를 검색한다.
2. 빈 프레임이 존재할 경우 빈 프레임을 사용한다.
3. 빈 프레임이 존재하지 않을 경우 <span style="color:#9fb584">**페이지 교체 기법에 따라 희생 페이지를 선택**</span>한다. 
   1. 희생자 페이지를 swap out하고, 페이지 및 페이지 테이블을 갱신한다.
4. 요구된 페이지를 swap in(read)하고, 페이지 및 페이지 테이블을 갱신한다.
5. 프로세스 실행을 재계한다.

<br/>
<br/> 

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source

