---
title: "관계 데이터 모델"
author: gyeomji
date: 2024-07-20 10:00:00 +0900
categories: [Database]
tags: [Relational Data Model, Key, 무결성 제약조건, 관계대수, 스키마, 인스턴스, 릴레이션]
pin: false
math: true
mermaid: true
---

<br/> 

## 관계 데이터 모델 Relational Data Model

---

- 데이터를 <span style="color:#9fb584">**2차원 테이블 형태인 릴레이션**</span>으로 표현한다.
- 릴레이션에 대한 <span style="color:#9fb584">**제약조건(constraints)**</span>과 관계 연산을 위한 <span style="color:#9fb584">**관계대수(relational algebra)**</span>를 정의한다.

> 💡 릴레이션 Relation : 행과 열로 구성된 테이블

<br/>

### 관계 Relationship

---

#### <span style="color:#9fb584">**릴레이션 내**</span>에서 생성되는 관계

- 릴레이션 내 데이터들의 관계
- 첫 번째 행(1, 축구의 역사, 굿스포츠, 7000)의 경우 4개의 집합에서 각각 원소 한 개씩 선택하여 만들어진 것으로 이 <span style="color:#9fb584">**원소들이 관계 Relationship**</span>을 맺고 있다.

<br/>

![bookRelation](/assets/img/bookRelation.png){: width="400" height="400"}

![bookRelation](/assets/img/bookRelation1.png)

#### <span style="color:#9fb584">**릴레이션 간**</span> 생성되는 관계

- 릴레이션 간의 관계

![bookRelation](/assets/img/bookRelation2.png)

<br/>

### 스키마 Schema (내포 Intension)

---

![bookRelation](/assets/img/bookRelation3.png)


- 데이터베이스에 저장되는 <span style="color:#9fb584">**데이터 구조와 제약조건**</span>을 정하는 것을 의미
  - ex) 데이터 타입 지정, 중복된 값을 허용하지 않는 등의 제약조건 지정
- 릴레이션(테이블)의 이름과 릴레이션에 포함된 모든 속성의 이름으로 정의한다.

#### 스키마의 요소

1. <span style="color:#9fb584">**속성 attribute**</span> : 릴레이션 스키마의 열
   - 사물의 속성
   - ex) 고객 이름, 전화번호, 집 주소, 나이
2. <span style="color:#9fb584">**도메인 domain**</span> : 속성이 가질 수 있는 값의 집합
   - 데이터 타입
   - ex) Int, Bool, String
3. <span style="color:#9fb584">**차수 degree**</span> : 한 릴레이션에서 속성의 전체 개수


#### 스키마의 표현

- 릴레이션 이름(속성1 : 도메인1, 속성2 : 도메인2, 속성3 : 도메인3 ...)
- ex) 도서(도서번호, 도서이름, 출판사, 가격)

<br/>

### 인스턴스 Instance (외연 Extension)

---

- 어떤 특정 시점에 정의된 스키마에 따라 <span style="color:#9fb584">**데이터베이스에 실제로 저장된 값**</span>이다.
- 데이터가 갱신될 때마다 변한다.


#### 인스턴스 요소

1. <span style="color:#9fb584">**투플 Tuple**</span> : 릴레이션의 행
    - n개의 원소로 이루어진 순서가 있는 목록이다.
2. <span style="color:#9fb584">**카디날리티 cardinality**</span> : 한 릴레이션에서 투플의 전체 개수

<br/>

### 릴레이션 특징

---

- <span style="color:#9fb584">**속성은 단일 값(원자 값 atomic value)**</span>을 가진다.
  - 각 속성의 값은 도메인에 정의된 값만 가지며 그 값은 모두 단일 값이여야 한다.
    > 원자 값 : 더이상 나눌 수 없는 값<br/>
    > 도서 이름 : 축구의 역사 O<br/>
    > 도서 이름 : 피겨 교본, 피겨 기초 X (단일 값이 아님 ➡️ 피겨 교본과 피겨 기초를 나눠야함)
- 속성은 서로 다른 이름을 가진다.
  - 속성은 한 릴레이션에서 서로 다른 이름을 가져야 한다.
- 한 속성의 값은 모두 같은 도메인 값을 가진다.
  - 한 속성에 속한 열은 모두 그 속성에서 정의한 도메인 값만 가질 수 있다.
- <span style="color:#9fb584">**속성의 순서는 상관없다.**</span>
  - 속성의 순서가 달라도 릴레이션 스키마는 같다
  - ex) 릴레이션 스키마에서 (이름, 주소) 순으로 속성을 표시하거나 (주소, 이름) 순으로 표시해도 상관 없다.
- <span style="color:#9fb584">**릴레이션 내 중복된 투플은 허용하지 않는다.**</span>
  - 하나의 릴레이션 인스턴스 내에서는 서로 중복된 값을 가질 수 없다.
  - 즉, 모든 투플은 서로 값이 달라야 한다.
- <span style="color:#9fb584">**투플의 순서는 상관없다.**</span>

<br/>

![bookRelation](/assets/img/bookRelation4.png){: width="600" height="600"}

<br/>

## 키

---

- 특정 투플을 <span style="color:#9fb584">**식별할 때 사용하는 속성, 속성의 집합**</span>이다.
- 릴레이션은 투플의 중복을 허용하지 않기 때문에 각각 투플에 포함된 속성 중 하나 이상은 값이 달라야 한다.
- 즉, <span style="color:#9fb584">**키가 되는 속성(속성의 집합)**</span>은 반드시 값이 달라 투플을 서로 구별할 수 있어야 한다.
- <span style="color:#9fb584">**릴레이션 간 관계를 맺는 데도 사용**</span>된다.(외래키)

![relationKey](/assets/img/relationKey.png)

### 1. 슈퍼키

- 투플을 유일하게 식별할 수 있는 하나의 속성, 속성의 집합
- <span style="color:#9fb584">**투플을 유일하게 식별할 수 있는 값이면 모두 슈퍼키가 될 수 있다.**</span>

#### 슈퍼키 예시 (고객 릴레이션)


![relationTableCustomer](/assets/img/relationTableCustomer.png){: width="450" height="450"}

<br/>

- 고객번호 ☑️ : 고객별로 유일한 값이 부여되어 있기 때문에 투플을 식별할 수 있음.
- 이름 : 동명이인이 있을 경우 투플을 유일하게 식별할 수 없음.
- 주민번호 ☑️ : 개인별로 유일한 값이 부여되어 있기 때문에 투플을 식별할 수 있음.
- 주소 : 가족끼리는 같은 정보를 사용하므로 투플을 식별할 수 없음.
- 핸드폰 : 한 사람이 여러 개의 핸드폰을 사용할 수 있고 반대로 핸드폰을 사용하지 않는 사람이 있을 수 있기 때문에 투플을 식별할 수 없음.
- 고객 릴레이션은 고객번호와 주민번호를 포함한 모든 속성의 집합이 슈퍼키가 된다.
  - ex) (주민번호), (주민번호, 이름), (고객번호), (고객번호, 이름, 주소), (고객번호, 이름, 주민번호, 주소, 핸드폰) 등


<br/>

### 2. 후보키

- 투플을 <span style="color:#9fb584">**유일하게 식별할 수 있는 속성의 최소**</span> 집합

#### 후보키 예시 (주문 릴레이션)

![relationTableOrder](/assets/img/relationTableOrder.png){: width="400" height="400"}

- 고객번호 : 한 명의 고객이 여러 권의 도서를 구입할 수 있으므로 후보키가 될 수 없음.고객번호가 1인 박지성 고객은 세 번의 주문 기록이 있으므로 투플을 유일하게 식별할 수 없음.
- 도서번호 : 도서번호가 2인 ‘축구아는 여자’는 두 번의 주문 기록이 있으므로 투플을 유일하게 식별할 수 없음.
- 주문 릴레이션의 후보키는 2개의 속성을 합한 (고객번호, 도서번호)가 된다.
  - 2개 이상의 속성으로 이뤄진 키를 <span style="color:#9fb584">**복합키 Composite Key**</span>라고 한다.

<br/>

### 3. 기본키

- <span style="color:#9fb584">**여러 후보키 중 하나를 선정하여 대표**</span>로 삼는 키
- 후보키가 하나뿐이라면 그 후보키를 기본키로 사용한다.
- 여러 개일 경우 릴레이션의 특성을 반영하여 하나를 선택한다.
- 릴레이션 스키마를 표현할 때 기본키는 <U>밑줄</U>을 그어 표시한다.


#### 기본키 선정 시 고려사항

- 릴레이션 내 투플을 <span style="color:#9fb584">**유일하게 식별할 수 있는 고유한 값을**</span> 가져야 한다.
- <span style="color:#9fb584">**NULL 값은 허용하지 않는다.**</span>
- 키 값의 변동이 일어나지 않아야 한다.
- 최대한 적은 수의 속성을 가진 것이여야 한다.
- 향후 키를 사용할 때 문제 발생 소지가 없어야 한다.

<br/>

### 4. 대체키 

- 대체키(alternate key)는 <span style="color:#9fb584">**기본키로 선정되지 않은 후보키**</span>이다.
- 고객 릴레이션의 경우 고객번호와 주민번호 중 고객번호를 기본키로 정하면 주민번호가 대체키가 된다.

![relationTableCustomer](/assets/img/relationTableCustomer.png){: width="400" height="400"}

- 고객번호 ☑️ : 고객별로 유일한 값이 부여되어 있기 때문에 투플을 식별할 수 있음.
- 주민번호 ☑️ : 개인별로 유일한 값이 부여되어 있기 때문에 투플을 식별할 수 있음.

<br/>

### 5. 대리키

- 기본키가 보안을 필요로 하거나, 여러 개의 속성으로 구성되어 복잡하거나, 마땅한 기본 키가 없을 경우 사용한다.
- 일련번호 같은 가상의 속성을 만들어 사용한다.
- 대리키(surrogate key) 또는 인조키(artificial key)라고 한다.
- 대리키는 DBMS나 관련 소프트웨어에서 임의로 생성하는 값으로 사용자가 직관적 으로 그 값의 의미를 알 수 없다.

#### 대리키(주문번호)를 사용하도록 변경한 주문 릴레이션

![relationTableOrder](/assets/img/relationTableOrderSurrogateKey.png){: width="400" height="400"}

<br/>

### 6. 외래키

- <span style="color:#9fb584">**다른 릴레이션의 기본키를 참조(자신의 값으로 갖는)하는 속성**</span>이다.
- <span style="color:#9fb584">**릴레이션 간의 관계를 표현**</span>한다.

![relationKey](/assets/img/relationKey1.png){: width="600" height="600"}

#### 외래키의 특징

- 다른 릴레이션의 기본키를 참조한다.
- 관계 데이터 모델의 릴레이션 간 관계를 표현한다.
- <span style="color:#9fb584">**참조하고(외래키) 참조되는(기본키) 양쪽 릴레이션의 도메인은 서로 같아야 한다.**</span>
- <span style="color:#9fb584">**참조되는(기본키) 값이 변경되면 참조하는(외래키) 값도 변경된다.**</span>
- NULL, 중복 값이 허용된다.
- <span style="color:#9fb584">**외래키가 기본키의 일부가 될 수 있다.**</span>
- 자기 자신의 기본키를 참조할 수 있다.

![relationKey](/assets/img/relationKey2.png){: width="400" height="400"}


<br/>

## 무결성 제약 조건

---

- 데이터 무결성(integrity)은 데이터베이스에 저장된 데이터의 <span style="color:#9fb584">**일관성(consistency)와 정확성(correctness)을 지키는 것**</span>을 말한다.

### 1. 도메인 무결성 제약조건 (도메인 제약 Domain Constraint)

- 릴레이션 내 투플들이 <span style="color:#9fb584">**각 속성의 도메인에 지정된 값만을 가져야 한다는 조건**</span>이다.
  - 허용되는 값을 제한한다.
- SQL문에서 데이터 형식(type), 널(null/not null), 기본 값(default), 체크(check) 등을 사용하여 지정할 수 있다.

<br/>

### 2. 개체 무결성 제약조건 (기본키 제약 Primary Key Constraint)

- <span style="color:#9fb584">**기본키는 릴레이션 내에서 유일한 값**</span>을 가져야 한다.
- <span style="color:#9fb584">**NULL 값을 가져서는 안된다.**</span>
- 삽입 : 중복된 기본키 값, NULL 값 삽입 금지
- 수정 : 기본키 값이 같거나 NULL으로 수정 금지
- 삭제 : 특별한 확인이 필요하지 않으며 즉시 수행

![primaryKeyConstraint](/assets/img/primaryKeyConstraint.png){: width="600" height="600"}

<br/>

### 3. 참조 무결성 제약조건 (외래키 제약 Foreign Key Constraint)

- 릴레이션 간 참조 관계에 대한 제약 조건이다.
- <span style="color:#9fb584">**자식 릴레이션의 외래키는 부모 릴레이션의 기본키와 도메인이 동일**</span>해야 한다.
- 자식 릴레이션의 값이 변경될 때 부모 릴레이션의 제약을 받는다.
- 두 릴레이션의 연관된 투플들 사이의 <span style="color:#9fb584">**일관성을 유지**</span>하는데 사용된다.
- 자식 릴레이션의 외래키가 부모 릴레이션의 기본키를 참조할 때 참조 무결성 제약조건은 아래의 두 조건 중 하나가 성립되면 만족된다.

#### 참조 무결성 제약 조건

1. 외래키의 값은 부모 릴레이션의 어떤 투플의 기본키 값과 같다.
2. 외래키가 기본키(또는 일부)가 아니면 NULL값을 가질 수 있다.

<br/>

#### 삽입

- 부모 릴레이션(학과) : 투플 삽입이 가능하다.
- 자식 릴레이션(학생) : 참조되는 테이블에 외래키 값이 없을 경우 삽입이 금지된다.
  - 새로운 투플 삽입(수정) 시 부모 투플 내에 있는 값으로 삽입해야 한다.

![foreignKeyConstraint](/assets/img/foreignKeyConstraint.png){: width="500" height="500"}


#### 삭제

- 부모 릴레이션(학과) : 참조하는 테이블(자식)을 같이 삭제할 수 있기 때문에 금지하거나 다른 추가 작업이 필요하다.
- 자식 릴레이션(학생) : 바로 삭제가 가능하다.

##### [ 부모 릴레이션에서 투플을 삭제할 경우 참조 무결성 제약조건의 옵션 ]

![foreignKeyConstraint1](/assets/img/foreignKeyConstraint1.png)

##### [ 부모 릴레이션에서 투플을 삭제할 경우 예시 ]

![foreignKeyConstraint2](/assets/img/foreignKeyConstraint2.png)


#### 수정

- 삭제와 삽입 명령이 연속해서 수행된다.
- 부모 릴레이션의 수정이 일어날 경우 삭제 옵션에 따라 처리된 후 문제가 없으면 다시 삽입 제약 조건에 따라 처리된다.


| |도메인|키|키|
|:------:|:------:|:------:|
| |도메인 무결성 제약조건|개체 무결성 제약조건|참조 무결성 제약조건|
|제약 대상|속성|투플|속성과 투플|
|해당되는 키|-|기본키|외래키|
|NULL 허용|O|X|O|
|릴레이션 내 제약조건의 개수|속성의 개수와 동일|1개|0 ~ 여러 개|
|기타|투플 삽입, 수정 시 제약사항 우선 확인|투플 삽입/수정 시 제약사항 우선 확인|투플 삽입/수정 시 제약사항 우선 확인<br/>부모 릴레이션의 투플 수정/삭제 시 제약사항 우선 확인|


<br/>

## 관계대수

---

- 릴레이션에서 원하는 결과를 얻기 위해 연산을 통해 결과 릴레이션을 찾는 절차를 기술한 언어이다.
- 관계 대수 : 어떤 데이터를 어떻게 찾는지에 대한 처리 절차를 명시하는 <span style="color:#9fb584">**절차적인**</span> 언어이며 DBMS 내부의 처리 언어로 사용된다.
- 관계 해석 : 어떤 데이터를 찾는지만 명시하는 <span style="color:#9fb584">**선언적인**</span> 언어로 관계대수와 함께 관계 DBMS의 SQL의 이론적인 기반을 제공한다.
  > 관계대수와 관계해석은 모두 관계 데이터 모델의 중요한 언어이며 동일한 표현 능력을 가진다. 관계 해석으로 표현된 언어 ➡️ 관계 대수로 표현 가능

<br/>

### 관계대수 연산자

![relationalAlgebra](/assets/img/relationalAlgebra.jpeg)

<br/>


### 관계대수식

---

- 관계대수 연산을 수행하기 위한 식이다.
- <span style="color:#9fb584">**릴레이션과 연산자로 구성되며 결과는 릴레이션으로 반환**</span>된다.
- 단항 연산자 : $연산자_{<조건>} 릴레이션$
- 이항 연산자 : $릴레이션 \ 1 연산자_{<조건>} 릴레이션 \ 2$

#### 관계대수 식 사용 예시

##### 예제 데이터 R1 R2

![relationalAlgebra](/assets/img/relationalAlgebra1.jpeg){: width="300" height="300"}


![relationalAlgebra](/assets/img/relationalAlgebra2.jpeg)

<br/>


### 셀렉션 Selection σ

---

> <br/> $$σ_{<조건>}(R)$$ <br/><br/>

- 하나의 릴레이션을 대상으로 하는 단항 연산자로 원하는 데이터를 <span style="color:#9fb584">**수평적**</span>으로 도출한다.
- 찾고자 하는 투플의 조건을 명시하고 그 조건에 만족하는 투플을 반환한다.

> 마당서점에서 판매하는 도서 중 8,000원 이하인 도서를 검색하시오.

![selection](/assets/img/selection.jpeg){: width="400" height="400"}

> <br/> $σ_{<복합조건>}(R)$ <br/> <br/> 

- ∧(and) ∨(or) ￢(not) 기호를 이용해 복합조건을 표시할 수 있다.
- 가격이 8,000원 이하이고, 도서번호가 3 이상인 도서를 검색할 경우
- $σ_{(가격<=8000∧도서번호>=3)}(도서)$


<br/>

### 프로젝션 Projection π

---

> <br/> $$π_{<속성리스트>}(R)$$ <br/><br/>

- 하나의 릴레이션을 대상으로 하는 단항 연산자로 원하는 데이터를 <span style="color:#9fb584">**수직적**</span>으로 도출한다.

> 신간도서 안내를 위해 고객의 (이름, 주소, 핸드폰)이 적힌 카탈로그 주소록을 만드시오.

![projection](/assets/img/projection.png){: width="400" height="400"}

- 셀렉션은 중복 투플이 존재할 수 없지만, 프로젝션 연산 결과 릴레이션은 중복된 투플이 존재할 수 있다.
- 결과 릴레이션에 <span style="color:#9fb584">**중복이 존재하면 하나만 남기고 제거**</span>된다.

![projection](/assets/img/projection1.png)


<br/>

### 합집합 ∪

---

> <br/> $$R \ ∪ \ S$$ <br/><br/>

- 두 개의 릴레이션을 합하여 하나의 릴레이션을 반환한다.
- 결과 릴레이션에서 <span style="color:#9fb584">**중복된 투플들은 하나만 남기고 제거**</span>된다.
- 이 때 두 개의 릴레이션은 서로 같은 속성 순서와 도메인을 가져야 한다.

> 마당서점에 지점A와 지점B가 있다. 두 지점의 도서는 각 지점에서 관리하며 릴레이션 이름은 각각 도서A, 도서B다. 마당서점의 도서를 하나의 릴레이션으로 보이시오.

`도서A ∪ 도서B`

![union](/assets/img/union.png)

<br/>

### 교집합 ∩

---

> <br/> $$R \ ∩ \ S$$ <br/><br/>

- 합병 가능한 두 릴레이션을 대상으로 두 릴레이션이 공통으로 가지고 있는 투플을 반환한다.

> 마당서점의 두 지점에서 동일하게 보유하고 있는 도서 목록을 보이시오.

`도서A ∩ 도서B` 

![intersection](/assets/img/intersection.png)


<br/>

### 차집합 -

---

> <br/> $$R \ - \ S$$ <br/><br/>

- 첫 번째 릴레이션에는 속하고 두 번째 릴레이션에는 속하지 않는 투플을 반환한다.

> 마당서점 두 지점 중 지점 A에서만 보유하고 있는 도서 목록을 보이시오.

`도서A - 도서B` 

![differenceSet](/assets/img/differenceSet.png)

![differenceSet](/assets/img/differenceSet1.png)

- RESULT1 = 모든 부서 번호를 검색
- RESULT2 = 모든 사원들의 부서 번호를 검색한다.
- RESULT3 = RESULT1 - RESULT2

<br/>

### 카티션 곱 ×

---

> <br/> $$R \ × \ S$$ <br/><br/>

- 두 릴레이션을 연결시켜 하나로 합칠 때 사용한다.
  - RS의 인스턴스는 R과 S 투플들의 모든 가능한 조합의 집합이다.
- 결과 릴레이션은 첫 번째 릴레이션의 오른쪽에 두 번째 릴레이션의 모든 투플을 순서대로 배열하여 반환한다.
- 결과 릴레이션의 차수는 두 릴레이션 차수의 합이며, 카디날리티는 두 릴레이션 카디날리티의 곱이다.

> 고객 릴레이션과 주문 릴레이션의 카티전 프로덕트를 구하시오

`고객 × 주문` 

![cartesion](/assets/img/cartesion.png)

<br/>

### 디비전 Division ÷

---

> <br/> $$R \ ÷ \ S$$ <br/><br/>

- 한 테이블에서 다른 테이블의 모든 값을 가지고 있는 행들을 반환한다.
  - <span style="color:#9fb584">**S의 모든 속성(Y)에 대한 값을 전부 가지고 있는 R의 R(X) 집합을 반환**</span>한다.
- '모든 ~에 대해 ~하는' 형태의 질의에 사용될 수 있다.
- 나누는 테이블의 열의 개수만큼 결과 테이블의 열의 개수가 줄어들게 된다.

![division](/assets/img/division.png)


<br/>

### 조인 ⋈

---

> <br/> $$R \ ⋈_{c} \ S \ = \ σ_{c}(R×S)$$ <br/><br/>

- c : 조인 조건
- 두 릴레이션의 공통 속성을 기준으로 속성 값이 같은 투플을 수평으로 결합하는 연산이다.
- 두 릴레이션의 조인에 참여하는 속성이 서로 동일 한 도메인으로 구성되어야 한다.
- 조인 연산의 결과는 공통 속성의 속성 값이 동일한 투플만을 반환한다.

<br/>

#### 세타 조인 Theta Join θ

> <br/> $$R \ ⋈_{(r \ 조건 \ s)} \ S$$ <br/><br/>

- r은 R의 속성, s는 S의 속성
- 조인에 참여하는 두 릴레이션의 속성 값을 비교하여 조건을 만족하는 투플만 반환한다. 
- 세타조인의 조건은 {=, ≠, ≤, ≥, <, >} 중 하나가 된다.

<br/>

#### 동등 조인 Equi Join =

> <br/> $$R \ ⋈_{(r \ = \ s)} \ S$$ <br/><br/>

- 세타조인에서 = 연산자를 사용한 조인을 말한다.
  - 보통 조인 연산이라고 하면 동등조인을 지칭한다. 
- 조인 석성이 모두 포함되어 있어 <span style="color:#9fb584">**같은 속성이 2번**</span> 나타난다.
- 두 릴레이션에서 공통된 애트리뷰트의 값이 같은 튜플들을 결합하는 것을 말한다.

> 고객과 고객의 주문 사항을 모두 보이시오.<br/>
> $$고객 \ ⋈_{고객.고객번호=주문.고객번호} \ 주문$$


![equiJoin](/assets/img/equiJoin.jpeg)

<br/>

#### 자연 조인 Natural Join N

> <br/> $$R \ ⋈_{N(r, \ s)} \ S$$ <br/><br/>

- 동등조인에서 조인에 참여한 속성이 두 번 나오지 않도록 <span style="color:#9fb584">**두 번째 속성을 제거한 결과**</span>를 반환한다. 

> 고객과 고객의 주문 사항을 모두 보여주되 같은 속성은 한 번만 표시하시오.<br/>
> $$고객 \ ⋈_{N(고객.고객번호=주문.고객번호)} \ 주문$$

![naturalJoin](/assets/img/naturalJoin.jpeg)

<br/>

#### 외부 조인 Outer Join

> <br/> 왼쪽 외부 조인 : $$R \ ⟕_{(r, \ s)} \ S$$<br/>
>  오른쪽 외부 조인 : $$R \ ​⟖_{(r, \ s)} \ S$$ <br/>
>  완전 외부 조인 : $$R \ ⟗_{(r, \ s)} \ S$$ <br/><br/>

- 자연조인 시 조인에 실패한 투플을 모두 보여주되 값이 없는 대응 속성에는 NULL 값을 채워서 반환한다. 
- 모든 속성을 보여주는 기준 릴레이션 위치에 따라 왼쪽(left) 외부조인, 오른쪽(right) 외부조인, 완전 (full) 외부조인으로 나뉜다.

> 마당서점의 고객과 고객의 주문 내역을 보이시오.<br/>
① 고객 기준으로 주문내역이 없는 고객도 모두 보이시오.<br/>
② 주문내역이 없는 고객과, 고객 릴레이션에 고객번호가 없는 주문을 모두 보이시오.<br/> ③ 주문내역 기준으로 고객 릴레이션에 고객번호가 없는 주문도 모두 보이시오.

![outerJoin](/assets/img/outerJoin1.jpeg)

<br/>


#### 세미 조인 Semi Join ⋉

> <br/> $$R \ ⋉_{(r, \ s)} \ S$$ <br/><br/>

- 자연조인을 한 후 두 릴레이션 중 한쪽 릴레이션의 결과만 반환한다.
- 기호에서 <span style="color:#9fb584">**닫힌 쪽 릴레이션의 투플만 반환**</span>한다.

> 마당서점의 고객 중 주문 내역이 있는 고객의 고객 정보를 보이시오.<br/>

![semiJoin](/assets/img/semiJoin.png)

<br/>

### 셀렉션, 프로젝션, 집합연산의 복합 사용 예시

---

> 마당서점의 도서 중 가격이 8,000원 이하인 도서이름과 출판사를 보이시오.<br/>
> ① 마당서점의 지점이 하나인 경우<br/>
> $$π_{도서이름, 출판사}(σ_{가격<=8000 \ }도서)$$<br/>

![selectionEx](/assets/img/selectionEx.png)

> ② 마당서점의 지점이 둘 이상인 경우<br/>
> $$π_{도서이름, 출판사}((σ_{가격<=8000 \ }도서A) \ ∪ \ (σ_{가격<=8000 \ }도서B))$$<br/>

![selectionEx](/assets/img/selectionEx1.png)

<br/>

> 마당서점의 박지성 고객의 거래 내역 중 주문번호, 이름, 가격을 보이시오.<br/>
> ① 카티전 프로덕트를 사용한 연산<br/>
> $$π_{주문.주문번호, 고객.이름, 주문.판매가격}(σ_{고객.고객번호=주문.고객번호 \ AND \ 고객.이름= \ ’ \ 박지성 \ ’ \ }(고객×주문))$$<br/>

![selectionEx](/assets/img/selectionEx2.png)

> ② 조인을 사용한 연산<br/>
> $$π_{주문번호, 이름, 판매가격}(σ_{이름= \ ’ \ 박지성 \ ’ \ }(고객⋈_{고객.고객번호=주문.고객번호}주문))$$<br/>

![selectionEx](/assets/img/selectionEx3.png)

<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source