---
title: "iOS 면접 준비 - 운영체제"
author: gyeomji
date: 2024-08-15 10:00:00 +0900
categories: [Interview]
tags: [운영체제]
pin: false
math: true
mermaid: true
---

## ✏️ 운영체제의 역할과 iOS에서의 운영체제 구조

---

- 운영체제는 컴퓨터의 주기억장치에 상주하여, 사용자가 컴퓨터의 하드웨어를 쉽게 사용할 수 있도록 인터페이스를 제공해주는 소프트웨어이다.
- 운영체제는 자원관리 및 운영과 보호, 사용자와 컴퓨터 하드웨어 간 인터페이스를 제공하며 효율성, 안정성, 확장성, 편리성을 목표로 동작한다.
- iOS에서의 운영체제는 하위 계층부터 Core OS / Core Service / Media / Cocoa Touch / Application으로 구성되어 있다.

[📝 운영체제](https://gyeom-ji.github.io/posts/operatingSystem/)

<br/>


## ✏️ 프로세스와 스레드의 차이점, iOS에서의 프로세스와 스레드 관리 방법

---

- 프로세스는 운영체제로부터 자원을 할당받는 작업의 단위이고, 스레드는 프로세스가 할당받은 자원을 이용하는 실행의 단위이며 CPU가 처리하는 작업의 단위이다.
  - 프로세스가 생성되면 CPU 스케줄러는 프로세스가 해야 할 일(스레드)을 CPU에 전달하고 실제 작업은 CPU가 수행한다.
- 프로세스는 독립된 개체로서 생성되면 각각의 메모리 자원을 할당 받고, 서로 접근할 수 없기 때문에 IPC를 통해서 통신할 수 있다.
  - 독립적인 메모리 공간으로 문맥 교환 Context Switch이 발생한다.
  > 프로세스를 여러 개 생성하는 방식은 많은 시간과 자원을 소비한다. 새 프로세스가 해야 할 일이 기존 프로세스가 하는 일과 동일하다면, 새로운 프로세스를 생성하거나 두 프로세스 사이에서 데이터 교환을 위한 IPC를 적용하기 보다 프로세스 안에 여러 스레드를 만들어 나가는 것이 더 효율적이다.
- 스레드는 프로세스 내부에 존재하기 때문에 프로세스의 데이터 영역에 접근이 가능하고, 스레드들끼리 서로의 데이터에 접근 가능하다.
  - 프로세스는 메모리 공간에 code, data, heap, stack 영역이 있고, 스레드는 프로세스내에서 stack 영역을 제외한 code, data, heap 영역을 공유한다.
- iOS에는 `Main`과 `Global Thread(Background Thread)`가 있다.
- 애플에서 제공하는 API인 `GCD(Grand Central Dispatch)`, `NSOperation`을 사용해 스레드를 관리한다.
  - 복잡한 스레드 관리를 피하고 메모리 사용량을 최적화할 수 있다.
  - 메인 스레드에 몰린 Task를 Queue에 보내면 다른 스레드(글로벌 스레드)를 적절히 생성하여 분배해준다.

[📝 프로세스](https://gyeom-ji.github.io/posts/process/)
[📝 스레드](https://gyeom-ji.github.io/posts/thread/)

<br/>

<details>
<summary>Main, Global Thread 💡</summary>
<div markdown="1">

### Main Thread

- Main Thread는 오직 한 개만 존재하며 나머지는 모두 Background Thread이다.
- 개발자가 작성한 코드는 일반적으로 Main Thread에서 동작한다.
- 작성된 코드는 Cocoa에서 실행되는데, 이 Cocoa가 코드를 MAin Thread에서 호출하기 때문이다.
> Cocoa : OS X, iOS 애플리케이션을 개발할 때 사용하는 프레임워크를 모두 포함하는 개념
- Main Thread는 interface Thread라고도 불리는데, 유저가 인터페이스에 접근하면 이벤트가 메인 스레드에 전달되고 개발자가 작성한 코드는 이에 반응해야하기 때문이다.
- 따라서 UI와 관련된 작업은 반드시 Main Thread에서만 작성되어야 한다.
- 메인 스레드는 Main Queue에서 실행되는데 메인 큐는 Serial Queue이다.
- 따라서 Main Thread에서 작업 시간이 오래걸리는 Task(비동기적으로 실행되는 단일 작업)를 수행하면 화면이 정지된것처럼 멈춰있다. 
> Serial : 단일 작업들을 직렬로 하나씩 순차적으로 실행하는 개념

<br/>

### Global Thread (Background Thread)

- iOS의 프레임워크들은 백그라운드에서 구동된다.
  - 몸체는 백그라운드에 있고, 가끔 메인 스레드에 delegate, Completion Handler를 호출한다.
- 화면에 나타나지 않는 작업은 백그라운드 스레드에 맡기고 delegate method나 callback 함수를 Main Thread에서 호출하여 이벤트를 컨트롤하는 것이 일반적이다.
- Global Thread는 Global Queue에서 실행되며, 이 Global Queue는 Concurrent Queue이다.
- Global Queue에서는 Task의 우선도를 선택할 수 있으며, 직접 선택이 아닌 QoS에 있는 QosClass에서 설정한다.
> Concurrent : 동시에 여러 개의 단일 작업들이 병렬로 실행되는 개념으로 한 번에 여러 개의 Task를 실행시킬 수 있다.
- 자동으로 Background에서 실행되는 작업들도 있지만, 명시적으로 Background에 코드를 작성해야하는 경우도 존재한다.
  - code가 callBack 되는데 Main Thread에 없는 경우 (개발자가 작성한 코드가 의도와 상관없이 background에서 실행되는 경우)
  - code 수행에 지나치게 오랜 시간이 걸리는 경우
    - 너무 오랜 시간 동안 메인 스레드가 블락되면 앱이 강제 종료될 수 있다.


</div>
</details>

<br/>

<details>
<summary>GCD, NSOperation 💡</summary>
<div markdown="1">

### GCD (Grand Central Dispatch)

- 개발자가 Queue에 작업을 보내면 그에 따른 스레드를 적절히 생성해서 분배해주는 방법이다.
  - Queue에 보내야 하니까 GCD의 큐 이름이 `DispatchQueue`이다.
  - DispatchQueue에 작업을 추가하면 GCD는 작업에 맞는 스레드를 자동으로 생성해서 실행하고, 작업이 종료되면 스레드를 제거한다.
- C언어 기반의 저수준 API로 가볍고 성능 면에서 뛰어나다.
- Block(Closure)으로 구현되어 있어 가독성과 사용성이 좋다.
- 작업 취소, 재사용 등은 직접 만들어야 한다.
- DispatchQueue에는 두 가지 Type이 존재하고, App이 실행함과 동시에 구조적으로 두가지 Queue가 자동 생성된다.
  - Main Queue
    - 오직 한 개만 존재한다.
    - Serial 특성을 가진다.
    - 메인 큐에 할당된 task는 메인 스레드에서 처리된다.(UI update)
    - 메인 큐에서는 교착 상태에 빠질 수 있기 때문에 sync task를 추가할 수 없다.
    ``` swift
    DispatchQueue.main.async {
	  // task
    }
    ```
  - Global Queue
    - Concurrent 특성을 가진다.
    - QoS (Quality Of Service) 를 결정할 수 있고 이에 따라 각각 다른 큐 객체를 생성한다.(6 종류)
    - 메인 큐에서는 교착 상태에 빠질 수 있기 때문에 sync task를 추가할 수 없다.<br/>
    
    ``` swift
    DispatchQueue.global().sync {
	  // task

    }

    DispatchQueue.global().async {
	  // task

    }
    ```
- UI는 메인 스레드에서 처리한다.
- 메인 큐에서 다른 큐로 작업을 보낼 때 sync를 사용하면 안된다.
- 현재와 같은 큐에 sync로 작업을 보내면 안된다.

<br/>

### NSOperation

- OperationQueue에 작업을 넣으면 스레드를 적절히 생성해 분배해준다.
- Object-C 언어 기반의 고수준 API이다.
- 내부적으로는 C로 구현된 GCD를 고수준 언어로 Wrapping 한 것으로, GCD보다 무겁다.
- 작업 취소, KVO, 작업 재사용 등 GCD에 비해 고성능 기능을 제공한다.


</div>
</details>

<br/>

## ✏️ 메모리 관리 기법 중 iOS에서 사용되는 방식과 그 특징

---

- iOS에서는 ARC(Automatic Reference Counting)로 메모리를 추적하고 관리한다.
- 객체의 참조 카운트를 자동으로 관리하여 클래스 인스턴스가 더 이상 필요하지 않을 때 사용되지 않는 메모리를 자동으로 해제하고, 메모리 누수를 방지한다.
- ARC는 레퍼런스 카운팅을 통해 할당 해제 여부를 결정한다.
  - 힙에 있는 모든 인스턴스의 레퍼런스 카운트를 가지고 있고, 인스턴스가 이를 직접 가지고 있게 한다.
  - 객체가 생성될 때 참조 수는 1로 설정된다. 
  - 다른 객체가 해당 객체에 대한 참조를 가지면 참조 수가 증가한다. 
  - 객체의 참조 수가 0에 도달하면 자동으로 힙을 락하고 메모리 블럭을 반환한다.
- 강한 참조(Strong Reference)와 약한 참조(Weak Reference)로 나누며, 일반적인 Reference Counting은 Strong Reference를 의미한다.
- ARC가 아직 사용중인 인스턴스의 할당을 해제할 경우 해당 인스턴스의 프로퍼티에 접근하거나, 메소드를 호출할 수 없다. 
  - 해당 인스턴스에 접근할 경우 앱에 크래시가 발생하게 된다. 
  - 때문에 ARC는 인스턴스에 참조가 하나라도 존재하면 인스턴트를 할당 해제하지 않는다. 
  - 이를 가능하게 하기 위해 인스턴스를 할당할 때마다 강한 참조를 만든다.
  - 강한 참조는 참조 카운트를 증가시키고, 직접 카운트를 감소시키지 않으면 카운트가 내려가지 않는다.
- 강한 참조로 인해 메모리 누수가 발생할 수 있다. 
  - 한 클래스에서 Strong Reference Count가 절대로 0이 되지 못하는 상황이 발생하면 메모리에서 해당 인스턴스를 해제할 수 없다.
  - 이는 클래스 인스턴스 두 개가 서로에 대한 강한 참조를 가지고 있을 경우 발생할 수 있고 이를 `강한 참조 순환`이라고 한다.
- 강한 참조 순환을 방지하기 위해 약한 참조와 미소유 참조를 사용한다.
  - 약한 참조(weak)는 참조 카운트를 증가시키지 않는 (인스턴스를 강하게 유지하지 않는) 참조이므로 ARC가 참조된 인스턴스를 처리하는 것을 중지하지 않는다.
  - 약한 참조가 참조하는 동안 해당 인스턴스가 할당 해제 될 수 있다. 
  - 다른 인스턴스가 먼저 할당이 해제될 때 약한 참조를 사용한다.
  - 미소유 참조(Unowned)는 참조 카운트를 증가시키지 않는 (인스턴스를 강하게 유지하지 않는) 참조로 다른 인스턴스의 수명이 같거나 더 긴 경우에 사용한다. 
  - 미소유 참조는 항상 값을 가지도록 예상되기 때문에, 미소유로 만들어진 값은 옵셔널로 만들어지지 않고 ARC는 미소유 참조의 값을 nil로 설정하지 않는다.
  - 💡 인스턴스가 메모리에서 해제되지 않는다는 확신이 있을 경우에만 사용한다. 
  - 인스턴스가 할당 해제된 후 미소유 참조 값에 접근시 런타임 에러 발생

[📝 ARC](https://gyeom-ji.github.io/posts/arc/)

<br/>

## ✏️ iOS에서의 메모리 구조와 관리 방식

---

- Swift에서 메모리는 주로 네 가지 영역으로 나눌 수 있다. 

![memoryStructure](/assets/img/memoryStructure.png)

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

``` swift
var name: String = "gyeomji"
var age: Int = 10

struct Place {
    static let country: String = "Korea"
}

// name, age, country 데이터 영역에 저장
```

<br/>

### Stack 영역

- <span style="color:#9fb584">**Stack**</span>은 일시적인 데이터를 저장하며, 하나의 명령어(pop, push)로 메모리를 할당, 해제할 수 있기 때문에 <span style="color:#9fb584">**매우 빠르고 효율적이다.**</span>
- 지역변수, 파라미터를 저장한다.
- 높은 주소(위) -> 낮은 주소(아래)로 할당된다.
- 스택 영역의 데이터는 <span style="color:#9fb584">**CPU가 관리하고 최적화하기 때문에 메모리에 빈 공간이 발생하지 않는다.**</span>
  - 함수 호출시 새로운 스택 프레임이 생성되고, 함수 실행이 끝나면 자동으로 해당 스택 프레임이 해제된다.
- <span style="color:#9fb584">**컴파일 단계에서 생성과 해제를 알 수 있는 value type 인스턴스가 저장된다.**</span>
  - <span style="color:#9fb584">**컴파일 시 크기가 결정**</span>된다.
- reference type 중에서도 크기가 고정되어있거나, 언제 지워야할지 컴파일러가 미리 예측 가능한 경우 가급적 stack을 사용해 성능을 향상시킨다.

``` swift
func add(_ a: Int, _ b: Int) -> Int {
    // result 변수 스택 영역에 할당
    // 함수 종료 시 result 변수는 자동으로 스택에서 해제
    let result = a + b
    return result
}
```

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
  - 클래스 인스턴스, 클로저와 같은 참조 타입의 값
  - 인스턴스 자체는 힙에 저장하고, 힙의 주소값을 식별자와 함께 스택에 저장한다.
  - 이 주소값을 이용해 실행 중 필요한 데이터를 가져올 수 있다.
  - <span style="color:#9fb584">**런타임 시 크기가 결정**</span>된다.
- <span style="color:#9fb584">**value type 인 Array, Dictionary, Set, String 등은 최적화를 위해 heap공간을 같이 활용한다.**</span>

``` swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

// first, second 힙 영역에 할당
// 동일한 메모리 위치 참조 ➡️ 한 변수를 통해 다른 변수 값 변경
let first = Person(name: "gyeomji")
let second = first

second.name = "yun"
print(second.name) // yun
print(first.name) // yun

```



<br/>

## ✏️ iOS 디바이스의 메모리 제약과 앱 메모리 제한

---

- iOS 디바이스는 메모리 제약이 있어 앱을 설계할 때 이를 고려해야 한다.
- 각 앱은 시스템 메모리와 디바이스의 하드웨어에 따라 메모리 제한이 다르다.
- 일반적으로 iOS는 앱에 대해 고정된 메모리 할당량을 두고 있으며, 이 한계를 초과하면 앱이 강제로 종료될 수 있다.
- 메모리 누수나 비효율적인 메모리 사용은 앱의 성능 저하와 충돌을 초래할 수 있다.

<br/>

### 메모리 제약 요인

- 디바이스 종류 : 최신 모델일수록 메모리가 크고, 구형 모델일수록 적은 메모리를 가진다.
- OS 버전 : iOS의 업데이트에 따라 메모리 관리 방식이 변화할 수 있다. 
  - 최신 버전은 메모리 최적화를 더 잘 지원힌다.
- 다중 작업 : iOS는 여러 앱을 동시에 실행할 수 있지만, 각 앱은 시스템 자원(메모리, CPU 등)을 공유한다.


<br/>

### 앱 메모리 제한

- 기본 제한 : 일반적으로 앱은 150MB에서 2GB까지 메모리를 사용할 수 있으며, 이는 디바이스의 종류와 상태에 따라 달라질 수 있다.
- 응답성 유지 : 앱이 메모리 한계를 초과할 경우, 시스템은 `메모리 경고`를 보내고, 이 경고를 처리하지 않으면 앱이 종료될 수 있다.
  - 메모리 경고 처리는 iOS 앱 개발에서 중요한 메모리 관리 전략 중 하나이다.
- 메모리 최적화 : 앱에서는 비동기적 데이터 로딩, 이미지 및 캐시 관리, 사용하지 않는 객체 해제 등을 통해 메모리 사용을 최적화해야 힌다.

<br/>

### ✏️ 메모리 경고(Memory Warning)가 발생하면 어떤 조치를 취해야 하나요?

- 캐시된 데이터를 삭제한다.
- 사용하지 않는 리소스를 해제한다.
- 메모리 사용량을 줄이고 앱의 안정성을 높여야 한다.

<br/>

### ✏️ 메모리 정렬(Alignment)이 성능에 미치는 영향은 무엇인가요?

#### 메모리 정렬

- 데이터 구조 정렬, 데이터 정렬, 바이트 정렬 등으로 불린다.
- 컴퓨터 메모리에서 데이터를 특정 경계에 맞춰 저장하는 것이다.
- CPU는 처리할 데이터를 메모리(RAM)에서 가져오는데, 가져오는 작업이 CPU의 속도 저하를 일으키는 큰 원인이 된다.
  - 즉, CPU가 메모리에 접근하는 횟수가 많을수록 느려진다.
  - 때문에 모든 데이터 타입에는 메모리 정렬 속성이 있다.
- 자연 정렬(Natural Alignment), 패딩(padding), 구조체 정렬(Structure Alignment)과 같은 개념이 있다.
  - 자연 정렬 : 데이터 타입의 크기에 맞춰 정렬한다.
    - ex) 4바이트 Int는 4의 배수 주소에 저장한다.
  - 패딩 : 데이터를 정렬하기 위해 사용하지 않는 바이트(패딩 파이트)를 메모리에 삽입한다.
    - 일반적으로 0x00의 형태이다.
    - 정렬되지 않은 데이터로 인해 메모리 접근 속도가 느려지고 효율성이 떨어지는 것을 방지한다.
    - 패딩을 사용할 경우 조금 더 많은 메모리를 사용하는 대신, 실행 속도를 빠르게 만들 수 있다.
  - 구조체 정렬 : 구조체 전체를 가장 큰 멤버의 크기에 맞춰 정렬한다.
    - 첫 번째 멤버 : 구조체의 시작 주소에 위치한다.
    - 다음 멤버들 : 자신의 크기나 가장 큰 멤버의 크기 중 작은 값의 배수 주소에 위치한다.
    - 전체 구조체 : 가장 큰 멤버의 크기가 배수가 되도록 패딩이 추가된다.
    - 구조체 멤버의 순서를 바꾸면 전체 구조체의 크기가 달라질 수 있다.
    - 크기가 작은 멤버부터 큰 멤버 순으로 배치하면 대체로 메모리를 절약할 수 있다.
- CPU는 데이터가 자연스럽게 정렬(naturally aligned) 될 때 (데이터 주소가 데이터 사이즈의 배수일 때) 메모리에 대한 읽기/쓰기를 효율적으로 수행할 수 있다.
  - 자연 정렬을 보장하기 위해 구조 요소 사이에 또는 구조의 마지막 요소 뒤에 패딩을 삽입해야 한다.
- 대부분의 CPU는 메모리에서 데이터를 가져올 때 메모리 정렬이 된 데이터 블록만 가져올 수 있다.
  - 메모리 정렬의 기준은 아래과 같다.
  - `1byte 정렬 객체는 어떤 메모리 주소에도 올 수 있다.`
  - `2byte 정렬 객체는 짝수인 주소에만 올 수 있다. (주소 끝이 0, 2, 4, 8, A, C, E)`
  - `4byte 정렬 객체는 4의 배수가 되는 주소에만 올 수 있다. (주소 끝이 0, 4, 8, C)`
  - `16byte 정렬 객체는 16의 배수가 되는 주소에만 올 수 있다.(주소 끝이 0)`
  - CPU는 위와 같은 규칙에 맞게 메모리의 데이터를 가져온다.

- ex) 32bit 정수 값이 아래와 같이 저장되어 있을 경우

![memoryAlignment](/assets/img/memoryAlignment.png)

- 왼쪽은 0x3A4A64라는 주소의 메모리 블럭에 4바이트 값이 정렬되어 저장되어 있다. (끝 주소가 4의 배수)
- 오른쪽은 0x3A4A60라는 주소의 메모리 블럭과 0x3A4A64라는 주소의 메모리 블럭에 나뉘어 저장되어 있다.
- CPU의 메모리 컨트롤러(MC) 또는 메모리 컨트롤러 유닛(MCU)은 정렬된 값을 읽을 때 다음과 같이 레지스터에 저장할 수 있다.

![memoryAlignment](/assets/img/memoryAlignment2.png)

- 정렬되지 않은 값을 읽을 땐 다음과 같은 작업을 수행한 후 레지스터에 저장한다.

![memoryAlignment](/assets/img/memoryAlignment3.png)

- 값이 두 블럭에 나뉘어 있기 때문에 MCU는 두 블럭을 모두 읽는다.
- 하나의 블록 안에 순서대로 합쳐질 수 있도록 shift 연산을 수행하고, OR 연산하여 하나의 데이터 블록으로 레지스터에 저장한다.
- 어떤 CPU는 이런 작업을 해주지 않고, 데이터가 정렬이 되어있지 않다면 쓰레기 값으로 읽거나 프로그램을 강제 종료한다.
- CPU가 위와 같은 작업을 하지 않도록 컴파일러가 패딩 데이터를 추가하여 정렬 조건을 갖추도록 한다.

<br/>

### 메모리 정렬의 중요성

- 성능 향상: 정렬된 데이터는 CPU가 더 빠르게 접근할 수 있다.
  - 캐시 라인에 맞춰 데이터를 정렬하면 캐시 미스를 줄일 수 있다.
  - 정렬된 데이터는 버스를 통한 데이터 전송을 최적화한다.
- 하드웨어 호환성: 일부 프로세서는 정렬되지 않은 데이터 접근 시 오류를 발생시킬 수 있다.
- 크로스 플랫폼, 네트워크 통신, 파일 입출력 시 활용할 수 있다.
  - 크로스 플랫폼 호환성 : 다른 시스템 간 데이터를 주고받을 때 패딩을 고려해야 한다.
  - 네트워크 통신 : 패킷 구조를 설계할 때 패딩을 고려해야 한다.
  - 파일 입출력 : 구조체를 파일에 저장하거나 읽을 때 패딩을 고려해야 한다.


<br/>

## ✏️ 메모리 관리에서 강한 참조(Strong Reference)와 약한 참조(Weak Reference)의 차이점은 무엇인가요?

---

- 강한 참조와 약한 참조는 Swift에서 메모리 관리와 객체 생명주기를 관리하기 위해 사용하는 참조 방식이다.
- 이 두 참조 방식은 ARC(Automatic Reference Counting) 시스템을 통해 메모리를 효율적으로 관리하는 데 중요한 역할을 한다.
- 객체가 `강한 참조`로 연결되어 있으면 <span style="color:#9fb584">**ARC가 객체를 메모리에서 해제하지 않고 유지**</span>한다.
- `약한 참조`는 <span style="color:#9fb584">**객체가 메모리에서 해제되도록 허용하여 순환 참조(Strong Reference Cycle) 문제를 방지**</span>할 수 있다.

### 강한 참조

- 강한 참조는 기본 참조 방식으로, <span style="color:#9fb584">**객체의 참조 횟수를 증가시키며 참조하는 동안 객체가 메모리에서 해제되지 않도록 한다.**</span>
- 강한 참조를 사용하면, 참조가 있는 동안 <span style="color:#9fb584">**객체의 생명주기가 유지**</span>되며, 해당 객체는 ARC에 의해 메모리에서 해제되지 않는다.
- 대부분의 속성은 기본적으로 강한 참조를 사용하며, 일반적인 객체 간의 참조에서 사용된다.

``` swift
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
}

var person1 = Person(name: "Gyeomji")  // person1이 Person 인스턴스를 강한 참조함
var person2 = person1                 // person2도 Person 인스턴스를 강한 참조함

person1 = nil                         // person2가 여전히 강한 참조 중이므로 Person 인스턴스는 해제되지 않음
```

### 약한 참조

- 약한 참조는 <span style="color:#9fb584">**객체의 참조 횟수를 증가시키지 않는**</span> 참조 방식이다.
- 객체가 <span style="color:#9fb584">**다른 강한 참조가 없으면 ARC가 메모리에서 해제할 수 있도록 허용**</span>한다.
- 약한 참조는 참조 대상이 해제되면 nil이 되며, <span style="color:#9fb584">**순환 참조 문제를 방지하는 데 유용**</span>하다.
- 런타임에 값을 nil으로 변경될 수 있기 때문에 항상 옵셔널 타입의 변수로 선언한다.
- 다른 인스턴스가 먼저 할당이 해제될 때 약한 참조를 사용한다.
- 일반적으로 뷰 컨트롤러의 델리게이트나 순환 참조가 발생할 수 있는 상황에서 사용한다.
- var로 선언하여야 하며, weak 키워드를 사용하고 옵셔널 타입이어야 한다.

``` swift
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Apartment {
    var unit: String
    weak var tenant: Person?  // 약한 참조로 선언하여 순환 참조 방지
    init(unit: String) {
        self.unit = unit
    }
}

var person = Person(name: "Alice")
var apartment = Apartment(unit: "101")

apartment.tenant = person     // 약한 참조로 tenant가 person을 참조
person = nil                  // Person 인스턴스 해제
print(apartment.tenant)       // 출력: nil (person이 해제되면서 tenant도 nil)
```


<br/>

## ✏️ 순환 참조(Retain Cycle)가 발생하는 경우와 해결 방법은 무엇인가요?

---

- 순환 참조(Retain Cycle)는 두 객체가 서로를 강하게 참조할 때 발생하는 문제로, 서로가 참조를 유지하고 있어서 메모리에서 해제되지 않는 상태이다.
- Swift에서는 ARC(Automatic Reference Counting)가 메모리를 관리하지만, 순환 참조가 발생하면 ARC가 객체의 참조 횟수를 0으로 만들지 못해 메모리 누수가 발생한다.
- 순환 참조는 주로 클로저와 델리게이트 패턴에서 발생할 수 있으며, 약한 참조와 미소유 참조를 사용해 해결할 수 있다.

### 순환 참조가 발생하는 경우

#### 1. 클래스 간 상호 참조

- 두 클래스가 서로를 강하게 참조하는 경우, 양쪽 모두 참조가 남아있어서 해제되지 않는 상태가된다.

``` swift
class Person {
    var pet: Pet?
    deinit {
        print("Person is being deinitialized")
    }
}

class Pet {
    var owner: Person?
    deinit {
        print("Pet is being deinitialized")
    }
}

var gyeomji: Person?
var ben: Pet?

gyeomji = Person() // gyeomji -> Person (strong reference)
ben = Pet() // ben -> Pet (strong reference)

gyeomji!.pet = ben // ben, Person -> Pet (strong reference)
ben!.owner = gyeomji // gyeomji, Pet -> Person (strong reference)

gyeomji = nil // Person -> Pet (strong reference)
ben = nil // Pet -> Person (strong reference)
```

<br/>

#### 2. 클로저에서의 강한 참조

- 클로저는 자신이 캡처하는 객체를 기본적으로 강하게 참조하므로, 클로저 내부에서 객체의 프로퍼티나 메서드를 참조할 때 순환 참조가 발생할 수 있다.

``` swift
class ViewController {
    var name = "Main ViewController"
    
    lazy var printName: () -> Void = {
        print(self.name)  // self를 강하게 참조하여 순환 참조 발생
    }
}

let viewController = ViewController()
viewController.printName()
```


### 순환 참조 해결 방법

#### 1. 약한 참조 weak 사용

- 서로를 참조하는 두 객체 중 하나를 약한 참조(weak)로 선언하여, <span style="color:#9fb584">**참조 횟수를 증가시키지 않고 객체가 해제**</span>될 수 있도록 한다.
- 해당 인스턴스가 <span style="color:#9fb584">**할당 해제 되면 ACR는 약한 참조를 nil으로 자동 설정**</span>한다.
- 런타임에 값을 nil으로 변경될 수 있기 때문에 <span style="color:#9fb584">**항상 옵셔널 타입의 변수로 선언**</span>한다.
- <span style="color:#9fb584">**다른 인스턴스가 먼저 할당이 해제될 때**</span> 약한 참조를 사용한다.

``` swift
class Apartment {
    weak var tenent: Person? // 어느 시점에 거주자가 없을 수도 있음
    deinit {
        print("Apartment is being deinitialized")
    }
}

class Person {
    var apartment: Apartment?
    deinit {
        print("Person is being deinitialized")
    }
}

var villiv: Apartment?
var gyeomji: Person?

villiv = Apartment() // Apartment RC = 1
gyeomji = Person() // Person ReferenceCount = 1

villiv!.tenent = gyeomji // Person RC = 1 (Apartment -> Person  약한 참조는 카운트를 증가시키지 않음)
gyeomji!.apartment = villiv // Apartment RC = 2

villiv = nil // Apartment RC = 1
gyeomji = nil // Person RC = 0 -> Person 인스턴스 해제 -> (Person -> Apartment) 강한 참조 - 1 -> Apartment RC = 0
// print
// Person is being deinitialized
// Apartment is being deinitialized
```

<br/>

#### 2. 미소유 참조 unowned 사용

- 약한 참조와 비슷하지만, <span style="color:#9fb584">**nil을 허용하지 않는 경우 사용**</span>한다.
- 미소유 참조는 <span style="color:#9fb584">**객체가 항상 존재할 것으로 확신할 때 사용하며, 객체가 해제될 때까지 미소유 참조는 참조를 유지**</span>한다.
  - 스턴스가 할당 해제된 후 미소유 참조 값에 접근시 런타임 에러가 발생한다.
- <span style="color:#9fb584">**다른 인스턴스의 수명이 같거나 더 긴 경우에 사용**</span>한다. 

``` swift
class Person {
    var card : Card? // 카드를 가지거나 가지지 않을 수 있다
    deinit {
        print("Person is being deinitialized")
    }
}

class Card {
    let company: String
    unowned let owner: Person // 항상 owner가 존재함 -> owner보다 오래 지속되지 않음
    init(company: String, owner: Person) {
            self.company = company
            self.owner = owner
    }
    deinit {
        print("\(company) card is being deinitialized")
    }
}

var gyeomji: Person?

gyeomji = Person() // Person ReferenceCount = 1
gyeomji!.card = Card(company: "sinhan", owner: gyeomji!)// Person -> Card RC = 1 (Card -> Person 미소유 참조는 카운트를 증가시키지 않음)

gyeomji = nil // Person RC = 0 -> Person 인스턴스 해제 -> (Person -> Card) 강한참조 - 1 -> Card RC = 0
// print
// Person is being deinitialized
// sinhan card is being deinitialized
```

<br/>

#### 3. 클로저에서 캡처 목록 Capture List 사용

- 클로저가 객체를 강하게 참조하지 않도록 캡처 목록을 사용하여 참조 유형을 약한 참조 [weak self] 또는 미소유 참조 [unowned self]로 지정할 수 있다.
- [weak self]를 사용해 self를 약한 참조로 캡처하여 클로저와 self 간의 순환 참조를 방지한다.
- 만약 self가 해제된 경우 nil로 설정되어 클로저 내부에서 안전하게 nil 체크를 할 수 있다.

``` swift
class ViewController {
    var name = "Main ViewController"
    
    lazy var printName: () -> Void = { [weak self] in  // 캡처 목록으로 약한 참조 사용
        guard let self = self else { return }
        print(self.name)
    }
}

let viewController = ViewController()
viewController.printName()
```
<br/>

## ✏️ 클로저에서 [weak self]와 [unowned self]의 차이는 무엇인가요?

---

### [weak self]

- 약한 참조를 사용하여 클로저가 self를 참조하도록 만든다.
- 약한 참조는 <span style="color:#9fb584">**객체가 메모리에서 해제될 수 있도록 허용하므로 참조된 객체가 해제되면 nil이 된다.**</span>
- [weak self]를 사용할 때 <span style="color:#9fb584">**self는 옵셔널(self?)로 처리**</span>해야 한다.
- <span style="color:#9fb584">**객체가 해제될 수 있는 가능성이 있는 경우에 사용**</span>한다.
- 보통 클로저 내부에서 <span style="color:#9fb584">**self의 존재 여부를 체크하고 안전하게 사용**</span>하기 위해 많이 사용된다.

``` swift
class ViewController {
    var name = "Main ViewController"
    
    func setupClosure() {
        let closure = { [weak self] in
            guard let self = self else {
                print("self가 해제되었습니다.")
                return
            }
            print("ViewController name: \(self.name)")
        }
        closure()
    }
}
```

### [unowned self] 

- 미소유 참조를 사용하여 클로저가 self를 참조하게 한다.
- <span style="color:#9fb584">**self가 항상 존재할 것으로 가정하므로 옵셔널이 아니며 self가 해제되어도 nil이 되지 않는다.**</span>
  - self가 해제된 후 참조하면 런타임 에러가 발생할 수 있다.
- self가 항상 존재한다고 확신할 때 사용한다.

``` swift
class ViewController {
    var name = "Main ViewController"
    
    func setupClosure() {
        let closure = { [unowned self] in
            print("ViewController name: \(self.name)")
        }
        closure()
    }
}
```

<br/>

## ✏️ iOS 앱에서 Multi-threading을 구현하는 방법은 무엇인가요?

---

- `GCD(Grand Central Dispatch)`, `Operation 및 OperationQueue`, `Swift Concurrency (async/await)`가 주로 사용된다.
- 각 방법은 비동기 작업을 효과적으로 관리하고, 앱의 성능과 사용자 경험을 향상시키는 데 중요한 역할을 한다.
- 간단한 비동기 작업: GCD 사용
- 작업 간 종속성이나 재사용이 필요한 작업: Operation & OperationQueue 사용
- 최신 Swift Concurrency 기능 사용 가능(iOS 15 이상): async/await 사용

### GCD(Grand Central Dispatch)

- Apple에서 제공하는 멀티스레딩 API로, 간단하게 비동기 작업을 처리할 수 있다.
- 주로 비동기 큐와 동기/비동기 메서드를 사용하여 작업을 백그라운드에서 처리하고, 필요할 때 메인 스레드로 전환하는 방식으로 구현한다.
- `DispatchQueue.main` : 메인 큐, UI 업데이트는 항상 메인 큐에서 실행되어야 한다.
- `DispatchQueue.global()` : 백그라운드 큐, 네트워크 호출이나 데이터 처리 같은 CPU 집중 작업을 수행할 때 사용한다.

``` swift
DispatchQueue.global().async {
    // 백그라운드 작업 (예: 네트워크 호출, 데이터 처리)
    let result = performHeavyTask()
    
    DispatchQueue.main.async {
        // 메인 큐에서 UI 업데이트
        self.updateUI(with: result)
    }
}
```

### Operation 및 OperationQueue

- GCD를 기반으로 한 더 높은 수준의 API로, 작업 간의 종속성 설정이나 재사용이 가능한 작업을 객체 형태로 정의하는 등 GCD보다 더 많은 기능을 제공한다.
- OperationQueue를 통해 여러 Operation을 큐에 넣어 비동기로 실행할 수 있으며, 큐의 우선순위와 동시성 처리를 조절할 수 있다.
- `Operation` : 작업을 객체로 정의하여 관리할 수 있습니다.
- `OperationQueue` : 여러 Operation 객체를 관리하고, 비동기 실행 및 종속성 설정이 가능합니다.

``` swift
let operationQueue = OperationQueue()

let operation1 = BlockOperation {
    // 작업 1
    print("Operation 1")
}

let operation2 = BlockOperation {
    // 작업 2
    print("Operation 2")
}

// 종속성 설정: operation1이 완료된 후 operation2가 실행됨
operation2.addDependency(operation1)

operationQueue.addOperation(operation1)
operationQueue.addOperation(operation2)
```

### Swift Concurrency (async/await)

- Swift Concurrency는 Swift 5.5부터 도입된 비동기/동시성 처리를 위한 최신 기능으로, async와 await 키워드를 사용해 비동기 작업을 동기 코드처럼 간단하게 구현할 수 있다. 
- Task와 TaskGroup 등을 통해 멀티스레딩 작업을 보다 안전하고 직관적으로 처리할 수 있다.
- `async/await` : 비동기 함수는 async 키워드로 선언되며, 호출 시 await 키워드로 호출하여 비동기 작업을 동기 코드처럼 처리할 수 있다.
- `Task` : async 함수 외부에서 비동기 작업을 생성하는 방법이다.
- `TaskGroup` : 여러 작업을 동시에 처리하고, 완료 시 결과를 수집할 수 있다.
 
``` swift
func fetchData() async -> Data {
    // 네트워크 호출 (예시)
}

func updateUI(with data: Data) {
    // UI 업데이트
}

Task {
    // fetchData() 비동기로 호출
    let data = await fetchData()
    // UI 업데이트 메인 스레드에서 수행
    await MainActor.run {
        updateUI(with: data)
    }
}
```

<br/>

## ✏️ DispatchQueue와 OperationQueue의 차이점은 무엇인가요?

---

### Dispatch Queue

- 앱의 메인 스레드 또는 백그라운드 스레드에서 작업의 실행을 직렬 또는 동시에 관리하는 객체이다.
- 항상 선입선출 방식으로 실행된다.
- 대기열에 추가된 항목은 시스템이 관리하는 스레드 풀에서 처리하고 작업을 완료하면 알아서 스레드를 해제한다.
- 단순하고 간단한 비동기 작업에 적합하다.
- Serial Disptach Queue : 한 번에 하나의 작업만을 실행하며 해당 작업이 대기열에서 제외되고 새로운 작업이 시작되기 전까지 대기한다.
  - 별도로 명시하지 않으면 DispatchQueue의 기본은 Serial이다.
- Concurrent Disptach Queue : 이미 시작된 작업이 완료될 때까지 기다리지 않고 가능한 많은 작업을 진행한다.
  - 즉, 병렬처리 방식이다.

### Operation Queue

- NSOperation 객체의 우선순위 및 준비 상태에 따라 대기열에 있는 객체를 실행한다. (FIFO ❌)
  - Operation Queue에 추가된 작업은 작업이 완료될 때까지 대기열에 남아 있다.
  - 작업이 추가된 후에는 대기열에서 직접 제거할 수 없다.
- 동시에 실행할 수 있는 연산(Operation)의 최대 수를 지정할 수 있다. (GCD ❌)
- KVO를 사용할 수 있는 프로퍼티가 있다. (GCD ❌)
  > KVO (Key-Value Observing) : A 객체에서 B 객체의 <span style="color:#9fb584">**프로퍼티가 변화됨을 감지**</span>할 수 있는 패턴이다.
  - operations (read only) : 현재 큐에 있는 작업들
  - operationCount (read only) : 현재 큐에 있는 작업의 개수
  - maxConcurrentOpeartionCount (readable and writable) : 큐에서 동시에 실행할 수 있는 작업의 최대 개수 
  - suspended (readable and writable) : 실행 작업을 적극적으로 스케쥴링하고 있는지 여부에 대한 Boolean 값
  - name (readable and writable) : operationQueue의 이름
- Operation 일시 중지, 다시 시작 및 취소를 할 수 있다. (GCD ❌)
- 작업 간의 의존성을 직관적으로 설정할 수 있으며, 작업을 취소하고 대기 중인 작업을 무시하는 등의 작업 관리가 쉽게 가능하다.
- 코드가 보다 명확하며 작업의 의존성 및 실행 순서를 이해하기 쉽다.
- GCD 위에 구축되어 있으며, 더 복잡한 작업 관리 및 의존성 처리를 위해 설계되었으므로, 작업 추가 및 작업간 의존성이 큰 앱에서 유용할 수 있다.

> Operatin : 백그라운드 스레드에서 실행할 Task를 캡슐화한 객체


<br/>

## ✏️ 동시성 프로그래밍에서 Race Condition을 방지하는 방법은 무엇인가요?

---

- `Race Condition` : <span style="color:#9fb584">**여러 스레드가 동시에 동일한 자원에 접근하여 데이터의 일관성을 깨뜨리는 문제**</span>이다.
- <span style="color:#9fb584">**자원의 접근을 순차적으로 제어하거나 스레드 간의 접근을 동기화하는 방법**</span>이 있다.

### DispatchQueue의 Serial Queue 사용

- 직렬 큐(Serial Queue)는 하나의 스레드에서만 순차적으로 작업을 처리하므로, 동시에 여러 스레드가 접근하지 못하게 제어할 수 있다. 
- 공통 자원에 대한 접근을 직렬 큐로 제한하면 Race Condition을 방지할 수 있다.

``` swift
let serialQueue = DispatchQueue(label: "com.example.serialQueue")

serialQueue.async {
    // 자원에 접근하는 작업
    sharedResource += 1
}
```

### Dispatch Semaphore 사용

- 세마포어(Semaphore)는 특정 자원에 접근할 수 있는 스레드 수를 제한하는 방식으로, 여러 스레드가 동일 자원에 접근하는 것을 조절할 수 있다. 
- 동시에 하나의 스레드만 자원에 접근할 수 있도록 세마포어를 1로 설정하여 Race Condition을 방지한다.
  - 하나의 스레드가 자원에 접근하는 동안 다른 스레드는 대기한다.

``` swift
let semaphore = DispatchSemaphore(value: 1)

func updateSharedResource() {
    semaphore.wait() // 접근 시작
    sharedResource += 1
    semaphore.signal() // 접근 종료
}
```

### Swift의 Actor 사용 (Swift Concurrency)

- Swift 5.5부터 도입된 Actor 모델은 데이터를 보호하기 위한 스레드 안전한 방식을 제공한다.
- Actor는 내부 데이터에 대한 접근을 단일 스레드에서만 처리하도록 제한해, 여러 스레드가 동시에 데이터에 접근하지 못하도록 한다.
  - value 프로퍼티는 Actor가 관리하여, 동시에 접근하는 Race Condition을 방지할 수 있다.

``` swift
actor Counter {
    private var value = 0

    func increment() {
        value += 1
    }

    func getValue() -> Int {
        return value
    }
}

let counter = Counter()

Task {
    await counter.increment()
    print(await counter.getValue())
}
```


<br/>

## ✏️ 메인 스레드에서 UI 업데이트를 해야 하는 이유는 무엇인가요?

---

- UIKit가 <span style="color:#9fb584">**메인 스레드에서의 작업을 전제로 설계**</span>되었기 때문이다.
- <span style="color:#9fb584">**메인 스레드를 UI요소와 이벤트 처리의 전담 스레드로 사용하며, 이를 통해 UI 업데이트가 일관되고 안정적으로 이뤄지도록 보장**</span>한다.

### 이유 1. UIKit는 메인 스레드에서만 안전하게 작동

- UIKit 프레임워크의 모든 UI 컴포넌트(ex. UIView, UILabel, UIButton)는 메인 스레드에서 안전하게 작동되도록 설계되었다.
- 메인 스레드 이외의 스레드에서 UI를 업데이트하면 UIKit의 상태가 예기치 않게 변경될 수 있어 **앱이 비정상적으로 동작하거나 충돌이 발생**할 수 있다.

### 이유 2. 사용자 경험의 일관성 유지

- 메인 스레드는 UI와 사용자 이벤트 처리를 전담하므로, UI 업데이트가 메인 스레드에서 이뤄져야 사용자가 보는 화면과 상호작용이 일관성을 유지한다.
- 만약 메인 스레드 외부에서 UI를 업데이트한다면, 다른 스레드의 작업 타이밍에 따라 UI가 엉뚱한 시점에 업데이트되거나 화면 깜빡임이 발생할 수 있다.

### 이유 3. 데이터 레이스 및 동기화 문제 방지

- <span style="color:#9fb584">**UI 업데이트를 여러 스레드에서 동시에 수행하면 Race Condition이 발생**</span>할 수 있다.
- 메인 스레드가 아닌 곳에서 UI를 변경하면 Race Condition이 발생할 가능성이 높아진다.
- 메인 스레드에서만 UI 업데이트를 처리하면 이러한 동기화 문제를 예방할 수 있다.


<br/>
<br/>


[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source