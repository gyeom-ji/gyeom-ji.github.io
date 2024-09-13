---
title: "메모리 관리 기법"
author: gyeomji
date: 2024-05-22 10:00:00 +0900
categories: [Operating System]
tags: [Memory, Swapping, Dynamic Loading, Dynamic Linking, Fixed Partitioning, Variable(Dynamic) Partitioning]
pin: false
math: true
mermaid: true
---


## 메모리 관리

---

- 운영체제의 핵심 기능 중 하나로, 프로그램이 실행되려면 주기억장치에 적재되어야 하기 때문에 <span style="color:#9fb584">**제한된 주기억장치의 효율적인 사용(할당)이 목적**</span>이다.
  - CPU는 main memory와 CPU 내의 register만을 직접 접근한다.
  - <span style="color:#9fb584">**가능한 많은 프로세스를 적재하는 효율적인 메모리 할당**</span> 해야 한다.
  - 너무 적은 수의 프로세스는 CPU를 `유휴(idle)` 상태로 만들 수 있다.
    > 유후 idle : 컴퓨터 시스템이 사용 가능한 상태이나 실질적인 작업이 없는 시간.
  - 너무 많은 수의 프로세스는 빈번한 `Swapping`을 야기할 수 있다.
- 메모리 관리는 OS(Memory Manager)에 의해 수행되며 하드웨어(MMU)의 지원이 필요하다.
  - <span style="color:#9fb584">**효율적인 메모리 참조(논리 <-> 물리 주소 변환)**</span> 관리가 필요하다.
  - 프로세스가 메모리 주소를 직접 참조하는 것이 아닌<span style="color:#9fb584">**프로세스의 주소 공간과 메모리의 주소 공간을 구분**</span>하고 있기 때문에 빠르게 메모리 주소를 참조할 수 있는 관리 방법이 필요하다.
- 프로그램의 <span style="color:#9fb584">**실행이 종료될 때까지 메모리를 가용한 상태로 유지 및 관리하는 기능**</span>이다.
  - 프로그램 실행 중 메모리가 꽉 차게 되면 시스템의 속도가 느려지고 멈추는 현상이 발생한다.

<br/>

> iOS에서의 메모리 관리는 [ARC(Automatic Reference Counting)](https://gyeom-ji.github.io/posts/arc/)에 의해 수행되며, 메모리 관리 방법으로 Reference Counting을 사용한다.

<br/>

## 메모리 관리 기법


### 1. 동적 적재 Dynamic Loading

---

- 프로그램이 <span style="color:#9fb584">**실행하는데 반드시 필요한 루틴, 데이터만 적재**</span> 하는 것이다.
  - 프로그램의 전체 코드에서 모든 루틴이 다 사용되는 것은 아니다.
  - 대표적으로 오류 처리 구문이 있다. (if문과 같이 오류가 발생할 때만 해당 내부 코드가 실행되는 것)
  - 동적 적재를 수행하면 프로그램의 실제 메모리에는 오류 구문을 제외하고 적재한다.
  - 오류가 발생하면 그 때 해당 오류 구문을 찾아 메모리에 올린다.
  - 데이터 또한 모든 데이터가 반드시 사용되는 것이 아니기 때문에 <span style="color:#9fb584">**필요한 부분만 메모리에 올리고 실행 도중 필요할 때마다 해당 부분을 찾아 메모리에 올린다.**</span>
- 반대로 모든 루틴과 데이터를 적재하는 것을 정적 적재 Static Loading 이라 한다.
  - 현대 운영체제는 대부분 동적 적재 사용


### 2. 동적 연결 Dynamic Linking

---

- 여러 프로그램에 <span style="color:#9fb584">**공통으로 사용되는 라이브러리를 중복으로 메모리에 올리는 것이 아니라 하나만 올려 공유**</span>하는 것이다.
- 메모리를 효율적으로 사용할 수 있다.
- 앱이 Link 되는 방식에 따라 Static Libary와 Dynamic Libary로 나뉜다.
- Static Library : ".a" 접미사를 가진다.
- Dynamic Libary : "dylib" 접미사를 가진다.


> Library : Xcode Target의 일부로 빌드되지 않은 코드 및 데이터 조각을 정의한 것으로 다른 프로젝트에서 재사용 할 수 있는 코드 및 리소스 모음을 나타낸다. 특정 기능을 제공하거나 일반적인 프로그램이 문제를 해결하는 미리 작성된 함수, 클래스 및 기타 구성 요소가 포함되어 있다. <br/>
> Link : 라이브러리와 앱의 소스코드 파일을 병합하는 프로세스

<br/>

#### Static Library

---

- <span style="color:#9fb584">**많은 Static Library를 앱에 링크하면 큰 Excutable File이 생성**</span>된다.
  - Excutable File 내부에 Static Library 코드가 복사되기 때문이다.
  - 동일한 Static Library를 여러 프레임워크 또는 프로그램에서 사용할 경우 <span style="color:#9fb584">**메모리에 중복되어 적재**</span>된다.
  - 큰 Excutable File은<span style="color:#9fb584">**느린 시작 시간(Launch Time)과 큰 메모리 공간을 차지**</span>한다.
- 앱 코드 내 <span style="color:#9fb584">**Heap 영역에 상주**</span>한다.
- 빌드 속도가 느리지만, 런타임 속도가 빠르다.
- Static Library가 업데이트되면 클라이언트 앱은 개발자가 <span style="color:#9fb584">**업데이트된 라이브러리와 다시 링크하지 않는 이상 업데이트된 기능을 사용할 수 없다.**</span>

<br/>

##### [ 동작 과정 ]

1. 앱에 Static Library를 추가한다.
2. <span style="color:#9fb584">**컴파일 타임에 Static Linker가 Library코드와 앱 코드를 합병**</span>한다.
3. 단일 Excutable File을 만든다.
4. 앱이 실행되면 Library 코드와 앱 코드가 앱 주소공간에 로드된다.

<br/>

![staticLibrary](/assets/img/staticLibrary.png){: width="500" height="500"}

- Static Library는 빨간 네모 안의 Static Libraries에 위치하고, 이를 사용하려면 Link 작업이 필요하다.
- 이 때 Static Linker를 이용해 앱의 소스 파일과 Static Library들을 병합하게 된다.
- Linker는 input으로 object 파일, Libraries(.dylib, .tbd, .a) 파일을 받게 되고, 이 파일을 받아 단일 파일로 결합한다.

![staticLibrary3](/assets/img/staticLibrary3.png){: width="500" height="500"}

- link 과정은 컴파일 마지막에 일어나기 때문에 excutable file을 생성한다고 보면 된다.
- 앱의 <span style="color:#9fb584">**소스코드와  Static Libraries 코드가 Excutable File 내부에 복사**</span>된다.
- Static Library가 Excutable File의 일부가 되기 때문에 앱이 실행되면 앱 소스코드와 Static Library 코드를 포함하는 것들이 앱의 주소공간에 로드된다.

<br/>

#### Dynamic Library

---

- 동시에 여러 프레임워크 또는 프로그램에서 <span style="color:#9fb584">**동일한 코드 사본을 공유하기 때문에 메모리를 효율적으로 사용**</span>한다.
- <span style="color:#9fb584">**동적으로 연결되어 있으므로 전체 빌드를 다시 하지 않아도 새로운 프레임워크 사용이 가능**</span>하다.
- Static Library에 비해 <span style="color:#9fb584">**메모리를 적게 사용**</span>한다.
- Dynamic Library를 로드하는 시간이 있고 이는 pre-main 시간을 늘리기 때문에 <span style="color:#9fb584">**Launch Time이 오래걸린다.**</span>
  - Launch Time을 개선하고 싶을 경우 Dynamic Library를 적게 embed 해야한다.
- 동적 연결은 같은 라이브러리가 중복으로 메모리에 올라가는 것을 방지하기 위해 <span style="color:#9fb584">**프로그램이 메모리에 적재된 후 링크 Link 작업을 수행**</span>한다.
  - 정적 연결은 실행 파일이 만들어지기 전 링크 과정을 수행한다.

<br/>

##### [ 동작 과정 ]

1. 앱을 실행한다.
2. 앱 코드와 Dynamic Library에 대한 <span style="color:#9fb584">**참조가 앱 주소공간에 로드**</span>된다.
3. 참조를 이용해 Dynamic Library를 로드한다.
  - Dynamic Library가 많을수록 pre-main time(main()이 실행되기 전까지의 시간)이 늘어나고, 이 시간이 증가할수록 앱 Launch Time이 길어진다.


<br/>

![dynamicLibrary](/assets/img/dynamicLibrary.png){: width="500" height="500"}

- Dynamic Library에 대한 <span style="color:#9fb584">**참조만 Excutable File에 포함**</span>된다.
  - Dynamic Library에서 제공하는 함수, 변수, 리소스 등 찾고 사용하는 방법에 대한 정보는 포함되어 있지만 실제 구현 코드는 포함하지 않는다.
- Excutable File과 Link되었지만 복사가 아닌 참조를 하고 있기 때문에 Excutable File의 크기가 Static에 비하여 작다.
- Dynamic Library는 앱이 실행될 때 앱 주소 공간에 로드된다.


<br/>

> Dynamic Library의 Executable 파일에는 Dynamic library에 정의된 기능 및 리소스에 대한 place holder역할을 하는 symbol, identifier가 포함되어있다. 이는 라이브러리에서 필요한 구성요소의 이름과 위치를 지정한다. 런타임에 OS의 Linker는 이런 참조를 이용하여 Dynamic Library에서 해당 File을 찾아 메모리에 Load하게 된다. <span style="color:#9fb584">**참조만을 포함하므로 Executable File은 전체 라이브러리 코드를 포함하지 않기 때문에 Static 링크에 비해 크기가 더 작게 유지**</span>된다. 이를 통해 시스템 리소스를 보다 <span style="color:#9fb584">**효율적으로 사용**</span>할 수 있고 여러 실행 파일이 <span style="color:#9fb584">**동일한 Dynamic Library를 공유**</span>할 수 있는 모듈식 개발, 배포가 용이해진다.

<br/>
<br/>

### 스와핑 Swapping

---

CPU에서 <span style="color:#9fb584">**시행되지 않는 프로세스 즉, ready 상태이거나 waiting 상태에 있는 프로세스들 중 일부를 메모리에 보관하지 않고 하드디스크에 보관**</span>한다. 하드디스크로 보관한다고 해서 원래 프로세스의 상태가 아닌 프로그램의 상태로 다시 되돌아온다는 뜻이 아니다. <span style="color:#9fb584">**프로그램은 별도의 파일시스템 영역에 남아있고, 메모리 상태에 존재하고 있던 프로세스의 이미지 즉 텍스트, 데이터, 힙, 스텍 등으로 구별되어 있는 상태 그대로를 하드디스크에 저장**</span>하는것이다. 다음에 ready 상태이거나 waiting상태인 프로세스가 running 상태로 스케줄링 됐을 때 하드디스크에 저장되어 있던 <span style="color:#9fb584">**데이터를 새로 적재하는 로딩 과정을 거치는 것이 아닌, 이미지만 그대로 메모리에 대해 복사하면 스케줄링 되어 CPU에서 시행**</span>될 수 있도록 하는 것이다. 이 방법은 가상메모리 기법의 핵심이 된다. 이와 같이 <span style="color:#9fb584">**스와핑은 각각의 프로세스에 어떤 스케줄링 상태에 의존해서 동작**</span>하게 된다.



![scheduler](/assets/img/scheduler.jpeg){: width="500" height="500"}

CPU에서 실행 하다가 입출력 요청이나 인터럽트를 받게 되면 ready queue 또는 I/O waiting queue 들어가서 상태가 바뀌게 된다. 스와핑을 고려하게 되면 <span style="color:#9fb584">**ready queue 상태로 들어가는 것이 아니라 swap out되면서 하드디스크에 저장**</span>된다. 즉, CPU실행이 끝난 후 다시 실행 상태로 들어오기 위해서는 앞에 있는 여러개의 프로세스가 실행을 마쳐야 하므로(시간이 오래 걸릴 것이라 예상) 메모리(ready queue)에 저장하는 것이 아닌 하드디스크에 저장하는 방식으로 스케줄링을 할 수 있다. 

<br/>

- 스왑 영역 Swap Space : 프로세스 이미지가 내려가는 하드디스크의 일부 영역
- Swap-In : Swap Space의 프로세스 이미지를 메모리로 Load하는 것
- Swap-Out : ready, waiting 상태인 프로세스 중 하나를 메모리에서 Swap Space(backing store)로 내리는 것
- Swap-Out 된 프로세스는 다시 Swap-In 을 할 때 <span style="color:#9fb584">**이전의 메모리 주소 공간이 아닌 새로운 주소 공간으로 갈 수도 있다.**</span>
  - 해당 프로세스가 스왑 영역에 있는 동안 다른 프로세스가 해당 주소 공간을 사용할 수 있기 때문이다.
  - <span style="color:#9fb584">**MMU 의 재배치 레지스터로 인해 어디에 적재되는지 상관없이 정상적으로 수행**</span>할 수 있다.
- 프로세스의 크기가 커지고 하드디스크는 메인 메모리보다 속도 면에서 느리기 때문에 Swapping 동작의 오버헤드는 크다고 볼 수 있다.
- victim process의 선정 방법이다.
- 실행시간 주소 바인딩이 요구된다.
- 문맥 교환 시간이 많이 증가한다.

> 스왑되어 들어오는 주소 = <span style="color:#9fb584">**실행 시간 주소 바인딩**</span><br/>
> 프로세스는 프로세스의 이미지 형태로 내려갔지만, 메모리상에 존재하는 데이터가 아니기 때문에 다시 메모리상에 올라올 때 주소의 바인딩이 일어나야 한다. 즉, <span style="color:#9fb584">**실행 시간에 동적으로 주소 바인딩**</span>이 일어나야 한다.

<br/>

#### 문맥 교환 Context Switch

---

- 메인 메모리에서 일어나는 것이 아닌 <span style="color:#9fb584">**CPU의 레지스터에서 일어나는 과정**</span>이다.
- <span style="color:#9fb584">**하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해 이전의 프로세스 상태(문맥)을 보관하고 새로운 프로세스의 상태를 적재**</span>하는 작업이다.
- CPU가 새로운 프로세스로 전환될 때 kernel은 이전 프로세스 상태를 저장하고, 새 프로세스를 위해 저장된 상태를 로드한다.
- CPU를 선점하고 있던 프로세스는 프로세스 문맥을 PCB에 저장하고, 새롭게 CPU를 할당받을 프로세스는 PCB로부터 예전에 저장했던 자신의 상태를 실제 하드웨어로 복원한다.
- <span style="color:#9fb584">**문맥 교환 중에는 CPU가 다른 작업을 할 수 없기 때문에 문맥 교환에 쇼요되는 시간을 오버헤드**</span>라한다.
  - [ 오버헤드 해결 방안 ]<br/>
    1) 문맥 교환이 자주 발생하지 않도록 다중 프로그래밍의 정도를 낮춘다.<br/>
    2) 스택 중심 장비에서는 Stack 포인터 레지스터를 변경하여 프로세스간 문맥 교환을 수행한다.<br/>
    3) 스레드를 이용하여 문맥 교환 부화를 최소화시킨다.

<br/>

> 프로세스 제어 블록 PCB(Process Control Block) : OS가 시스템 내의 프로세스들을 관리하기 위해 프로세스마다 유지하는 정보들을 담는 커널 내 자료구조이다. 커널 주소 공간의 Data 영역에 존재한다.<br/><br/>
> [ PCB에 저장되는 내용 ]<br/>
> - Process 상태 : CPU를 할당해도 되는지 여부를 결정<br/>
> - PC값 : 다음에 수행할 명령어의 위치를 가르킴<br/>
> - CPU Register : CPU 연산을 위해 현 시점에 레지스터에 어떤 값을 저장하고 있는지 나타냄<br/>
> - CPU 스케줄링 정보<br/>
> - 메모리 관리 정보<br/>
> - 자원 사용 정보<br/>
> - 입출력 상태 정보 <br/>

<br/>

##### 문맥 교환 시점

1. 멀티 태스킹
  - 다수의 프로세스가 하나의 CPU자원을 나눠서 사용하는 것으로 실행 가능한 프로세스들이 OS의 스케줄러에 의해 조금씩 번갈아 수행된다.
  - <span style="color:#9fb584">**CPU를 할당받을 때 문맥 교환**</span>이 일어난다.
  - 문맥 교환이 빠르게 일어나기 때문에 한 CPU에서 여러개의 프로세스가 동시에 수행되는 것처럼 보인다.
2. 인터럽트 처리
  - 컴퓨터 시스템에서 예외 상황이 발생했을 때 CPU에게 알려 처리할 수 있도록 하는것이다.
  - <span style="color:#9fb584">**인터럽트가 발생할 때 문맥 교환**</span>이 일어난다.
  - I/O request(입출력 요청), time slice expried(CPU 사용시간 만료), fork a child(자식 프로세스 생성), wait for an interrupt(인터럽트 처리 대기)
3. 사용자 및 커널 모드 전환
  - OS에서 <span style="color:#9fb584">**사용자 모드와 커널 모드 사이 전환이 필요할 때 OS에 따라 문맥 교환이 발생**</span>한다.

<br/>

##### 문맥 교환 과정

![contextSwitch](/assets/img/contextSwitch.png){: width="500" height="500"}

1. 요청 발생 
  - 인터럽트 또는 트랩에 의한 요청이 발생한다. 
  > 트랩 : 소프트웨어 인터럽트
2. PCB에 저장
  - OS는 <span style="color:#9fb584">**현재 실행중인 프로세스 P0의 정보를 PCB에 저장**</span>한다.
3. CPU 할당
  - OS는 <span style="color:#9fb584">**다음 프로세스 P1의 정보를 PCB에서 가져와 CPU를 할당**</span>한다.


<br/>

#### 스와핑의 문제점 - 문맥 교환 Context Switch 시간 증가


- 스와핑은 하드디스크를 활용하여 큰 메모리가 있는 것처럼 메모리를 효율적으로 사용할 수 있는 메모리 관리기법이지만 <span style="color:#9fb584">**문맥 교환 시간이 많이 증가**</span>한다는 문제가 있다. 
  - 메모리의 동작 속도와 저장 장치의 동작 속도가 매우 큰 차이를 가지고 있기 때문이다. 
  - 100MB 프로세스가 50MB/s 인 HDD에 스왑을 하면 2초가 걸릴 경우, 메모리상에서 같은 양의 데이터를 읽고 쓰고 처리하는데 밀리sec ~ 마이크로sec 정도의 시간이 걸리기 때문에 SSD라고 할지라도 1000 배 ~ 10만 배 가까운 속도 차이가 발생한다. 
- <span style="color:#9fb584">**문맥 교환은 오버헤드를 야기**</span>하여 프로그램의 실행속도를 늦추는데, 이 문맥교환에 <span style="color:#9fb584">**스와핑 동작까지 포함된다면 프로그램의 실행이 더욱 느려진다.**</span>
- 스와핑을 구현할 때는 <span style="color:#9fb584">**디스크 내에 별도의 스왑 공간을 사용하거나 (일반 파일 시스템과 분리) 실제로 사용하는 부분만 스왑하도록 최적화가 필요**</span>하다.

<br/>
<br/>


### MMU (Memory Management Unit) 

---

<span style="color:#9fb584">**CPU가 메모리에 접근하는 것을 관리하는 컴퓨터 하드웨어 부품으로 가상 메모리 주소를 실 메모리 주소로 변환하는 장치**</span>이다. 하드웨어 장치인 MMU를 사용하는 이유는 소프트웨어 방식보다 하드웨어 장치가 주소 변환이 더 빠르기 때문이다.

- 가상(논리) 주소 : 프로세스가 참조하는 주소
- 물리 주소 : 실제 메모리 RAM 주소


![mmu](/assets/img/mmu.png)


<br/> 
<br/> 


###  기억장치관리 요구 사항

---

#### 1. 재배치(relocation)를 지원해야 한다.

- 프로그램은 (최초 적재, swapping 시) <span style="color:#9fb584">**임의 위치에 적재 가능**</span>해야한다.
  - <span style="color:#9fb584">**상대 주소가 있는 재배치 가능한 적재 모듈**</span>
  - 일이 메모리의 어느 위치에 로드되는지 알 수 없기 때문에 임의의 주소에서도 문제없이 수행 가능하도록 하는 것이다.
    - 일이 항상 0번지에 로드된다고 가정하고 주소 값(논리 주소)을 계산한다.
    - 실제 메모리에 로딩될 때는 베이스 레지스터(relocation(base) register)에 저장된 값을 더하여 물리 주소를 계산한다.
    - 메모리 보호와 메모리 재배치를 성공적으로 수행하기 위해 MMU가 서포트 한다.
- <span style="color:#9fb584">**동적 주소 바인딩이 요구**</span>된다.
  - <span style="color:#9fb584">**주소 바인딩 : 논리 주소 ➡️ 물리 주소**</span>
  - 초기 적재 시간(load time)에 Relocating loader에 의하여
  - 실행 시간(excution time)에  MMU에 의하여

![dynamicRelocationUsingRelocationRegister](/assets/img/dynamicRelocationUsingRelocationRegister.png)


#### 2. 보호(protection)를 지원해야 한다.

- 프로세스는 오직 <span style="color:#9fb584">**자신의 주소공간(Address Space)만 접근**</span> 해야한다.
  - 프로세스들은 기본적으로 <span style="color:#9fb584">**상호 독립적**</span>이기 때문이다.

![precossAddressSpace](/assets/img/precossAddressSpace.png)

- 주소검사는 <span style="color:#9fb584">**실행 중 H/W**</span>에 의해 수행된다.
  - 어떤 일이 다른 일이나 OS 영역을 침범하여 발생하는 문제를 막는 것이다.
    - 일이 사용하는 메모리의 시작 주소를 base register에 저장한다.
    - 일이 사용하는 메모리의 크기를 경계 레지스터(limit register)에 저장한다.
    - 현재 접근하려는 주소의 값이 베이스 값과 베이스 + 경계 값 사이에 있는지 확인한다.
  - 프로그램은 실행 중 재배치가 가능하므로 컴파일 시 불법 주소 탐지는 불가능하다.

![hardwareAddressProtectionWithBaseAndLimitRegisters](/assets/img/hardwareAddressProtectionWithBaseAndLimitRegisters.png)

#### 3. 공유(sharing)를 지원해야 한다.

- 공유 자원(shared data, shared library) 접근이 가능해야 한다.

#### 4. 프로그램의 논리적 구조를 지원해야 한다.

- 하나의 프로그램은 여러개의 모듈로 구성된다.
  - ex) code module, library routine, data module
- OS와 하드웨어는 프로그램 모듈을 지원해야한다.
  - 모듈 유형에 따라 다른 보호 기법, 공유 등의 지원
- Segmentation 기법이 프로그램 모듈을 잘 지원한다.


#### 5. 기억장치의 물리적 구조를 지원해야 한다.

- Memory Hierarchy : Registers — Cache memory — Main memory — 보조기억장치
- <span style="color:#9fb584">**주기억장치와 보조기억장치 간 정보(process, page, segment) 이동을 관리**</span>해야 한다. (메모리 관리의 핵심)


<br/>
<br/>

###  3. 연속 메모리 할당

---

- 각 프로세스를 하나의 <span style="color:#9fb584">**연속된 메모리 공간에 저장**</span>하는 방법이다.
  - 논리 주소 : CPU가 생성하는 주소
  - 물리 주소 : MMU에 의해 일련의 변환 과정을 거친 실제 메모리 주소
![logicalPhysicalAddress](/assets/img/logicalPhysicalAddress.png)


- <span style="color:#9fb584">**디스크에 있는 프로세스를 주기억장치의 어느 위치에 저장할 것인지 결정**</span>한다.

<br/>

#### MMU의 메모리 보호 / 재배치 

![logicalPhysicalAddress](/assets/img/mmuLimitRelocationRegister.png)

- CPU 스케줄링 시 Dispatcher는 문맥 교환을 수행하며 이때 두 register의 값을 설정한다.

<br/>


#### 1) 고정 분할 기법 Fixed Partitioning

- <span style="color:#9fb584">**프로세스의 크가와 상관 없이 메모리를 여러 개의 파티션으로 나눠 관리**</span>하는 방식이다.
- <span style="color:#9fb584">**각 파티션에 하나의 프로세스를 저장**</span>한다.
- 파티션의 개수 = Multiprogramming Degree
  > 멀티프로그래밍 차수 Multiprogramming Degree : 현재 메인 메모리에 존재하는 활성화된 일(active job)의 개수 (수행 시작은 했지만 종료는 되지 않는 것)
- <span style="color:#9fb584">**메모리를 일정한 크기로 나눠 관리하기 때문에 메모리 관리가 수월**</span>하다.
  - 메모리 통합과 같은 부가적인 작업이 필요없다.
  - 프로그램이 적재되고 남은 공간에 다른 프로그램을 적재하여 수행하므로 프로세서와 기억장치 같은 자원의 활용도를 향상시킨다.
  - 동시에 여러 프로그램을 주기억장치에 적재하여 수행하는 <span style="color:#9fb584">**다중 프로그래밍 기법이 가능**</span>하다.
- 메모리가 미리 나눠져 있기 때문에 융통성이 없고 <span style="color:#9fb584">**내부 단편화가 발생**</span>한다.
  - 일정하게 나눠진 공간보다 작은 프로세스가 올라올 경우 메모리 낭비가 발생한다.
- Equal-size, Unequal-size partitioning이 있다.
- Equal-size partitioning : First available partition 선택
- Unequal-size partitioning : Best-fit partition 선택

<br/>
<br/>

#### 2) 가변 분할 기법 Variable(Dynamic) Partitioning

- 시작시, 모든 메모리가 가용한다. (하나의 큰 파티션)
- <span style="color:#9fb584">**프로세스의 크기에 따라 요구된 만큼 메모리를 분할하여 할당**</span>한다.
  - 프로세스를 한 덩어리로 처리하여 하나의 프로세스를 연속된 메모리 공간에 적재한다.
  - 주기억장치 내에 새로운 프로그램이 들어올 때마다 그 프로그램의 크기에 맞춰 <span style="color:#9fb584">**가변적으로 기억 공간을 분할**</span>하여 프로그램에 맞는 공간만을 할당한다.
- <span style="color:#9fb584">**메모리 통합 등의 부가적인 작업이 필요하기 때문에 메모리 관리가 복잡**</span>하다.
  - 파티션 반환시 인접한 빈 파티션들을 합쳐야한다.(Merging)

![variablePartitioning](/assets/img/variablePartitioning.png)

<br/>

#### (1) 최초 적합 First-fit

- 프로세스가 적재될 수 있는 <span style="color:#9fb584">**가용 공간 중 첫 번째 분할에 할당**</span>하는 방식이다.
- 검색의 시작점은 알고리즘에 따라 다르며 검색 속도가 빠르다.


#### (2) 최적 적합 Best-fit

- <span style="color:#9fb584">**가용 공간 중에서 가장 크기가 비슷한 공간을 선택**</span>하여 프로세스를 적재하는 방식이다.
- 공백이 최소화라는 장점이 있다.


#### (3) 최악 적합 Worst-fit

- 프로세스의 <span style="color:#9fb584">**가용 공간 중 가장 큰 공간에 할당**</span>하는 방식이다.
- 분할되어 남는 가용 공간이 크기 때문에 활용 가능성이 높다.
- 검색 속도가 느리고 메모리 이용 효율이 좋지 않다.

![memoryFit](/assets/img/memoryFit.png)

<br/>


#### 연속 메모리 할당의 문제점 - 메모리 단편화 Memory Fragmentation

![hole](/assets/img/hole.png)

- 분할된 주기억장치에 프로세스를 할당, 반납하는 과정에서 사용되지 못하고 <span style="color:#9fb584">**낭비되는 기억장치가 발생**</span>하는 현상이다.
- 부팅 직후 메모리는 운영체제만 할당되고 비어있는 상태이다.
  - 이 빈 공간을 Hole이라 부른다.
  - 즉, 부팅 직후에는 운영체제와 Big Single Hole이 있는 상태이다.
- 시간이 지나면서 프로세스가 생성, 종료를 반복하며 여러 곳에 서로 다른 크기의 홀이 존재하게 된다.
  - 이와 같이 Hole들이 불연속하게 흩어져 있는 상태를 메모리 단편화라고 한다.
- 내부 단편화와 외부 단편화가 있다.

<br/>

#### 내부 단편화

- 분할된 공간에 <span style="color:#9fb584">**프로세스를 적재한 후 남은 공간**</span>이다.
- <span style="color:#9fb584">**고정 분할 할당 방식**</span> 사용 시 발생한다.

<br/>

#### 외부 단편화

- 할당된 크기가 <span style="color:#9fb584">**프로세스 크기보다 작아 사용하지 못하는 공간**</span>이다.
- <span style="color:#9fb584">**가변 분할 할당 방식**</span> 사용시 발생한다.

<br/>

#### 단편화 해결 방법 

- 페이징 Paging
- 세그멘테이션 Segmentation
- 통합 Coalescing
  - 인접한 단편화 영역을 찾아 하나로 통합하는 기법이다.
- 압축 Compaction
  - 주기억장치를 검사하여 모든 단편화 영역(Hole)을 하나의 커다란 Hole으로 만드는 방법이다.
  - 운영체제는 사용중인 블록을 한데 모으고, 비어있는 기억장소를 하나의 커다란 공백으로 만든다.
  - 기억 장소에 분산되었던 공간들을 한 곳에 모음으로써 사용 가능한 큰 영역을 만들 수 있다.
    - 이를 통해 기억 장소의 낭비를 줄일 수 있다.
  - Hole을 옮기는 오버헤드가 크고 어느 Hole을 옮겨야 빠르게 합칠 수 있는지에 대한 최적 알고리즘이 존재하지 않는다는 단점이 있다.
    - 기억장소를 집약하는 동안 전체 시스템은 지금까지 수행해 오던 일들을 일단 중지해야 하며, 집약을 위하여 많은 시간이 소모된다.
    - 수행 중이던 프로그램과 데이터를 주기억장치 내의 다른 장소로 이동시키기 때문에 각각의 위치 및 이에 관계되는 내용을 수정(주소 관련)해야 한다.

  ![memoryCompaction](/assets/img/memoryCompaction.png)




<br/>
<br/> 

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source