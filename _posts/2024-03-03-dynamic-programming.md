---
title: "동적 계획법 Dynamic Programming"
author: gyeomji
date: 2024-03-03 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Dynamic Programming, 동적 계획법]
pin: false
math: true
mermaid: true
---

<br/> 
<span style='background-color:#c8d8b4'>한 큰 문제를 여러개의 작은 문제로 쪼개 그 결과를 저장하고 큰 문제를 해결할 때 **재사용**</span>하는 특정한 알고리즘이 아닌 하나의 문제 해결 패러다임<br /> 
문제를 작게 쪼개서 하위 문제를 해결하고 연계적으로 큰 문제를 헤결한다는 점은 분할 정복과 동일하지만, DP는 중복이 일어나는 경우 사용한다.<br /> 

> 💡  <span style="font-size: 15px"> 분할 정복은 1. 분할 2. 정복으로 문제를 나눠서 해결한다.<br /> 　 　분할된 하위 문제가 중복이 일어나지 않는 경우에 사용한다.<br />　 　병합 정렬, 이진 탐색, 거듭제곱 연산</span>

<br/> 
<br/>

## DP 사용 이유

---

- 재귀 방식은 DP와 매우 유사한데, 일반적인 재귀를 단순히 사용 시 동일한 작은 문제들이 여러번 반복되어 비효율적인 계산이 될 수 있다

- 동일한 문제들이 여러번 반복 될 때 DP를 사용하면 효율적으로 문제 해결이 가능하다

<br/>
<br/>

## DP 사용 조건

---

1. 겹치는 부분 문제 Overlapping Subproblems
    - DP는 문제를 **나누고** 그 문제의 결과 값을 **재사용**하여 전체 답을 구한다
    - <span style='background-color:#c8d8b4'>동일한 작은 문제들이 반복해 나타나는 경우 사용 가능</span>

2. 최적 부분 구조 Optimal Substructure
    - <span style='background-color:#c8d8b4'>부분 문제의 최적 결과 값을 사용해 전체 문제의 최적 결과를 낼 수 있는 경우 사용 가능</span>

<br/> 
<br/>

## DP 사용

---

DP는 특정 경우에 사용하는 알고리즘이 아닌 하나의 **방법론**이기 때문에 **다양한 문제 해결에 사용** 가능하다.
<br />

### [ DP 사용 과정 ]

1. DP 사용 조건을 만족하는 문제인지 확인

2. 문제 변수 파악
    - DP는 현재 변수에 따라 결과 값을 찾고 그 값을 재사용한다
    - => 문제 내 <span style='background-color:#c8d8b4'>변수 개수</span>를 알아야 한다

3. 변수 간 관계식 만들기 ( 점화식 )

4. 메모하기(memoization or tabulation)
    - 변수 값에 따른 결과 저장 <span style="color:#9fb584">**Memoization**</span>

5. 기저 상태 파악하기
    - 가장 작은 문제의 상태를 알아야 한다
    - 피보나치 수열 f(0) = 0, f(1) = 1
    - <span style='background-color:#c8d8b4'>해당 기저 문제에 대해 파악 후 미리 저장</span>

6. 구현하기
    - Bottom-Up (Tabulation 방식) - 반복문 사용 
    - Top-Down (Memoization 방식) - 재귀 사용

<br/> 

### [ DP 구현 ]

- Bottom-Up 방식
: 밑에서 부터 계산을 수행 하고 누적시켜 전체 큰 문제를 해결하는 방식이다<br />
dp[0]부터 시작하여 반복문을 통해 점화식으로 결과를 내서 dp[n]까지 그 값을 전이시켜 재활용

> Tabulation
> : 반복을 통해 dp\[0](기저 상태) 부터 하나 하나씩 채우는 과정을 table-filling 이라 하며,<br /> 이 테이블에 저장된 값에 직접 접근하여 재활용하므로 **Tabulation**이라고 한다<br />결과값을 기억하고 재사용한다는 측면에서 Memoization과 크게 다르지 않다.

<br/> 

- Top-Down 방식
 : dp[0]의 기저 상태에서 출발하지 않고 dp[n]의 값을 찾기 위해 위에서 부터 호출을 시작해 dp[0]의 상태까지 내려간 다음 해당 결과 값을 재귀를 통해 전이시켜 재활용<br />- 이전에 해당 문제 계산을 완료한 경우, 메모리에 저장된 값을 꺼내 활용<br /> <span style='background-color:#c8d8b4'>=> 가장 최근 상태 값을 메모해 둠 Memoization</span>

<br/>
<br />

## DP 사용해 해결한 문제

---



<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
