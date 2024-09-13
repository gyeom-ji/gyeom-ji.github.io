---
title: "메모리 관리 기법 - Segmentation 세그멘테이션"
author: gyeomji
date: 2024-05-25 10:00:00 +0900
categories: [Operating System]
tags: [Memory, Segmentation]
pin: false
math: true
mermaid: true
---

<br/> 

## 세그멘테이션 Segmentation

---

- 세그먼테이션은 프로세스를 <span style="color:#9fb584">**논리적 단위(Segment)로 나눠 메모리의 임의 위치에 적재**</span>한다.
  - 페이징은 프로세스를 물리적으로 일정한 크기(Page)로 나눠서 메모리에 할당한다.
- 프로세스를 세그먼트 집합으로 만들고, 각 <span style="color:#9fb584">**세그먼트의 크기는 일반적으로 같지 않다.**</span>
  - 프로세스를 Code, Data,Stack 으로 나누는 것 역시 세그먼테이션의 모습이다.
  - Code, Data,Stack 각각 내부에서 더 작은 세그먼트로 나눌 수도 있다.
  - method, procedure, function, object, variables, stack 등 함수단위로 나눌 수 있다.
  - C컴파일러 관점에서는 코드, 전역 변수, 힙, 스택, 표준 C 라이브러리 단위로 구분지어 나눌 수 있다.
- segment 유형에 따라 서로 다른 code sharing, protection이 가능해진다. 
- segment 길이는 가변적이므로 동적 할당 기법을 사용한다.
- 세그먼트를 <span style="color:#9fb584">**메모리에 할당할 때는 페이지를 할당하는 것과 동일하지만, 세그먼트 테이블을 사용**</span>한다.
- 세그먼트 테이블은 <span style="color:#9fb584">**세그먼트 번호와 시작 주소(Base), 세그먼트 크기(Limit)**</span>을 엔트리로 가진다.
- 주소 변환도 페이징과 유사하다.
  - <span style="color:#9fb584">**세그먼트의 크기는 일정하지 않기 때문에 테이블에서 Limit 정보**</span>가 주어진다.
  - CPU에서 해당 세그먼트 크기를 넘어서는 주소가 들어올 경우 <span style="color:#9fb584">**인터럽트가 발생해 해당 프로세스를 강제로 종료**</span>시킨다.


<br/>

## 세그멘테이션의 보호와 공유

---

- 페이징보다 효율적이다.
- 보호에서는 세그먼테이션도 R, W, X 비트를 테이블에 추가하는데 세그먼테이션은 논리적으로 나누기 때문에 해당 비트를 설정하기 매우 간단하고 안전하다.
  - 페이징은 Code, Data, Stack 영역이 있을 때 이를 일정한 크기로 나누므로 두 영역이 섞일 수 있다. (비트 설정이 까다롭다)
- 페이징에서는 Code 영역을 나눈다고 해도 다른 영역이 포함될 확률이 매우 높다.
- 세그먼테이션은 정확히 Code 영역만 나누기 때문에 더 효율적으로 공유를 수행할 수 있다.


<br/>

## 세그멘테이션과 페이징

---

- 세그먼테이션은 논리적인 단위로 나누기 때문에 세그먼트의 크기가 다양하다. 
- 이로 인해 다양한 크기의 Hole이 발생하기 때문에 외부 단편화 문제가 발생할 수 있다.
- 세그먼테이션은 보호, 공유에서 효율적이지만, 페이징은 외부 단편화 문제를 해결할 수 있다.
- 두 장점을 포함하여 세그먼트를 페이징 기법으로 나누는 방법이 나왔지만, 세그먼트와 페이지가 동시에 존재하기 때문에 주소 변환을 2번 해야하는 문제가 발생한다.
- 현재는 대부분 페이징 기법을 사용하고 있다.


<br/>
<br/> 


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source