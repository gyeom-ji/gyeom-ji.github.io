---
title: "마이크로 아키텍처"
author: gyeomji
date: 2024-07-12 10:00:00 +0900
categories: [Computer Architecture]
tags: [Computer Architecture]
pin: false
math: true
mermaid: true
---

<br/> 

## 컴퓨터 아키텍처

---

- 컴퓨터를 구성하는 하드웨어와 소프트웨어를 포함한 컴퓨터 시스템 전체의 구조와 설계 모형을 의미한다.
- 하드웨어와 소프트웨어의 상호작용에 대한 설계와 구현을 포함한다.
- 시스템 성능, 신뢰성, 에너지 효율성 등을 결정하는데 중요한 역할을 한다.
- 프로세서와 메모리 간 데이터 전송 방식, 명령어 집합 구성, 파이프라인 구조, 캐시 구조 등 다양한 요소들을 고려하여 설계된다.

<br/> 

## 마이크로프로세서

---

- 제어장치, 산술 논리 장치, 레지스터가 하나의 대규모 집적회로 IC칩에 내장된 것이다.
  - CPU 뿐만 아니라 GPU, DSP도 포함된다.
  - 개인용 PC에서 중앙처리장치로 사용된다.
- 설계 방식에 따라 RISC와 CISC로 구별한다.

<br/>

## 마이크로 아키텍처 Micro Architecture

---

- CPU의 하드웨어적 설계를 말한다.
- 정의된 명령어 집합을 효율적으로 처리할 수 있도록 CPU 회로를 설계한다.
- CPU의 설계와 명령어 집합(ISA)을 정의하는 방식이다.
- 아키텍처란 다른 하드웨어와 소프트웨어 간의 상호 작용을 가능하게 하는 인터페이스이다.
- CPU의 내부 구성 요소, 데이터 버스, 레지스터, ISA를 포함하는 개념이다.

> CPU 아키텍처 : 하드웨어 시스템의 전반적인 구조와 동작을 나타내는 개념<br/>
> ISA : CPU가 이해하고 실행할 수 있는 명령어 집합을 정의하는 개념


<br/> 

## 폰 노이만 구조

---

![vonNeumannArchitecture](/assets/img/vonNeumannArchitecture.png)

- 내장 프로그램(stored-program) 방식 컴퓨터이다.
  - 컴퓨터에 여러 목적의 작업을 시킬 때 하드웨어를 재배치할 필요 없이 소프트웨어(프로그램)만 교체하면 되기 때문에 편의성이 증가했다.
  - 다양한 목적으로 사용 가능해져 범용성이 향상되었다.
  - 현재 거의 모든 컴퓨터가 폰 노이만 구조를 따르는 이유이다.
- 메모리, 입출력장치가 CPU를 중심으로 연결되어 있는 구조이다.
- CPU와 메모리는 서로 분리되어 있고 둘을 연결하는 버스를 통해 명령어 읽기, 데이터 읽고 쓰기가 가능하다.
  - 메모리 안에 프로그램과 데이터 영역은 물리적 구분이 없기 때문에 명령어와 데이터가 같은 메모리, 버스를 사용한다.
    - 프로그램과 데이터가 같은 메모리 공간에서 저장되어 처리된다.
  - CPU가 명령어와 데이터에 동시 접근할 수 없다.
  - 명령어와 데이터를 구분하기 위한 별도의 제어 신호가 필요하지 않다.
- CPU는 메모리에 저장된 프로그램을 실행하며, 프로그램에서 필요한 데이터를 메모리에서 가져온다. 
  - CPU가 메모리와 입출력장치에 직접 접근할 수 있기 때문에 처리 속도가 빠르다.
- CPU는 메모리에서 순차적으로 명령어를 하나씩 읽어와 실행한다.
  - 순차적으로 프로그램을 처리하므로 메모리나 시스템 버스에 병목 현상이 생겨 속도가 느려진다.
  - CPU는 명령어와 데이터를 동시에 처리할 수 없기 때문에 CPU가 명령어를 실행하는 동안에는 데이터 처리가 이뤄지지 않는다.
- 기억장치의 속도가 전체 시스템의 성능 저하를 야기하는 현상을 폰 노이만 병목현상이라 한다.
  - RAM과 CPU의 속도 차이로 인해 CPU가 메모리가 응답하기까지 기다려야 하는 현상이 발생한다.

<br/>

## 하버드 구조

---

![harvardArchitecture](/assets/img/harvardArchitecture.png)

- 폰 노이만 구조의 병목 현상을 완화하기 위한 구조이다.
- 병목현상이 일어나는 근본적인 원인은 프로그램 메모리와 데이터 메모리가 물리적 구분 없이 하나의 버스를 통해 CPU와 교류하기 때문이다.
  - 이러한 구조에 의해 CPU는 명령어와 데이터에 동시 접근이 불가능하고 명령을 한 번에 하나씩 읽고 쓴다.
- 하버드 구조는 명령어와 데이터를 나눠 보관한다.
- CPU가 명령어와 데이터를 동시에 사용할 수 있도록 명령용 버스와 데이터용 버스를 물리적으로 구분한다.
- 명령을 읽음과 동시에 데이터를 저장할 수 있다.

<br/>


## 명령어 집합 구조 ISA (Instruction Set Architecture)

---

- CPU가 이해하고 실행할 수 있는 명령어 집합이다.
- CPU마다 ISA가 다를 수 있다.
- 이 명령어 집합들은 OS와 어플리케이션에 사용된다.
- ISA의 종류는 x86, AMD64(x86-64), ARM, MIPS, AVR 등이 있다.
  - 컴퓨터 마다 필요한 연산 능력과 컴퓨팅 환경이 다르기 때문에 다양한 ISA가 개발되고, 사용된다.
- CPU가 이해하는 명령어가 달라지면 제어장치가 명령어를 해석하는 방식, 사용되는 레지스터의 종류와 개수, 메모리 관리 방법 등 많은 것이 달라진다.
  - 이는 CPU 하드웨어 설계에도 큰 영향을 미친다.
- ISA는 CPU의 언어임과 동시에 CPU를 비롯한 하드웨어가 소프트웨어를 어떻게 이해할지에 대한 약속이라 볼 수 있다.

> ISA가 다르다는 건 CPU가 이해할 수 있는 명령어가 다르다는 뜻이다. 같은 소스 코드로 만들어진 프로그램이라 할지라도 ISA가 다르면 CPU가 이해할 수 있는 명령어, 어셈블리어가 달라진다. ISA가 같은 CPU들은 서로 명령어를 이해할 수 있지만 ISA가 다르면 서로의 명령어를 이해할 수 없다. (ISA는 일종의 CPU언어라고 볼 수 있다) 인텔 계열 프로세서와 AMD 계열 프로세서는 내부 구조가 다르지만 같은 언어, 즉 같은 ISA(x86)을 사용하기 때문에 문제 없이 프로그램이 작동된다. ISA가 다른 프로세서일 경우 x86 ISA로 만들어진 프로그램은 바로 작동할 수 없다.

<br/>

### CISC (Complex Instruction Set Computer)

- 복잡한 구성의 명령어 셋팅을 가지는 CPU 아키텍처이다.
- 폰 노이만 구조로 구성된다.
- 가변 길이 명령어 형식이다.
- 다양한 주소 지정 모드가 있다.
- 명령어가 복잡하기 때문에 명령어를 해석하는데 시간이 오래 걸리며 명령어 해석에 필요한 회로가 복잡하다.
- 마이크로 프로그래밍(S/W) 제어 방식이다.
  - 명령어가 소프트웨어적이므로 호환성이 높다.
- 명령어 파이프라이닝이 불리하다.
  - 명령어의 크기와 실행되기까지 시간이 일정하지 않다.
  - 명령어 하나를 실행하는데 여러 클록 주기가 필요하다.
  - 대다수의 복잡한 명령어는 사용 빈도가 낮다.
- 메모리에서 오퍼랜드(피연산자)를 조작하는 명령어들이 많다.
- 컴파일 과정이 쉽고 호환성이 좋지만 속도가 느리다.
- 개인용 PC와 같이 고사양 작업을 필요로 하는 곳에 사용된다.
- CISC 명령어 집합을 가진 CPU 아키텍처 : AMD64, x86

<br/>

### RISC (Reduced Instruction Set Computer) 

- 간단한 구성의 명령어 셋팅을 가지는 CPU 아키텍처이다.
  - CISC의 많은 명령어와 주소 모드 중 실제로 사용되는 명령어는 몇 개 되지 않는다는 사실을 바탕으로 적은 수의 명령어만으로 명령어 집합을 구성했다.
  - CPU 명령어 개수를 줄여 명령어 해석시간을 줄임으로써 개별 명령어의 실행 속도를 빠르게 한다.
- 하버드(Harvard)에서 개발한 아키텍처로 하버드 구조를 사용한다.
- 단순하고 적은 수의 고정 길이 명령어와 주소 지정 모드를 사용한다.
  - 명령어의 개수가 적어 처리 속도가 빠르다.
  - 명령어 길이가 고정되어 있으므로 해석 속도가 빠르다.
- 논리회로를 이용한 하드웨어적 제어 방식이다.
  - 명령어가 하드웨어적이므로 호환성이 낮다.
- 명령어 파이프라이닝에 유리하다.
  - 한 클록 사이클에 하나의 명령어를 실행하기 때문에 파이프라인을 기다리게 하지 않는다.
- Load-Store 구조이다.
  - 데이터 처리를 레지스터 기준으로 수행한다.
  - 연산을 수행하기 위해 메모리에 직접 접근하는 명령어를 제한하고 대신 레지스터를 활용한다.
  - 메모리 접근은 store, load 명령어로 제한하여 불필요한 메모리 접근을 줄인다.
    - 메모리에 연산할 것이 있을 경우 load 명령어로 레지스터에게 전달
    - 레지스터에서 연산이 끝나면 store 명령으로 다시 메모리에 결과를 가져와 저장
- 모든 오퍼랜드(피연산자)는 CPU내 레지스터에서 조작된다.
- 스마트폰 또는 임베디드 기기와 같이 전력 소모가 적고 처리 속도가 빨라야 하는 곳에 사용된다.
- RISC 명령어 집합을 가진 CPU 아키텍처 : ARM, RISC-V

> 파이프 라이닝 : 인출과 실행 단계가 겹치도록 프로세서를 설계하여 명령어 여러 개가 다양한 단계에 걸쳐 진행되도록 만든다. 명령어 한 개가 완료되는 데는 같은 시간이 걸리지만, 여러 명령어를 동시에 처리하므로 전체적인 처리 속도가 빨라진다.


<br/>

| |RISC|CISC|
|:------:|:------:|:------:|
|명령어의 수|적음|많음|
|명령어의 길이|고정|가변|
|실행 사이클|단일|다중|
|주소 지정|간단|복잡|
|레지스터|많음|적음|
|전력 소모|적음|많음|
|처리 속도|빠름|느림|
|프로그래밍|복잡함|간단함|
|용도|서버, 워크스테이션|개인용 컴퓨터(PC)|


<br/>

#### x86

- 인텔이 개발한 마이크로프로세서 계열, 이들과 호환되는 프로세서들에서 사용한 명령어 집합 구조들을 통칭하는 말이다.
- 현재는 내부적으로 RISC와 유사한 기능을 하도록 설계된다.
- 특징: 인텔과 AMD가 주로 사용하는 CISC(복잡 명령어 집합 컴퓨터) 구조이다.
- 명령어 세트: 복잡하고 다양한 명령어를 지원하여 프로그래밍의 유연성을 높인다.
- 성능: 고성능 데스크탑, 서버, 워크스테이션 등에서 사용된다.
- 소프트웨어 호환성: Intel 및 AMD가 이 아키텍처를 기반으로 프로세서를 제공한다.
- 단점: 설계가 복잡하고 전력 소모가 많다.
  - 이전 버전들과 호환하기 위해 구조를 추가하며 개발했기 때문에 복잡한 명령어 체계를 가진다.
- 장점 : 명령어 길이가 서로 다르기 때문에 다른 명령어의 길이를 줄임으로 코드를 간결하게 해 디버깅 속도를 높일 수 있다.
  - 이전 버전과의 호환성이 좋다.

<br/>

#### ARM

- 특징: 단순 명령어 집합 컴퓨터(RISC) 구조로 모바일, 애플 A칩 임베디드 시스템에서 주로 사용된다.
- 명령어 세트: 간단하고 효율적인 명령어를 사용하여 성능과 에너지 효율성을 최적화한다.
- 전력 효율: 저전력 소모로 배터리 수명을 연장한다.
- 성능: 최근 고성능 ARM 프로세서가 개발되어 데스크탑과 서버에서도 사용된다.
  - 애플 M1칩을 기점으로 노트북, 데스트톱 등 PC에서 사용된다.
  - M1칩은 기존 인텔 기반 맥북보다 빠르고 저전력 모드에서도 높은 성능을 발휘한다.
  - ARM 아키텍처가 가지고 있는 에너지 효율성의 장점을 살린것이다.
- 단점: 명령어 길이가 정해져 있어 많은 코드를 구현해야한다.
  - PC에서 구현하기 위해서는 ARM을 지원하는 프로그램으로 실행해야 하기 때문에 속도가 느리다.
- 장점 : 고성능 저전력이다.
  - 메모리 저장방식을 2가지 선택하여 사용하기 때문에 메모리 사용 유연성을 높인다.
  - 간단한 명령어를 사용하기 때문에 구조가 간단하고 실행 속도가 빠르다.


<br/>
<br/>

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
