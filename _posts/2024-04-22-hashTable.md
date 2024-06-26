---
title: "Hash Table"
author: gyeomji
date: 2024-04-22 10:00:00 +0900
categories: [Data Structure]
tags: [Data Structure, Hash Table]
pin: false
math: true
mermaid: true
---

<br/> 
Hash Table은 <span style='background-color:#c8d8b4'>(Key, Value)로 데이터를 저장하는 자료구조</span>로, 빠르게 데이터를 검색할 수 있다. Key값에 해시 함수를 적용해 <span style="color:#9fb584">**배열(버킷)**</span>의 인덱스로 이용한다. 이러한 해싱 구조로 데이터를 저장할 경우 Key값으로 테이터를 찾을 때 해시 함수를 한 번만 수행하면 되기 때문에  <span style="color:#9fb584">**탐색, 삽입, 삭제 연산에 평균 O(1)의 시간**</span>이 소요된다. swift의 Dictionary가 내부적으로 해시테이블을 사용한다.

><span style="font-size: 15px"> - 해싱 Hashing : 간단한 함수를 사용해 key를 변환한 값을 인덱스로 이용해 항목을 저장한다.<br />- 해시 함수 : 해싱에 사용되는 함수<br />- 해시 값, 해시 주소 : 해시함수가 계산한 값<br />- 해시 테이블 : 항목이 해시값에 따라 저장되는 배열</span>

<br/> 

## 해시 함수 Hash Function

---

: 가장 이상적인 해시함수는 키들을 <span style="color:#9fb584">**균등하게**</span> 해시테이블의 인덱스로 변환하는 함수이다.

 - 균등하게 변환한다는 것은 키들을 해시테이블에 <span style="color:#9fb584">**랜덤하게 흩어지도록 저장**</span>하는 것을 뜻한다.
 - 아무리 균등한 결과를 보장하는 해시함수여도 <span style="color:#9fb584">**함수 계산 자체에 긴 시간이 소요될 경우 해싱의 장점인 연산의 신속성을 상실**</span>하므로 가치를 잃는다

### 대표적인 해시 함수

: 키의 모든 자리의 숫자들이 함수 계산에 참여함으로써 계산 결과에는 원래 키에 부여된 의미나 특성을 찾을 수 없다. 계산 결과에서 해시테이블의 크기 M에 따라 특정부분만 해시값으로 활용한다.

 - 중간 제곱 함수 Mid-square Function
    : 키를 제곱한 후, 적절한 크기의 중간부분을 해시값으로 사용한다.
 - 접기 함수 Folding Function
    : 큰 자릿수를 가지는 십진수를 키로 사용하는 경우, 몇 자리씩 일정하게 끊어 만든 숫자들의 합을 이용해 해시값 생성
    - 123456789012 -> 1234 + 5678 + 9012 = 15924를 계산한 후 해시 테이블의 크기에 맞게 n자리 수만을 해시값으로 사용한다.
 - 곱셈 합수 Multiplicative Function
    : 1보다 작은 실수를 키에 곱해 얻은 수의 소수 부분을 테이블 크기 M과 곱하여 나온 값의 정수부분을 해시값으로 사용한다.
 - 나눗셈 함수 Division Function
    : 가장 널리 사용되는 함수로, 키를 소수 M으로 나눈 뒤, 나머지를 해시값으로 사용한다.
    - h(key) = key % M
    - 소수를 사용하는 이유는 나눗셈 연산을 했을 때, 소수가 키들을 균등하게 인덱스로 변환시키는 성질을 가지기 때문이다.

<br />

## 해시 테이블 충돌 Hash Table Conllision

---

: 아무리 우수한 해시함수를 사용해도 2개 이상의 항목을 해시테이블의 동일한 원소에 저장해야 하는 경우가 발생하고, <span style="color:#9fb584">**서로 다른 키들이 동일한 해시값을 가질 때 충돌이 발생**</span>한다. 충돌 해결 방법은 개방 주소 방식과 폐쇄 주소 방식으로 구분한다.

### 개방 주소 방식 Open Addressing

---

: 해시테이블 전체를 열린 공간으로 가정하고 <span style="color:#9fb584">**충돌된 키를 일정한 방식에 따라서 찾아낸 empty 원소에 저장**</span>한다. 대표적인 개방 주소 방식으로는 선형조사, 이차조사, 랜덤조사, 이중해싱이 있다. <span style="color:#9fb584">**적은 양의 데이터에 효과적**</span>이지만, 해시 테이블에 비어있는 원소가 적을 경우, <span style="color:#9fb584">**재해시**</span>가 필요하다.

#### 1. 선형 조사 Linear Probing
   
: 충돌이 일어난 해시 인덱스부터 <span style="color:#9fb584">**순차적으로 검색해 처음 발견한 empty 원소에 충돌이 일어난 키를 저장**</span>한다. swift의 Dictionary에서 충돌 해결 방법으로 사용한다.

- h(key) = i일때, 해시테이블 a[i], a[i + 1], a[i + 2]...a[i + j]를 차례로 검색하여 첫번째로 찾은 empty원소에 key를 저장한다.
- 해시테이블은 1차원 배열이므로 i + j가 M일 경우 a[0]을 검색한다.
- 1차 군집화 Primary Clustering 문제를 가진다.
- <span style="color:#9fb584">**순차탐색으로 empty 원소를 찾아 충돌된 키를 저장하므로 해시테이블의 키들이 빈틈없이 뭉치는 현상이 발생**</span>한다.
- 군집화는 탐색, 삽입, 삭제 연산 시 군집된 키들을 순차적으로 방문해야하는 문제점을 야기시킨다.
- 군집화는 해시테이블에 <span style="color:#9fb584">**empty 원소의 수가 적을수록 심화되며, 이는 해시 성능을 극단적으로 저하시킨다.**</span>


#### 2. 이차 조사 Quadratic Probing

: 선형조사와 근복적으로 동일한 충돌 해결 방법으로, 해시의 <span style="color:#9fb584">**저장순서 폭을 제곱으로 저장**</span>한다. 처음 충돌이 발생한 경우 1만큼 이동하고, 그 다음 충돌이 발생하면 2^2, 3^2 만큼 옮기는 방식이다.

- 이웃하는 빈 곳이 채워져 만들어지는 1차 군집화 문제를 해결한다.
- 하지만, 2차 군집화 Secondary Clustering 문제를 가진다.
- 같은 해시값을 가지는 서로 다른 키들이 똑같은 점프 시퀀스를 따라 empty 원소를 찾아 저장하기 때문에 <span style="color:#9fb584">**또 다른 형태의 군집화를 야기**</span>한다.
- 점프 크기가 제곱만큼 커지기 때문에 배열에 empty원소가 있어도 건너뛰어 빈공간 탐색에 실패하는 경우도 존재한다.


#### 3. 랜덤 조사 Random Probing

: 선형조사와 이차조사의 규칙적인 점프 시퀀스와 달리 <span style="color:#9fb584">**점프 시퀀스를 무작위화**</span> 하여 빈 원소를 찾는다.

- 의사 난수 생성기를 사용해 다음 위치를 찾는다.
- 하지만, 랜덤 조사 방식도 동의어들이 <span style="color:#9fb584">**똑같은 점프 시퀀스에 따라 empty 원소를 찾아 키를 저장하기 때문에 3차 군집화가 발생**</span>한다.


#### 4. 이중 해싱 Double Hashing

: 이중해싱은 <span style="color:#9fb584">**2개의 해시함수를 사용**</span>한다.

- 하나는 기본적인 해시함수로 키를 해시테이블의 인덱스로 변환한다.
- 두번째 함수는 충돌 발생 시 다음 위치를 위한 점프 크기를 정한다.
- 이중해싱은 동이어들이 서로 다른 두번째 해시함수를 가지기 때문에 점프 시퀀스가 일정하지 않다.
- 이중 해싱은 <span style="color:#9fb584">**모든 군집화 문제를 해결**</span>하지만, 해싱을 2번 하기 때문에 연산량이 많다.

<br />

### 폐쇄 주소 방식 Closed Addressing

---

: <span style="color:#9fb584">**키에 대한 해시값에 대응되는 곳에만 키를 저장하는 방식**</span>으로, 충돌이 발생한 키들은 한 위치에 모여서 저장된다. 폐쇄 주소 방식을 구현하는 가장 대표적인 방법은 <span style="color:#9fb584">**체이닝 Chaining 기법**</span>이다.


#### Chaning

: 충돌이 일어날 경우, <span style="color:#9fb584">**연결 리스트**</span>를 이용하여 데이터를 뒤에 추가로 연결 시켜 저장한다. 군집화 현상이 발생하지 않으며, 구현이 간결하여 가장 많이 활용되는 해시방법이다. 개방 주소법에 비해 테이블의 확장을 늦츨 수 있지만, <span style="color:#9fb584">**데이터의 수가 많아지면 연결리스트들의 길이가 너무 길어져 해시 성능이 매우 저하**</span>된다. (최악의 경우 탐색에 O(n)) 또한, 개방 주소법에 비해 데이터 양이 적을 경우 연결 리스트 자체의 <span style="color:#9fb584">**메모리 오버헤드**</span>가 있다. (다음 노드를 가리키는 포인터 공간) 

<br />

## 재해시 Rehash

- 어떤 해싱방법도 해시테이블에 <span style="color:#9fb584">**빈 공간이 적으면, 삽입에 실패하거나 해시 성능이 급격하게 저하되는 현상**</span>을 피할 수 없다.
- 이때, 해시테이블을 확장시키고 새로운 해시함수를 사용하여 <span style="color:#9fb584">**모든 키들을 새로운 해시테이블에 다시 저장하는 재해시**</span>가 필요하다
  - 모든 키들을 다시 저장해야 하므로 <span style="color:#9fb584">**O(N)**</span> 시간 소요
- 재해시 수행 여부는 적재율에 따라 결정한다.
  - 적재율 = (테이블에 저장된 키의 수 N) / (테이블 크기 M)
  - 일반적으로 적재율 > 0.75가 되면 해시테이블 크기를 2배로 늘리고, 적재율 < 0.25 가 되면 해시테이블을 반으로 줄인다.

<br />

## 동적 해싱 Dynamic Hashing

: 대용량의 데이터베이스를 위한 해시방법으로 재해싱을 수행하지 않고 <span style="color:#9fb584">**동적**</span>으로 해시테이블의 크기를 조절한다. 대표적으로 확장해싱과 선형해싱이 있다.


### 확장 해싱 Extendible Hashing

: 디렉토리를 메인 메모리에 저장하고, 데이터는 디스크 블록 크기의 버킷(키 저장하는 곳) 단위로 저장한다.

- 버킷에 오버플로우 발생 시 새 버킷을 만들어 나눠 저장한다.
- 이때 이 버킷들을 가리키던 디렉토리는 2배로 확장한다.

<br />

### 선형 해싱 Linear Hashing

: 디렉토리 없이 삽입할 때 버킷을 순서대로 추가하는 방식이다.

- 추가되는 버킷은 삽입되는 키가 저장되는 버킷과 무관하게 순차적으로 추가한다.
- 삽입되는 버킷에 저장공간이 없으면 오버플로우 체인에 새 키를 삽입한다.
- 체인은 단순연결리스트로 오버플로우된 키를 임시로 저장하고, 버킷이 추가되면 오버플로우 체인의 키들을 버킷으로 이동한다.
  

<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
