---
title: "Struct & Class"
author: gyeomji
date: 2024-04-17 10:00:00 +0900
categories: [Swift]
tags: [Swift, Struct, Class]
pin: false
math: true
mermaid: true
---

Struct와 Class는 <span style="color:#9fb584">**OOP(Object-Oriented Programming)**</span>를 위한 필수 요소로, 프로그램의 코드를 추상화하기 위해 사용한다. 프로퍼티와 메서드를 하나의 패키지에 구조화시켜 사용할 수 있으며, 이를 새로운 사용자 정의 타입으로 추상화하는 개념이다.

>💡  <span style="font-size: 15px"> instance : 클래스, 구조체에서 생성된 **객체**<br />
> 　 　property : 클래스, 구조체의 객체에 들어있는 **정보, 값**<br />
> 　 　method : 클래스, 구조체 객체에 들어있는 **함수**</span>

## Struct와 Class의 성능에 영향을 주는 요소

---

- Allocation : 인스턴스 생성 시 stack과 heap 중 어디에 **할당** 되는지<br />
- Reference Counting : 인스턴스를 통해 **레퍼런스 카운트가 몇개** 발생하는지<br />
- Method Dispatch : 인스턴스에서 메소드를 호출했을 때, **메소드 디스패치가 정적인지 동적인지**


## Value Type과  Reference Type

---

- <span style="color:#9fb584">**Value Type**</span>은 인스턴트를 생성하여 다른 인스턴스에 할당하면, <span style="color:#9fb584">**전체 값이 복사**</span>된다.
    - function, closure을 제외한 swift의 대부분 타입은 value type
    - <span style="color:#9fb584">**struct**</span>와 <span style="color:#9fb584">**enum**</span> 으로 생성한 모든 타입은 value type
    - Int, Float, Double, Bool, String, Array, Dictionary, Set, Tuple 등 (struct로 구현)
    <br/>
<br />
- <span style="color:#9fb584">**Reference Type**</span>은 인스턴트를 생성하여 다른 인스턴스에 할당하면, <span style="color:#9fb584">**주소값이 복사**</span>된다.
    - 여러 인스턴스가 같은 주소값을 가리키기 때문에 코드의 다른 부분에서 데이터 변경이 가능하다.
    - <span style="color:#9fb584">**class**</span>로 생성한 모든 타입은 reference type
    <br/>
<br />

[ COW (Copy-on-write) ]

: Collection 타입의 Array, Dictionary, Set과 원시 타입의 Int, Double, String은 기본적으로 value type이지만, reference type이 섞여있다.
    - array, string은 많은 값을 내부에 포함하고 있기 때문에 복사 할때마다 전체를 복사하면 시간, 공간의 부담이 커진다.
    <center>⬇️</center>
    <br/>
    swift 컴파일러는 <span style="color:#9fb584">**Copy-on-write**</span> 방법 사용 (<span style="color:#9fb584">**reference type의 효율성과 value type의 불변성**</span>)<br/>
    - A에 있는 컬렉션 값을 새로운 변수 B에 할당할때, 복사본을 만들지 않고 reference type처럼 참조만 공유 -> 컬렉션 내의 복사 부담이 줄어듬
    - B 값에 변경 발생 시, 새로운 복사본을 B에 만들고 값 변경 
    - 할당에 비해 변경이 적게 일어나기 때문에 복사 비용을 줄일 수 있고, 변경이 발생해도 원래 값은 변하지 않기 때문에 불변성 유지
    - 값이 실제로 복사되는 첫번째 수정 작업 시 시간이 조금 더 걸리고 약간의 오버헤드가 발생할 수 있다.

<br />

## Memory Allocation

---

![memoryStructure](/assets/img/memoryStructure.png)

프로그램이 실행되면 OS는 메모리(RAM)에 해당 프로그램을 위한 공간을 할당해준다. 메모리에는 Code, Data, Stack, Heap 4가지 영역이 존재한다.

### Code 영역

- <span style="color:#9fb584">**Code**</span> 영역은 앱이 <span style="color:#9fb584">**실행해야 할 프로그램의 코드를 기계어 형태로 저장**</span>한다.
- CPU는 코드 영역에 저장된 명령을 하나씩 가져가서 처리한다.
- 프로그램이 시작되고 종료될 때까지 메모리에 계속 남아있는다.
- <span style="color:#9fb584">**컴파일 타임에 결정되고, 중간에 코드가 변경되지 않도록 Read-Only**</span> 형태로 저장된다.
- 시스템 콜이나 인터럽트 처리 루틴 코드
  > `인터럽트` : 프로그램 실행 중 예기치 않은 상황이나 필요에 의해 실행중인 프로세스를 중단하고 OS에게 CPU 제어권을 넘겨 처리해야 하는 작업들<br/>
  > `시스템 콜` : 일반 프로세스가 OS의 도움이 필요한 특권 명령(ex. I/O 요청)을 요청할 때 일으키는 인터럽트
- CPU, 메모리 등의 자원 관리를 위한 코드
- 편리한 인터페이스 제공을 위한 코드
  
<br/>

### Data 영역

- <span style="color:#9fb584">**Data**</span> 영역은 프로그램의 <span style="color:#9fb584">**전역 변수, 정적 변수(static)를 저장**</span>한다.
- 어디서든 접근 가능하며, 프로세스가 끝날때까지 유지된다.
- 프로그램 <span style="color:#9fb584">**시작과 동시에 할당되고, 프로그램이 종료 되어야 메모리가 해제**</span>된다.
  
> <span style="color:#9fb584">**Swift에서 static은 기본 동작이 lazy**</span>이다. 모든 전역(global) 상수, 변수는 항상 lazy로 생성된다. Lazy Stored Property의 동작방식과 같지만, lazy 키워드가 필요 없다. 지역(local) 상수, 변수는 절대 lazy로 생성되지 않는다. static은 항상 전역으로 선언되기 때문에 lazy로 생성된다. 즉 <span style="color:#9fb584">**Swift에서 static변수는 Lazy Stored Property와 같이 초기값이 그 변수가 처음 사용될 때 계산**</span>된다. Lazy Stored Property와의 차이점은, 전역 변수는 <span style="color:#9fb584">**프로그램 시작과 동시에 메모리에 올라가고, 실 사용될 때 값이 초기화**</span>되지만, lazy property는 메모리에도 올라가지 않고, 실제 사용될 때 메모리에 올라가고 초기화된다.

- 실행 도중 변수 값이 변경될 수 있어 <span style="color:#9fb584">**Read-Write**</span>로 저장된다.
- Data 영역은 BSS 영역과 Data(GVAR) 영역으로 나눠진다.
- 초기화 된 데이터는 Data(GVAR) 영역에 저장되고, 초기화되지 않은 데이터는 BSS 영역에 저장된다.
- BSS에는 초기값을 설정하지 않은 전역 변수와 정적 변수를 위한 영역으로 0으로 자동 초기화를 해준다.

> BSS 영역과 Data(GVAR) 영역을 구분하는 이유 : 초기화 된 데이터는 초기 값을 저장해야 하므로 Data(GVAR) 영역에 저장되에 ROM에 저장된다. 하지만 초기화 되지 않은 데이터까지 ROM에 저장하면, 크기가 큰 ROM이 필요하고, 이는 비효율적이므로 초기화 되지 않은 데이터는 RAM에 저장하여 영역을 구분한다.

<br/>

### Stack 영역

- <span style="color:#9fb584">**Stack**</span>은 일시적인 데이터를 저장하며, 하나의 명령어(pop, push)로 메모리를 할당, 해제할 수 있기 때문에 <span style="color:#9fb584">**매우 빠르고 효율적이다.**</span>
- 지역변수, 파라미터를 저장한다.
- 높은 주소(위) -> 낮은 주소(아래)로 할당된다.
- 스택 영역의 데이터는 <span style="color:#9fb584">**CPU가 관리하고 최적화하기 때문에 메모리에 빈 공간이 발생하지 않는다.**</span>
  - 함수 호출시 스택에 해당 함수에 해당하는 공간이 생기고, 함수 실행이 끝나면 사라진다.
- <span style="color:#9fb584">**컴파일 단계에서 생성과 해제를 알 수 있는 value type 인스턴스가 저장된다.**</span>
  - <span style="color:#9fb584">**컴파일 시 크기가 결정**</span>된다.
- reference type 중에서도 크기가 고정되어있거나, 언제 지워야할지 컴파일러가 미리 예측 가능한 경우 가급적 stack을 사용해 성능을 향상시킨다.

<br/>

### Heap 영역

- <span style="color:#9fb584">**Heap**</span> 영역은 데이터를 지우기 전까지 반영구적으로 지속되며,  <span style="color:#9fb584">**스택에 비해서 속도가 느리다.**</span>
- 동적으로 할당되는 메모리 공간이며, 개발자가 할당 및 해제를 해줘야한다.
  - 유연성이 높다.
- 낮은 주소(아래) -> 높은 주소(위)로 할당된다.
- <span style="color:#9fb584">**swift는 ARC (Automatic Reference Counting)로 메모리를 추적하고 관리한다.**</span>
  - 컴파일 시 컴파일러가 메모리에서 해제하는 코드를 적절히 넣어준다.
- ARC는 힙 영역에서 사용하지 않은 블록을 찾아 메모리 할당을 처리하고, 해당 메모리를 적절한 위치로 다시 삽입하여 할당을 해제한다. 
- 여러 스레드가 동시에 힙에 메모리를 할당할 수 있기 때문에 locking, 기타 동기화 메커니즘을 통해 무결성을 보호해야 한다. (오버헤드로 이어짐)
- <span style="color:#9fb584">**컴파일 단계에서 생성과 해제를 알 수 없는 reference type 인스턴스가 저장된다.**</span>
  - 인스턴스 자체는 힙에 저장하고, 힙의 주소값을 식별자와 함께 스택에 저장한다.
  - 이 주소값을 이용해 실행 중 필요한 데이터를 가져올 수 있다.
  - <span style="color:#9fb584">**런타임 시 크기가 결정**</span>된다.
- <span style="color:#9fb584">**value type 인 Array, Dictionary, Set, String 등은 최적화를 위해 heap공간을 같이 활용한다.**</span>


<br/>

## Reference Counting

---

- 참조된 인스턴스의 개수를 세는 것이다.
- 강한 참조(Strong Reference)와 약한 참조(Weak Reference)로 나누며, 일반적인 Reference Counting은 Strong Reference를 의미한다.
- <span style="color:#9fb584">**힙 영역에 할당하는 것 자체로 레퍼런스를 사용하기 때문에 레퍼런스 카운팅이 발생한다.**</span>
- swift는 레퍼런스 카운팅을 통해 할당 해제 여부를 결정한다.
    - swift는 힙에 있는 모든 인스턴스의 레퍼런스 카운트를 가지고 있고, 인스턴스가 이를 직접 가지고 있게 한다.
    - 이는 레퍼런스가 추가, 제거될 때 값이 변경 된다. (optional type 변수라면 nil 선언시 값 감소)
    - <span style="color:#9fb584">**카운트가 0**</span>이 되면 swift는 해당 인스턴스를 메모리에서 해제하기에 안전하다고 판단하여 힙을 락하고 메모리 블럭을 반환한다.

- 힙에 할당되지 않는 <span style="color:#9fb584">**struct에서도 레퍼런스 카운팅이 발생**</span>할 수 있다.
    - struct가 reference 타입을 프로퍼티로 가질 경우 발생한다.
    - reference counting으로 오버헤드를 처리하는 비용이 들게 된다.
        - overhead : 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리
        - 레퍼런스 카운팅 오버헤드는 구조제의 레퍼런스 개수에 비례한다.
    - <span style="color:#9fb584">**struct가 하나보다 더 많은 레퍼런스 타입을 가지게 된다면 reference counting 오버헤드가 class보다 더 많이 발생하게 된다.**</span>

<br/>

## Method Dispatch

---

- 프로그램이 어떤 메소드를 호출할건지 결정해 그 메소드를 호출하는 과정을 말한다.
- 결정 시점에 따라 <span style="color:#9fb584">**static method dispatch, dynamic method dispatch**</span> 로 나뉘게 된다.
- 레퍼런스 카운팅, 힙 할당과 같은 스레드 동기 오버헤드가 없기 때문에 단일 static dispatch와 dynamic dispatch는 큰 비용차가 없다.
    - 여러 method dispatch가 발생하는 dispatch chain에서는 차이가 있다.
    - dynamic dispatch chain은 컴파일러가 추론할 수 없다.
    - static dispatch chain은 컴파일러가 메소드 인라이닝으로 붕괴시켜 콜 스택 오버헤드 없이 하나의 코드 덩어리로 바꿀 수 있다.
- static에서는 컴파일러가 최적화 작업이 가능하지만, dynamic에서는 컴파일러가 추론할 수 없다.

<br/>

[ Static Method Dispatch ]

- <span style="color:#9fb584">**컴파일 시점**</span>에 컴파일러가 메소드의 실제 코드 위치를 파악해 런타임에 찾는 과정 없이 해당 코드를 바로 실행한다.
- 구현된 코드가 어디서 실행되는지 알 수 있기 때문에 메소드 인라이닝과 같은 코드 최적화를 시행한다.
    - Method Inlining : 메소드 호출 시 해당 메소드로 이동하지 않고 메소드의 결과값을 바로 반환하여 성능을 향상 시키는 것
- struct

<br/>

[ Dynamic Method Dispatch ]

- 컴파일 타임에 어떤 메소드를 호출하는지 판단할 수 없다.
- <span style="color:#9fb584">**런타임**</span>에 모든 과정이 일어나기 때문에 오버헤드가 발생하고, 성능상 손해를 볼 수 있다.
- 런타임에 vTable 구현을 참조해 해당 메소드에 대한 정보를 가져화 코드를 실행시킨다.
    - vTable : 클래스 내부 함수 중 어떤 함수를 불러올지 결정하는 함수 포인터 테이블으로, 오버라이딩된 함수가 있을 수도 있고, 확장된 함수도 있을 수 있으니 이 중에서 한 함수를 찾는 것이다.
- class

<br/>

## Struct  - Value Type

---

- 인스턴트를 생성하여 다른 인스턴스에 할당하면, 전체 값이 <span style="color:#9fb584">**복사**</span>된다. 
- 복사된 인스턴스는 기존 인스턴스와 구분되어 스택에 저장되기 때문에  <span style="color:#9fb584">**내부 값이 변경되어도 원래 값에 영향을 주지 않는다.**</span>
- Heap을 사용하지 않기 때문에 reference counting도 사용하지 않는다.
- 상속이 불가능하다.
    - 타입 계층을 만들 수 있는 protocol 사용
- 자동으로 초기화가 적용된다.
- static dispatch로 메소드를 호출한다.

<br/>

[ Struct 사용 권장 조건 ]

- 연관성 있는 간단한 데이터 값의 캡슐화를 원할 경우
- 캡슐화한 값을 <span style="color:#9fb584">**참조하는 것보다 복사하는 것이 합당할 경우**</span>
- 프로퍼티가 <span style="color:#9fb584">**value 타입일 경우**</span>
- 다른 타입으로 <span style="color:#9fb584">**상속받거나 상속할 필요가 없을 경우**</span>

<br/>

## Class - Reference Type

---

- 주소값은 스택에, 데이터는 힙에 할당되고 이로 인해 reference counting이 발생한다. (ARC가 관리)
- 클래스는 Reference Semantics로 항상 하나의 <span style="color:#9fb584">**identity**</span>이다.
- 인스턴스를 생성하고 복사하면 인스턴스가 아닌 stack에 있는 레퍼런스가 복사되기 때문에 기존 인스턴스와 복사된 인스턴스는 같은 값을 가리키게 된다. 
- 복사된 인스턴스 수정 시 기존 인스턴스의 데이터도 변경된다.
- deinit을 사용해 클래스 인스턴스의 메모리 할당을 해제할 수 있다.
- dynamic dispatch로 메소드를 호출한다.
    - final 키워드 사용 시 static Dispatch로 실행되어 런타임 성능이 향상된다.

> 💡 final 키워드<br/>
> 해당 클래스, 메소드, 프로퍼티가 상속이 불가능하고, 하위 클래스에서 오버라이딩할 수 없다는 것을 뜻한다.<br/>
> Dynamic Dispatch가 아닌 Static Dispatch를 통해 Directly Call을 하기 때문에 런타임 성능 향상을 기대할 수 있다.

<br/>

[ Class 사용 권장 조건 ]

- <span style="color:#9fb584">**고유성**</span>이 필요한 인스턴트일 경우
    - 레퍼런스 타입의 인스턴스는 identity를 가지기 때문에 값이 같다고 해서 두 인스턴스가 같다고 할 수 없다.
    - 값 타입의 인스턴스는 두 값이 같으면 동일하다고 판단한다.
    - 위치값을 저장하는 position은 고유한 identity가 없고 x,y 값이 동일하면 같기 때문에 struct가 적합하다.
    - 학생을 저장하는 student는 고유한 identity가 있다. 이름과 나이가 같다고 해서 같은 학생이 아니기 때문이다. 이 경우 class가 적합하다.
- Objective-C와 호환성이 필요할 경우
- <span style="color:#9fb584">**변경 가능한 상태를 공유하는 것이 꼭 필요한 경우**</span>
    - 데이터의 상태를 계속 바꿔 업데이트 해야 하는 경우
    - linked list를 구현하는 경우
<br />



[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
