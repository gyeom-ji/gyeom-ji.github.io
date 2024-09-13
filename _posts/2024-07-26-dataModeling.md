---
title: "데이터 모델링"
author: gyeomji
date: 2024-07-26 10:00:00 +0900
categories: [Database]
tags: [Data Modeling, ER, ERD]
pin: false
math: true
mermaid: true
---

<br/> 

## Data Modeling

---

- 정보 시스템을 위한 Data Model을 생성하는 프로세스(일련의 행위)이다.
    > Data Model : 데이터 구조, 연산, 제약조건
    - 현실 세계의 데이터를 컴퓨터 세계의 데이터베이스로 옮기는 변환 과정
- 개념적 모델링 : 현실 세계에서 중요 데이터를 추출하여 개념 세계로 옮기는 작업 (추상화, 일반화)
    > 개념 Concept : 어떤 사물, 현상에 대한 보편적 관념<br/>
    > 추상화 Abstraction : 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념, 기능을 간추리는 것<br/>
    > 일반화 Generalization : 여러 객체로부터 공통된 점을 추출하여 새로운 객체를 만들어 내는 것 (남학생, 여학생 ➡️ 학생)
- 논리적 모델링 : 데이터베이스에 저장할 구조를 결정하고, 해당 구조로 개념 세계의 데이터를 표현하는 작업
- 일반적으로 개념적 모델링과 논리적 모델링을 합쳐 데이터 모델링이라 한다.
- 데이터 모델링은 <span style="color:#9fb584">**데이터베이스 설계의 핵심 과정**</span>이다.
  - 데이터를 추상화한 데이터 모델을 바탕으로 데이터베이스의 골격을 이해하고, SQL문을 기능/성능적인 측면에서 효율적으로 작성할 수 있기 때문이다.


![dataModeling](/assets/img/dataModeling.png)

<br/>

### 데이터베이스 생명주기 Database Life Cycle

---

![databaseLifeCycle](/assets/img/databaseLifeCycle.png)

1. 요구사항 수집 및 분석
    - 사용자들의 요구사항을 듣고 분석하여 데이터베이스 구축 범위를 정한다.
2. 설계<br/>
    ① 개념적 설계 : 분석된 요구사항을 기초로 주요 개념과 업무 프로세스등을 식별한다.<br/>
    ② 논리적 설계 : 사용하는 DBMS의 종류에 맞게 변환한다.<br/>
    ③ 물리적 설계 : 데이터베이스 스키마를 도출한다.<br/>
3. 구현
    - 설계 단계에서 생성한 스키마를 실제 DBMS에 적용하여 테이블 및 관련 객체(뷰, 인덱스)를 만든다.
4. 운영
    - 구현된 데이터베이스를 기반으로 소프트웨어를 구축하여 서비스를 제공한다.
5. 감시 및 개선
    - 데이터베이스 운영에 따른 시스템의 문제를 관찰하고 데이터베이스 자체의 문제점을 파악하여 개선한다.


<br/>

### 데이터 모델링 과정

---

![dataModeling](/assets/img/dataModeling2.jpeg)

### 개념적 모델링 Conceptual Modeling

- 요구사항을 수집하고 분석한 결과를 토대로 업무의 핵심적인 개념을 구분하고 전체적인 뼈대를 만드는 과정이다.
- 개체 entity를 추출하고 각 개체들 간 관계를 정의하여 ER 다이어그램(ERD, Entity Relationship Diagram)을 만드는 과정까지를 말한다.

![conceptualModeling](/assets/img/conceptualModeling.png)

<br/>

### 논리적 모델링 Logical Modeling

- 개념적 모델링에서 만든 ER다이어그램을 사용하려는 DBMS에 맞게 매핑하여 실제 데이터베이스로 구현하기 위한 모델을 만드는 과정이다.

![logicalModeling](/assets/img/logicalModeling.png)

#### [ 논리적 모델링 과정 ]

1. 개념적 모델링에서 추출하지 않았던 상세 속성들을 모두 추출
2. 정규화 수행
3. 데이터 표준화 수행

<br/>

### 물리적 모델링 Physical Modeling

- 작성된 논리적 모델을 실제 컴퓨터 저장 장치에 저장하기 위한 물리적 구조를 정의하고 구현하는 과정이다.
- DBMS의 특성에 맞게 저장 구조를 정의해야 데이터베이스가 최적의 성능을 낼 수 있다.

![physicalModeling](/assets/img/physicalModeling.png)

#### [ 물리적 모델링 시 트랜잭션, 저장 공간 설계 측면에서 고려할 사항 ]

1. 응답시간을 최소화해야 한다.
2. 얼마나 많은 트랜잭션을 동시에 발생시킬 수 있는지 검토해야 한다.
3. 데이터가 저장될 공간을 효율적으로 배치해야 한다.

<br/>



## ER(Entity Relationship) 모델

---

- 세상의 사물을 개체 entity와 개체 간의 관계로 표현한다.
- 하나의 엔티티는 사람, 장소, 사물, 사건 등과 같이 고유하게 식별이 가능한 실세계의 독립적인 객체이다.
  - 생각이나 개념과 같이 추상적인 것도 있다.
- 개체의 특성을 나타내는 속성 attribute에 의해 식별된다. 
- 개체끼리 서로 관계를 가진다.

<br/>

### ERD

- ER 모델은 개체와 개체 간 관계를 표준화된 ER 다이어그램으로 나타낸다.

![erModel](/assets/img/erModel.png)

<br/>

### 개체와 개체 타입

- 개체란 사람, 사물, 장소, 개념, 사건과 같이 유무형의 정보를 가지고 있는 독립적인 실체이다.
- 데이터베이스에서 주로 다루는 개체는 낱개로 구성된 것, 낱개가 각각 데이터 값을 가지는 것, 데이터 값이 변하는 것 등이 있다.
- 비슷한 속성의 개체 타입을 구성하며 개체 집합으로도 묶인다.

![erModel](/assets/img/erModel1.png)

<br/>

#### 개체 타입의 ERD 표현법

- ERD에서 개체 타입은 직사각형으로 나타낸다.
- 개체 타입 유형은 강한 개체와 약한 개체가 있다.
- 강한 개체 타입 (정규 개체 타입)
    - 독자적으로 존재하며 개체 타입 내에서 자신의 키 애트리뷰트를 사용하여 고유하게 엔티티들을 식별할 수 있는 엔티티 타입이다.
- 약한 엔티티 타입
    - 키를 형성하기에 충분한 애트리뷰트들을 갖지 못한 엔티티 타입이다.
    - 독자적으로 존재할 수 없고 반드시 상위 개체 타입을 가진다.
    - 상위 엔티티 타입의 키 애트리뷰트를 결합해야만 고유하게 약한 엔티티 타입의 엔티티들을 식별할 수 있다.
    - 상위 개체 타입의 키와 결합하여 개체를 고유하게 식별하는 속성을 식별자(discriminator) 혹은 부분키(partial key)라고 한다.

![erModel](/assets/img/erModel2.png)

![erModel](/assets/img/erModel19.png)

<br/>

### 속성 Attribute

- 개체가 가진 성질이다.

![erModel](/assets/img/erModel3.png)

<br/>

#### 속성의 ERD 표현법

- 속성은 기본적으로 타원으로 표현한다.
- 개체 타입을 나타내는 직사각형과 실선으로 연결된다.
- 속성의 이름은 타원의 중앙에 표기한다.
- 속성이 개체를 유일하게 식별할 수 있는 키일 경우 속성 이름에 밑줄을 긋는다.

![erModel](/assets/img/erModel4.png)

<br/>

#### 속성의 유형

![erModel](/assets/img/erModel5.png)

<br/>

### 관계와 관계 타입

- 관계 Relationship : 개체 사이의 연관성을 나타내는 개념이다.
- 관계 타입 : 개체 타입과 개체 타입 간 연결 가능한 관계를 정의한 것이다.
- 관계 집합 : 관계로 연결된 집합을 의미한다.

![erModel](/assets/img/erModel6.png)

<br/>

#### 관계 타입의 ERD 표현법

![erModel](/assets/img/erModel7.png)

<br/>

#### 관계 타입의 유형

- 차수에 따른 유형

![erModel](/assets/img/erModel8.png)

- 관계 대응수(cardinality)에 따른 유형
  - 두 개체 타입의 관계에 실제로 참여하는 개별 개체 수
  - 한 엔티티가 참여할 수 있는 관계의 수

![erModel](/assets/img/erModel9.png)

- 일대일(1:1) 관계
    - 좌측 개체 타입에 포함된 개체가 우측 개체 타입에 포함된 개체와 일대일로 대응하는 관계

![erModel](/assets/img/erModel10.png)

- 일대다(1:N), 다대일(N:1) 관계
    - 실제 일상생활에서 가장 많이 볼 수 있는 관계
    - 한쪽 개체 타입의 개체 하나가 다른 쪽 개체 타입의 여러 개체와 관계를 맺는다.

![erModel](/assets/img/erModel11.png)

- 다대다(N:M) 관계
    - 각 개체 타입의 개체들이 서로 임의의 개수의 개체들과 서로 복합적인 관계를 맺고 있는 관계

![erModel](/assets/img/erModel12.png)

- 관계 대응수의 최솟값과 최댓값
    - 관계 대응수 1:1, 1:N, M:N에서 1, N, M은 각 개체가 관계에 참여하는 최댓값을 의미한다.
    - 관계에 참여하는 개체의 최솟값을 표시하지 않는다는 단점을 보완하기 위해 다이어그램에는 대응수 외에 최솟값과 최댓값을 관계실선 위에 (최솟값, 최댓값)으로 표기한다.

![erModel](/assets/img/erModel14.png)
![erModel](/assets/img/erModel15.png)


<br/>

#### ISA 관계

- 상위 개체 타입의 특성에 따라 하위 개체 타입이 결정되는 형태이다.
  
![erModel](/assets/img/erModel16.png)

![erModel](/assets/img/erModel17.png)

<br/>

#### 참여 제약 조건

- 개체 집합 내 모든 개체가 관계에 참여하는지 유무에 따라 전체 참여와 부분 참여로 구분한다.
- 전체 참여는 개체 집합의 모든 개체가, 부분 참여는 일부만 참여한다.
  

![erModel](/assets/img/erModel18.png)

<br/>

#### IE 표기법(Information Engineering 표기법)

- 개체 타입과 속성은 직사각형으로 표현한다.
  
![erModel](/assets/img/erModel21.png)

- 관계는 실선 혹은 점선으로 표기한다.

![erModel](/assets/img/erModel20.png)

<br/>

### ER 모델을 관계 데이터 모델로 매핑

---

- 완성된 ER 모델은 실제 데이터베이스로 구축하기 위해 논리적 모델링 단계를 거치는데, 이 단계에서 사상(mapping)이 이루어진다.

![erModel](/assets/img/erModel22.png)

<br/>

#### 개체 타입의 사상 (1 ~ 2단계)

##### [1단계] 강한(정규) 개체 타입 

- 정규 개체 타입 E의 경우 대응하는 릴레이션 R을 생성한다.

##### [2단계] 약한 개체 타입 

- 약한 개체 타입에서 생성된 릴레이션은 자신의 키와 함께 강한 개체 타입의 키를 외래키로 사상하여 자신의 기본키를 구성한다.

![entityMapping](/assets/img/entityMapping.png)

<br/>

#### 관계 타입의 사상 (3 ~ 6단계)

![entityMapping](/assets/img/entityMapping1.png)

- `방법 1` 오른쪽 개체 타입 E2를 기준으로 관계 R을 표현한다. 
  - E1(<U>KA1</U>, A2)
  - E2(<U>KA2</U>, A4, KA1)
- `방법 2` 왼쪽 개체 타입 E1을 기준으로 관계 R을 표현한다.
  - E1(<U>KA1</U>, A2, KA2)
  - E2(<U>KA2</U>, A4)
- `방법 3` 단일 릴레이션 ER로 모두 통합하여 관계 R을 표현한다.
  - ER(<U>KA1</U>, A2, <U>KA2</U>, A4)
- `방법 4` 개체 타입 E1, E2와 관계 타입 R을 모두 독립된 릴레이션으로 표현한다.
  - E1(<U>KA1</U>, A2) 
  - R(<U>KA1</U>, <U>KA2</U>) 
  - E2(<U>KA2</U>, A4)

<br/>

##### [3단계] 이진 1:1 관계 타입

- 이진 1:1 관계 타입의 경우 `방법 1` ~ `방법 4`까지 모든 유형으로 사상이 가능하다.
- 개체가 가진 정보 유형에 따라 판단한다.

![entityMapping](/assets/img/entityMapping2.png)


##### [4단계] 이진 1:N 관계 타입

- 이진 1:N 관계 타입의 경우 N의 위치에 따라 `방법 1` 또는 `방법 2`의 유형으로 사상된다.

![entityMapping](/assets/img/entityMapping3.png)


##### [5단계] 이진 M:N 관계 타입

- 이진 M:N 관계 타입은 `방법 4`의 유형으로 사상된다.

![entityMapping](/assets/img/entityMapping4.png)


##### [6단계] N진 관계 타입

- ER 모델의 차수가 3 이상인 다진 관계 타입의 경우 `방법 4`의 유형으로 사상된다.

![entityMapping](/assets/img/entityMapping5.png)

<br/>

#### 다중값 속성의 사상 (7단계)

- 다중값 속성의 개수에 따른 사상 방법

![entityMapping](/assets/img/entityMapping6.png)

##### [7단계] 다중값 속성

- 속성의 개수를 알 수 없는 경우 `방법 1`을, 속성의 개수가 제한적으로 정해지는 경우의 `방법 2`를 사용한다.

![entityMapping](/assets/img/entityMapping7.png)

<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source