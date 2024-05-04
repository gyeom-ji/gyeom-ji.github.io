---
title: "레드 블랙 트리 Red-Black Tree"
author: gyeomji
date: 2024-05-04 10:00:00 +0900
categories: [Algorithm]
tags: [Algorithm, Red-Black Tree]
pin: false
math: true
mermaid: true
---

<br/> 
노드에 색을 부여하여 트리의 균형을 유지하며 <span style="color:#9fb584">**탐색, 삽입, 삭제 연산의 수행시간이 각각 O(logN)을 넘지 않는**</span> 매우 효율적인 자료구조이다.
<br/> 


## 일반적인 레드 블랙 트리 CLRS

---

: 노드의 컬러가 레드 또는 블랙인 이원 탐색 트리이다. 삽입이나 삭제 수행시 트리의 균형을 유지하기 위해 많은 경우를 고려해야 한다는 단점이 있으며, 이에 따라 프로그램이 복잡하고, 길이도 증가한다.

<br/>

![redBlackTree](/assets/img/redBlackTree.png)

<br/>

### 확장 이진 트리인 레드 블랙 트리의 성질
  
1. 루트와 모든 외부 노드들은 블랙이다.
2. 루트에서 외부 노드로의 경로는 2개의 연속적인 레드 노드를 가질 수 없다.
   1. 빨간 노드의 부모 != 빨간 노드
3. <span style="color:#9fb584">**루트에서 외부 노드로의 모든 경로들에 있는 블랙 노드의 수는 동일하다.**</span>

<br/>

### 포인터에 자식의 컬러를 부여하는 경우에 대한 성질

1. 내부 노드로부터 외부 로드로의 포인터는 블랙이다.
   1. 루트 -> 외부노드로의 경로 상에 있는 마지막 포인터는 블랙
2. 루트에서 외부 노드로의 경로는 2개의 연속적인 레드 포인터를 가질 수 없다.
   1. 레드 포인터 뒤에는 항상 블랙 포인터가 와야 한다.
3. 루트에서 외부 노드로의 모든 경로들에 있는 블랙 포인터의 수는 동일하다.

> 💡 랭크 : 한 노드로부터 외부 노드로의 경로상에 있는 블랙 포인터의 수 = (노드 수 - 1)

<br/>

### 레드 블랙 트리 삽입에서 노드 컬러 지정

- 루트는 무조건 블랙
- 루트가 아닌 경우
  - 블랙으로 지정하면 경로상 블랙 노드의 개수 문제 발생
  - 레드로 지정하는 경우 2개의 연속적인 레드 노드 발생 가능
  - <span style="color:#9fb584">**새 노드는 무조건 레드로 지정하고 불균형을 조정한다.**</span>

<br/>

![redBlackTree](/assets/img/redBlackTree1.png)
![redBlackTree](/assets/img/redBlackTree2.png)
![redBlackTree](/assets/img/redBlackTree3.png)
![redBlackTree](/assets/img/redBlackTree4.png)
![redBlackTree](/assets/img/redBlackTree5.png)

<br/>

### 불균형 타입

: 새 노드 u, u의 부모 pu, u의 조부모 gu, 삼촌 노드 (부모의 형제) c 라고 했을 때 Double Red가 발생한 경우

- 삼촌이 블랙 : Restructuring
- 삼촌이 빨강 : Recoloring

### Restructuring

---

1. 새 노드 u, 부모 노드 pu, 조부모 gu를 오름차순으로 정렬한다.
2. 셋 중 중간값을 부모로 만들고 나머지 둘을 자식으로 만든다.
3. 새로 부모가 된 노드를 검은색으로 만들고 나머지 자식들을 빨간색으로 만든다.
   
- LLb : pu가 gu의 왼쪽 자식이고, u는 pu의 왼쪽 자식이며, gu의 다른 자식이 블랙인 경우

![llb](/assets/img/llb.png)
![llb](/assets/img/llb2.png)

<br/>

- LRb : pu가 gu의 왼쪽 자식이고, u는 pu의 오른쪽 자식이며, gu의 다른 자식이 블랙인 경우

![lrb](/assets/img/lrb.png)
![lrb](/assets/img/lrb2.png)

<br/>

### Recoloring

---

1. 새 노드 u의 부모 노드 pu와 삼촌 c를 검은색으로 바꾸고 조부모 gu를 빨간색으로 바꾼다
   1. 조상이 루트 노드라면 검은색으로 바꾼다
   2. 조상을 빨간색으로 바꿨을 때 다시 Double Red가 발생한다면 다시 Restructuring, Recoloring 진행


- LLr : pu가 gu의 왼쪽 자식이고, u는 pu의 왼쪽 자식이며, gu의 다른 자식이 레드인 경우

![llr](/assets/img/llr.png)
![llr](/assets/img/llr2.png)


<br/>

- LRr : pu가 gu의 왼쪽 자식이고, u는 pu의 오른쪽 자식이며, gu의 다른 자식이 레드인 경우

![lrr](/assets/img/lrr.png)
![lrr](/assets/img/lrr2.png)

<br/>

- 위와 마찬가지로 RRb, RRr, Rlb, Rlr이 있다.

<br/>
<br />

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
