---
title: "iOS 면접 준비 - 네트워크"
author: gyeomji
date: 2024-08-10 10:00:00 +0900
categories: [Interview]
tags: [네트워크]
pin: false
math: true
mermaid: true
---


## ✏️ 컴퓨터 네트워킹에서의 OSI 7계층 모델

---

![computerNetworkLayer](/assets/img/computerNetworkLayer.png)

### OSI (Open Systems Interconnection Reference Model)

- 국제표준화기구(ISO)에서 개발한 모델이다.
- 통신 관련 프로토콜을 기능별로 나눴다.
- 각 계층은 서로 독립적으로 구성되어 있고, 각 계층은 하위 계층의 기능을 이용해 상위 계층에 기능을 제공한다.
- TCP/IP 모델의 뼈대를 제공하기 위해 개발된 참조 모델이다.

![osi7Layer](/assets/img/osi7Layer.png)

- 계층을 지날 때 마다 Header가 붙는다.
  - 이는 해당 계층의 기능과 관련된 제어 정보가 포함되어 있다.
- 제어 정보들은 모두 OS가 제공하는 프로토콜에 의해 송신 측에서는 계층을 지날 때마다 추가되고, 수신 측에서는 계층을 지날 때마다 제거된다.

<br/> 

### ✏️ 각 계층의 역할과 프로토콜

#### Application Layer

- 응용 프로그램에 대한 인터페이스를 제공한다.
- 사용자와 네트워크 간 응용 서비스를 연결하고 데이터를 생성한다.
- 프로토콜 : HTTP, SMTP, FTP, DNS, RTP
  - FTP : 파일을 전송하도록 설계된 네트워크 프로토콜
  - HTTP : 네트워크 장치 간에 정보를 전송하도록 설계된 프로토콜
  - SMTP : 인터넷을 통해 이메일 메시지를 보내고 받는 데 사용되는 통신 프로토콜
  - DNS : 사용자에게 친숙한 도메인 이름을 IP 주소로 변환하는 인터넷 표준 프로토콜의 구성 요소
  - RTP : 실시간 음성과 영상 및 데이터를 IP 네트워크로 전송하는 표준 프로토콜
- ex) 어떤 화물(data)을 전송할 것인가

<br/> 

#### Presentation Layer

- 두 사용자의 응용 프로그램 간 교환되는 데이터의 형식 변환 및 복구 관련 기능을 제공한다.
- 응용 계층으로부터 받은 Data를 수신 측에 맞는 코드/형식으로 변환하거나 반대의 과정을 수행한다.
- 필요시 암호화(복호화)를 진행한다.
- 프로토콜 : JPEG, MPEG, ASCII, EBCDIC, SSL, TLS
- ex) 어떻게 포장할 것인가 (type)

<br/> 

#### Session Layer

- 통신을 위한 세션 관리, 연결 설정 및 해제, 동기화 기능을 제공한다.
- 송신 시 데이터 복구를 위한 동기점을 생성한다.
- 수신 시 동기점을 확인한다.
- 프로토콜 : NetBIOS, RPC, NFS
- ex) 각 화물을 어떻게 구분할 것인가

<br/> 

#### Transport Layer

- 종단 프로세스 간 데이터 전송을 보장한다.
- 포트번호를 관리하여 수신된 데이터가 어느 응용 프로그램에 전송될지 판독하고 데이터를 전송한다.
- 신뢰성 있는 통신 서비스를 제공한다.
- 흐름 제어, 오류 제어, 메시지 전달, 패킷 분할 및 재조립, 오류 검출 및 복구 기능을 수행한다.
- 프로토콜 : TCP, UDP
- ex) 목적지에 잘 전달되었는가

##### 흐름 제어 Flow Control

- 송신 측과 수신 측의 데이터 처리 속도 차이를 해결하기 위한 기법이다.
- receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것이다.
- 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점이다.
- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.
  - 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있다.
  - 손실 된다면 불필요한 응답과 데이터 전송이 송/수신 측 간에 빈번이 발생한다.
  - 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.
- `Stop and Wait ARQ` : 매번 전송한 프레임에 대해 확인 응답(ACK/NAK)을 받아야만 그 다음 프레임을 전송하는 방법이다.
  - 송신 측이 ACK를 받으면 다음 프레임을 전송하고 NAK를 받으면 해당 프레임을 재전송한다.
  - 구현이 간단하고 송신 측에서 최대 프레임 크기의 버퍼가 1개만 있어도 된다.
  - 전송 시간이 긴 경우 전송 효율이 저하된다.
- `Sliding Window (Go-back-N ARQ)` : 수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어기법이다.
  - 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송한다.

> Window : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 3 way handshaking을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.

##### 혼잡 제어 Congesion Control

- 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법이다.
- 송신 측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달된다.
- 한 라우터에 데이터가 몰릴 경우 자신에게 온 데이터를 모두 처리할 수 없게 된다.
- 이 경우 호스트들은 재전송을 하게 되고 결국 혼잡을 가중시켜 오버플로우나 데이터 손실을 발생시킨다.
- 또한, 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라 한다. 
- `AIMD` : 처음에 패킷을 하나씩 보내고 문제 없이 도착하면 window 크기를 1씩 증가시켜가며 전송하는 방법이다.
  - 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷 보내는 속도를 절반으로 줄인다.
  - 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있다.
  - 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못한다. 
  - 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식이다.
- `Slow Start 느린 시작` : AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 1씩 늘려주는 방식이다.
  - 즉, 한 주기가 지나면 window size가 2배로 된다.
  - 전송속도는 AIMD에 반해 지수 함수 꼴로 증가시켜 빠르게 네트워크의 대역폭을 사용하게 된다.
  - 대신 혼잡 현상이 발생하면 window size를 1로 줄인다.
  - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있다.
  - 그러므로 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킨다.
- `Fast Retransmit 빠른 재전송` : 빠른 재전송은 TCP의 혼잡 조절에 추가된 정책이다.
  - 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 된다.
  - 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보내게 되므로, 중간에 하나가 손실되게 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 된다. 
  - 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있다.
  - 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다. 
  - 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 window size를 줄이게 된다.
- `Fast Recovery 빠른 회복` : 혼잡한 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방법이다.
  - 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.

<br/> 

#### Network Layer

- 패킷을 목적지까지 전달하고, 경로 선택 및 데이터 전송을 위한 논리적인 주소(IP)를 할당한다.
- 네트워크에서 목적지로 데이터 전송을 위해 IP주소를 이용해 최적의 경로를 찾고(routing), 다음 라우터에 데이터를 넘겨준다. (forwarding)
- 목적지에 대한 최적 경로를 설정한다.
- 프로토콜 : IP, ICMP, ARP, RARP
- ex) 목적지에 대한 경로는 어떻게 되는가

<br/> 

#### Data Link Layer

- 유선 또는 무선으로 직접 연결된 이웃한 두 장치 간 데이터 전달을 담당한다.(LAN 내부)
- 물리 계층으로부터 전송된 데이터의 오류를 검출하고 수정하며, 데이터 전송을 위한 물리적 주소(MAC)를 지정한다.
  > MAC (Medium Access Control) : 여러 장치가 동시에 매체를 통해 데이터를 전송할 때 제어하는 규칙이다.
- 동기화, 오류 제어, 흐름 제어, 회선 제어 기능을 제공한다.
- ex) 다음 교차로까지 잘 전달 되었는가

<br/> 

#### Physical Layer

- 장치 간 비트 단위 데이터를 전송한다.
- 1과 0 비트로 구성된 데이터를 전기 신호로 바꿔 전송한다.
- 수신한 신호에서 비트 1, 0을 찾는다.
- ex) 다음 교차로까지 도로 위로 자동차가 이동한다.

> 출발지 - 도착지<br/> 
> 응용 + 표현 + 세션 계층 : 화물 종류, 포장 방법, 구분 방법<br/> 
> 전송 계층 : 도착지에 잘 도착했는지 확인<br/> 
> 교차로 (Router)<br/> 
> 네트워크 계층 : 도로 표지판(방향 결정)<br/> 
> 데이터 링크 계층 : 신호등 및 차선 대기(출발 시간 결정)<br/> 
> 물리 계층 : 자동차 바퀴(실제 이동)


<br/> 

## ✏️ TCP/IP 모델과 OSI 모델의 차이점

---

- 계층구조
  - OSI는 7계층 모델이고, TCP/IP는 4계층 모델이다.
- 사용 목적
  - OSI : 이론적인 참조 모델으로 개발되었으며, 네트워크 기술 및 프로토콜의 표준화를 위해 사용된다.
  - TCP/IP : 인터넷 표준 프로토콜로 개발되었으며, 실질적인 통신을 위해 사용된다.
- 프로토콜 구성
  - OSI : 각 계층의 기능과 역할에 대한 이론적인 설명을 제공한다.
  - TCP/IP : 인터넷에서 사용되는 실질적인 프로토콜들을 구성하고 있다.

<br/> 

### TCP/IP


![networkProtocol](/assets/img/networkProtocol.png)

### Application Layer

- 효율성을 고려하여 OSI의 응용, 표현, 세션 계층을 합쳤다.

<br/> 

### Transport Layer

- 종단 프로세스 간 데이터 전송을 보장한다.
- 포트번호를 관리하여 수신된 데이터가 어느 응용 프로그램에 전송될지 판독하고 데이터를 전송한다.
- 신뢰성 있는 통신 서비스를 제공한다.
- 흐름 제어, 오류 제어, 메시지 전달, 패킷 분할 및 재조립, 오류 검출 및 복구 기능을 수행한다.

<br/> 

### Internet Layer

- 패킷을 목적지까지 전달하고, 경로 선택 및 데이터 전송을 위한 논리적인 주소(IP)를 할당한다.
- 네트워크에서 목적지로 데이터 전송을 위해 IP주소를 이용해 최적의 경로를 찾고(routing), 다음 라우터에 데이터를 넘겨준다. (forwarding)
- 목적지에 대한 최적 경로를 설정한다.

<br/> 

### Network Access Layer

- 효율성을 고려하여 OSI의 데이터 링크, 물리 계층을 합쳤다.
- LAN : Ethernet, IEEE 802.11(wifi), FDDI 등
- WAN : SDLC, HDLC, PPP, LAPB, ATM 등

> LAN (Local Area Network) : 근거리 네트워크로 한정된 좁은 지역에 구성된 네트워크이다. 일반적으로 사용자의 장치를 포함한다. (PC방, 사무실, 학교, 컴퓨터, 서버, 스파트폰, 노트북 등) 스위치로 장치들이 연결된다. (스위치에 IP정보가 없다.)<br/> 
> MAN (Metropolitan Area Network) : 도시 정도의 지역에 구성된 네트워크이다. 라우터로만 구성되어 있다.<br/> 
> WAN (Wide Area Network) : MAN과 기능은 같지만 사이즈가 크다.(라우터로만 구성) 원거리, 광역 네트워크로 넓은 지역을 연결한다. 두 개 이상의 LAN을 넓은 지역에 걸쳐 연결한다. (SK 브로드밴드, KT, LG U+) <br/> 
> Internet : 전세계의 수많은 LAN과 WAN들이 연결된 거대한 네트워크이다.<br/> 
> 네트워크 간 서로 동일한 프로토콜을 사용해야 한다.


<br/> 

## ✏️ 네트워크 프로토콜 스택과 iOS에서의 네트워크 통신 방식

---

- 네트워크 프로토콜 스택 : 데이터 통신에 활용되는 프로토콜의 구조에 관한 개념으로 스택 구조로 모여 있는 프로토콜의 집합이다.

<details>
<summary>프로토콜 💡</summary>
<div markdown="1">

### 프로토콜

- 통신을 원하는 두 개체간에 무엇을, 어떻게, 언제 통신할 것인가를 서로 약속한 규약이다.
- 구문(Syntax), 의미(Semantics), 타이밍(Timing)으로 구성되어 있다.

#### 구문

- 데이터의 형식, 부호화 및 신호의 크기 등을 포함하여 `무엇을 전송`할 것인가에 관한 내용이 들어있다.

#### 의미

- 데이터의 특정한 형태에 대한 `해석`을 `어떻게` 할 것인가와 그와 같은 해석에 따라 `어떻게 동작`을 취할 것인가 등, 전송의 조정 및 오류 처리를 위한 제어정보 등을 포함한다.

#### 타이밍

- 데이터를 `언제 전송`할 것인가와 얼마나 빠른 속도로 전송할 것인가와 같은 내용을 포함한다.

<br/>

#### 프로토콜 설계 기본 원칙

- 최대한 `간단`하게 정의한다.
  - 두 시스템 간 처리해야 하는 내용을 정리한다.
  - 정리된 내용 중 핵심 요소를 추출하여 중복된 내용을 제거한다.
- `명확`하게 정의한다.
- `확장 가능`하게 정의한다.

</div>
</details>


![protocolStack](/assets/img/protocolStack.png)

### Application 응용 계층 

- 송수신측 사이 주고 받는 서비스에 따라 응용 계층의 프로토콜이 달라진다.
- 클라이언트(웹 브라우저)가 HTML 문서 요청 ➡️ HTTP 프로토콜
- 클라이언트가 메일 요청 ➡️ SMTP
- 클라이언트가 파일 요청 ➡️ FTP

### Transport 전송 계층 

- 데이터 전송의 신뢰성을 보장하기 위한 계층으로 송신 측에서 수신 측으로 패킷이 정상적으로 전달되었는지 확인하는 역할을 한다.
- TCP (전송 제어 프로토콜) : 신뢰성과 정확성을 추구하는 연결형 통신 프로토콜을 따르기 때문에 신뢰할 수 있는 연결을 위해 3-way 핸드쉐이크 과정을 거친다. 
  - 브라우저나 메일 등 일반적인 애플리케이션이 데이터를 송/수신할 경우 활용한다.
- UDP (사용자 데이터그램 프로토콜) : 효율성을 추구하는 비연결형 통신 프로토콜을 따르므로 핸드쉐이킹 과정이 생략된다. 
  - 데이터 전송의 정확성보다 신속성이 강조되는 스트리밍 방식의 동영상 서비스에 활용된다. 
  - DNS서버에 대한 조회 등 짧은 제어용 데이터를 송/수신할 경우 사용된다.
  > DNS : 사용자에게 친숙한 도메인 이름을 IP 주소로 변환하는 인터넷 표준 프로토콜의 구성 요소

### Network 계층 

- 데이터를 원하는 목적지로 전송한다. 데이터 패킷들이 라우터를 거쳐 수신 측으로 전달된다.
- 라우터는 데이터의 목적지가 정해지면 해당 목적지까지 어떤 경로로 가는 것이 효율적인지 파악한다.
- 경로를 결정하기 전 원하는 네트워크를 식별하기 위한 목적지의 주소 정보인 IP 주소를 확인하는데, 목적지 IP 주소까지 어떤 경로를 거쳐 데이터를 보낼지 결정하는 것을 라우팅이라고 한다.

### Data Link 계층 

- 송/수신측 사이에 경로가 결정되었으면 링크 계층은 한 노드에서 인접한 노드로 패킷을 보내기 위한 역할을 한다.
- 위의 계층을 모두 거칠 경우 데이터는 서버의 LAN(Local Area Network)에 도착한다.
- LAN에서 데이터를 정상적으로 주고 받기 위해 네트워크 장비 간 신호를 주고받는 규칙을 정한다.
- 유선 연결에서 가장 많이 사용되는 기술이 이더넷이다.
  - LAN을 위해 개발된 네트워크 기술으로, 초고속 인터넷 서비스와 같은 것들을 유선으로 연결할 때 사용한다.
  - 네트워크에 연결된 각 기기들이 고유의 MAC주소를 가지고, 주소를 이용해 상호간에 데이터를 주고 받을 수 있게 만들어진 기술이다.
  - 충돌을 방지하기 위해 데이터를 보내는 시점을 늦추는 CSMA/CD 방법을 사용한다.
  > 충돌 Collision : 컴퓨터 여러 대가 동시에 데이터를 보내 데이터들이 서로 부딪히는 경우
- `DLC(Data Link Control)`, `MAC(Media Access Control)` 두 개의 계층으로 나뉜다.
- DLC(LLC - 이더넷에서 구현할 경우) : 데이터 링크 제어 부계층으로 프레임을 어떻게 주고 받을지 약속하는 계층이다.
  - 프레임에 번호를 어떻게 붙일지, 오류를 어떻게 검출할지 등
- MAC : 매체 접근 제어 부계층으로 여러 장치가 동시에 매체를 통해 데이터를 전송할 때 제어하는 규칙이다.
  - 한 매체에는 단 두 개의 장치만 독점적으로 사용하거나, 여러 장치가 동시에 한 매체에 접속되어 있을 수 있다.
  - 두 개만 서로 연결된 링크를 P2P링크, 여러 개가 동시에 접속된 경우 브로드캐스트 링크라 한다.
  - P2P 링크의 경우 각 장치에서 데이터를 주고 받을 대상이 서로 밖에 없기 때문에 데이터를 그냥 주고 받으면 된다.
  - 브로드캐스트 링크의 경우 매체를 공유하기 때문에 충돌을 방지, 해결하는 방법이 있어야 한다.
  - MAC은 충돌이 발생하지 않게 하거나 충돌이 발생해도 재전송하여 모든 장치가 매체에 접근 가능하도록 만들어준다.
  - 유선에서는 브로드캐스트가 잘 없지만, 무선 통신은 모든 장치가 동일한 매체(공기 중의 전자기파)를 사용하기 때문에 무선 통신 성능에서 MAC이 중요한 역할을 한다.

### Physical 계층 

- 컴퓨터와 네트워크 장비를 연결하고 컴퓨터와 네트워크 장비 간 전송되는 데이터를 전기 신호로 변환한다.
- 컴퓨터 내부에 존재하는 랜 카드가 0과 1로 이뤄진 비트열을 전기 신호로 변환하는 역할을 수행한다.

<br/>


## ✏️ iOS 앱에서 네트워크 통신을 하는 방법에는 어떤 것들이 있나요?

---

- iOS에서는 네트워크 통신을 위한 다양한 라이브러리와 프레임워크를 제공한다.
  - URLSession, Alamofire, Combine과 Swift Concurrency 등
- iOS에서는 보안 통신을 위해 주로 HTTPS를 사용한다. 
  - iOS 9부터는 <span style="color:#9fb584">**App Transport Security (ATS)**</span>가 도입되어 앱이 기본적으로 안전한 네트워크 연결(HTTPS)을 사용하도록 권장한다.

<br/>

### URLSession

- Foundation Framework에서 지원하는 iOS 앱과 서버간 데이터를 주고받기 위해 사용되는 API이다.
  - url로 표시되는 endpoint로 부터 데이터를 다운로드하거나 endpoint로 업로드를 수행하는 API이다.
- 데이터를 <span style="color:#9fb584">**비동기적**</span>으로 주고 받을 수 있으며 HTTP, HTTPS, FTP 등의 프로토콜을 지원한다.
  - 비동기 작업을 지원하여 네트워크 요청 중에도 UI가 멈추지 않도록 도와준다.
  - iOS 앱이 실행중이지 않을 때 백그라운드에서 다운로드를 수행할 수 있다.
- <span style="color:#9fb584">**파일 다운로드, 업로드 등을 처리**</span>할 수 있다.
- 인증, 쿠키 관리, 캐시 관리 등을 지원한다.
- 통신 시 `Requst`와 `Response`를 기본 구조로 가진다.
- Request
  - URL객체를 통해 직접 통신하는 형태 / URLRequest객체를 만들어 통신하는 형태(옵션 설정 가능)가 있다.
  - 설정 가능한 옵션으로는 HTTP Method(Get, Post등), 어떤 내용을 전송할 것인지 등이 있다. 
- Response
  - 설정된 `Completion Handler`로 Response를 받는 형태 / `URLSessionDelegate`를 통해 지정 메소드를 호출하는 형태가 있다.
  - background 상태에서의 동작이나, 인증 및 캐싱을 default 옵션으로 사용하지 않는 경우 Delegate 패턴을 사용한다.
- 장점 : iOS에서 기본 제공되므로 별도의 라이브러리 추가가 필요 없고, 다양한 요청 방식(GET, POST 등)을 지원한다.
- 단점 : 추가적인 설정이나 비동기 처리를 위해 코드가 복잡해질 수 있다.

<details>
<summary>Response 받는 법 (Completion Handler, URLSessionDelegate) 💡</summary>
<div markdown="1">

#### 1. Completion Handler를 이용한 데이터 결과 받기

- 특별한 작업 없이 간단히 데이터만 가져오는 경우 사용한다. 
- `completion handler`를 사용하는 `data task`를 생성한다.
- `task`는 서버의 `response`, `data`, `error`를 `completion hander`에 전달한다.

![completionHandler](/assets/img/completionHandler.png)

- `data task`를 생성하기 위해서는 `URLSession`클래스의 `dataTask` 메소드를 사용해야 한다.
- 해당 메소드의 `completion handler`는 아래 3가지 작업을 수행해야 한다.
1. `error` 파라미터를 확인한다. 파라미터가 `nil`이면 전송이 성공적으로 수행되었다는 의미이다.
2. `response`파라미터의 `status code`를 확인하여 서버와의 연결이 성공인지 확인한다.
   1. `MIME` 타입을 확인하여 전송받은 문서가 원래 예측된 문서 형식인지 확인한다.
    > MIME 타입 : 클라이언트에게 전송된 문서의 다양성을 알려주는 메커니즘으로 파일의 확장자를 의미한다. 이 타입을 이용하여 전송받는 문서의 종류를 알고, 다음 동작을 정의하기 위해 사용한다. ex) application/json
3. 1번과 2번을 확인하여 모두 정상인 경우에는 서버에서 전송받은 데이터가 담긴 `data` 파라미터를 사용하면 된다.


#### 2. `URLSessionDelegate`을 이용하여 데이터 결과 받기

- session에 delegate를 설정하여 전송 결과를 받을 수 있다.
- 캐싱과 인증 등 다양한 기능이 필요한 경우 delegate 패턴을 적용해야 한다.
- 아래 사진은 delegate를 구현하여 task 로부터 전송 결과를 받는 과정의 모습이다.

![urlSessionDelegate](/assets/img/urlSessionDelegate.png)

- 데이터 전송을 수행할 클래스가 구현해야 할 프로토콜의 종류는 다음과 같다.
- `URLSessionDelegte` : session Life cycle와 같은 session-level의 이벤트를 처리하는 프로토콜
- `URLSessionTaskDelegte` : task-level의 이벤트를 처리하는 프로토콜
- `URLSessionDataDelegte` : 데이터 업로드 task와 같은 특정 task-level의 이벤트를 처리하는 프로토콜
- `URLSessionDownloadDelegte` : 다운로드 task를 수행하는 프로토콜

</div>
</details>

<br/>

- URLSession은 두 가지 종류로 구성되어 있다.
  - `URLSessionConfiguration` : URLSession 생성
  - `URLSessionTask` : 실제 서버와 통신 담당(서버로 보낸 요청에 대한 응답을 받는 역할)
  - URLSession은 Thread-Safty하기 때문에 어떤 스레드에서든 자유롭게 Session과 Task를 생성할 수 있다.
  - URLSession은 `URLSessionConfiguration`을 통해 생성할 수 있다.
  - URLSession을 통해 한 개 이상의 `URLSessionTask`를 생성할 수 있으며, `URLSessionTask`를 통해 실제로 서버와 통신을 할 수 있다.
  - `URLSessionTask` 각각은 데이터를 fetch하거나, 업로드하거나, 파일을 다운로드하는 역할을 할 수 있다.
- `OperationQueue`, `DispatchQueue`를 사용해 재시도 로직을 구현할 수 있다.
  - 네트워크 통신의 안정성을 향상시키기 위해 네트워크 요청이 실패했을 때 일정 횟수만큼 다시 시도하는 것이다.
  ``` swift
  func fetchData(retryCount: Int = 3) {
      let url = URL(string: "https://api.example.com/data")!
      let task = URLSession.shared.dataTask(with: url) { data, response, error in
          if let error = error, retryCount > 0 {
              DispatchQueue.global().asyncAfter(deadline: .now() + 2) {
                  fetchData(retryCount: retryCount - 1)
              }
          } else if let data = data {
            // 데이터 처리

          }
      }
      task.resume()
  }
  ```
- 캐싱을 통해 네트워크 요청의 빈도를 줄이고 서버의 부하를 줄여 앱의 성능을 향상시킬 수 있다.
  - `URLCache`로 캐싱을 구현할 수 있다.
  - URLCache는 URL 요청과 응답을 캐싱하는 클래스이다.

  ``` swift

  let url = URL(string: "https://api.example.com/data")!
  let request = URLRequest(url: url)
  let cache = URLCache.shared

  if let cachedResponse = cache.cachedResponse(for: request) {
    // 캐시된 데이터 사용

  } else {
      let task = URLSession.shared.dataTask(with: request) { data, response, error in
          if let data = data, let response = response {
              let cachedResponse = CachedURLResponse(response: response, data: data)
              cache.storeCachedResponse(cachedResponse, for: request)
          }
      }
      task.resume()
  }

  ```


### Alamofire와 같은 외부 라이브러리를 사용하지 않고 URLSession을 사용할 때 장점

- 경량성 및 의존성 감소
  - URLSession은 Swift 표준 라이브러리의 일부이기 때문에, 별도의 외부 라이브러리를 추가할 필요가 없다.
  - 이로 인해 앱의 크기가 줄어들고, 의존성 관리도 간소화된다.
  - 반면 Alamofire와 같은 라이브러리를 사용하면 외부 코드에 의존하게 되며, 이는 라이브러리의 업데이트나 변경에 따라 앱이 영향을 받을 수 있다.
- 더 깊은 제어 및 커스터마이징
  - URLSession을 사용하면 네트워크 요청의 모든 세부 사항을 직접 제어할 수 있다. 
  - 요청의 헤더, 캐싱 정책, 타임아웃 설정 등을 세밀하게 조정할 수 있어, 특정한 요구사항에 맞춘 네트워크 작업을 구현할 수 있다.
- 배우기 좋은 기본 개념
  - URLSession을 직접 사용하면 네트워킹의 기본 개념을 깊이 이해할 수 있다. 
  - 네트워크 요청의 흐름, HTTP 프로토콜, 비동기 작업 처리 등의 원리를 이해하는 데 도움이 된다.
  - 이는 네트워킹에 대한 깊이 있는 지식을 쌓고자 하는 개발자에게 큰 이점이다. 
- 빠른 디버깅 및 문제 해결
  - 표준 라이브러리를 사용하면 Swift와 iOS의 기본 디버깅 도구를 활용하여 문제를 추적하고 해결하기가 더 쉬울 수 있다. 
  - 외부 라이브러리를 사용할 경우, 해당 라이브러리 내부의 동작을 이해하는 데 추가적인 노력이 필요할 수 있다.
- 최신 기능 사용
  - URLSession은 Apple의 공식 API이기 때문에, 최신 iOS 기능이나 최적화가 가장 빠르게 반영된다.
  - 외부 라이브러리의 경우 이러한 기능이 지원되기까지 시간이 걸릴 수 있다.
- 배터리 효율성 및 성능 최적화
  - Apple은 URLSession을 iOS 시스템에 최적화하여 개발했기 때문에, 배터리 효율성과 성능 측면에서 최적의 결과를 기대할 수 있다.


<br/>

### session 종류

- `shared session`
  - 싱글턴 패턴 구조로 단순한 네트워크 요청에 사용한다. 
  - Complete Handler를 이용해 네트워크 응답을 전달받는다. 
  - 백드라운드 전송을 지원하지 않는다. 
  - 커스터마이징이 불가능하다.(delegate, configuration 설정 불가)
- `default session`
  - 기본 통신을 할 때 사용한다. 
  - 커스터마이징이 가능하다. 
  - Delegate를 이용해 데이터를 점진적으로 받아올 수 있다. 
  - `URLSessionConfiguration` 인스턴스를 사용해야 하며, `URLSessionDelegate` 프로토콜 및 서브 프로토콜을 구현하는 클래스를 생성해야 한다.
  - 디스크 기반 캐싱을 지원한다.
  > 디스크 기반 캐싱 : 네트워크 요청의 응답 데이터를 디스크에 저장하여, 이후 동일한 요청이 발생했을 때 네트워크를 통해 다시 요청하는 대신 디스크에 저장된 응답 데이터를 사용하는 방식이다. 네트워크 트래픽을 줄이고, 응답 시간을 단축시킬 수 있다.
- `Ephemeral session`
  - 쿠키나 캐시를 저장하지 않는 정책을 사용할 때 사용한다. 
  - 사파리 개인정보보호 모드
- `Background session`
  - 앱이 백그라운드 상태에 있을 때 컨텐츠를 다운로드/업로드한다.

> [Session 생성 시 유의할 점]<br/>
> 불필요하게 session을 생성하면 안 된다.<br/>
> 앱에서 유사한 session들이 필요한 경우 하나의 session을 만들고 이를 공유하여 사용하도록 설계하는 것이 좋다.

<br/>

### task 종류

- `Data task` : NSData객체를 사용하여 데이터를 보내고 받는다. 백그라운드 세션 지원을 하지 않는다.
- `Upload task` : data task와 유사하나, 파일 형식 데이터 전송을 지원하며, 백그라운드에서 업로드가 가능하다.
- `Donwload task` : 파일 형식 데이터 전송을 지원하며, 백그라운드 상태에서 업로드와 다운로드가 가능하다.

<br/>


### URLSession의 기본 사용 방법 (URLSession Life Cycle)

1. `URLSessionConfiguration`을 설정하여 `URLSession` 인스턴스를 생성한다.
2. `URL`을 생성한다. 
   1. 유효한 URL인지 확인한다.
3. `URLRequest`를 생성한다. 
   1. 요청 보낼 url과 HTTP 메서드, 헤더, 바디를 설정한다.
4. 적절한 `URLSessionTask`를 생성한다. 
   1. dataTask, uploadTask, downloadTask 중 하나의 작업을 생성한다.
   2. 해당 작업은 요청이 완료되면 클로저를 통해 data, response, error를 반환한다.
5. Task를 `resume` 해준다.
   1. `dataTask()` 메소드 중 하나를 사용하여 data task를 생성하면 처음에는 `suspended` 상태가 된다.
   2. 그러므로 task를 시작하는 `resume()` 메소드를 명시적으로 호출해야 한다.
6. Task가 완료되면 `Completion Handler` 클로저가 실행된다.(JSON 데이터 파싱, 전송 확인 완료 등)

``` swift

// URLSessionConfiguration 설정, URLSession 생성
let session = URLSession(configuration: .default)
// URL 설정
guard let url = URL(string: urlString + "/annotations") else {
          print("Invalid URL")
          return
}

// URLRequest 설정
var request = URLRequest(url: url)
request.httpMethod = "POST"
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("\(userId)", forHTTPHeaderField: "userId")
request.httpBody = requestBody

// 데이터 작업 생성 및 실행
session.dataTask(with: request) { (data: Data?, response: URLResponse?, error: Error?) in
  guard error == nil else {
      print("Error occur: insertAnnotation error calling POST - \(String(describing: error))")
      return
  }
  guard let data = data, let response = response as? HTTPURLResponse, (200..<300) ~= response.statusCode else {
      print("Error: insertAnnotation HTTP request failed")
      return
  }
  
  let dateDecoder = JSONDecoder()
  dateDecoder.dateDecodingStrategy = .formatted(self.dateformat)
  
  guard let decodedData = try? dateDecoder.decode(ResponseModel<Annotation>.self, from: data) else {
      print("Error: insertAnnotation JSON parsing failed")
      return
  }
  
  if decodedData.status == 201 {
      self.annotation = decodedData.data!
      completion(self.annotation)
  }
  else {
      print("insertAnnotation failed")
  }
// 작업 시작
}.resume()
```

<br/>

### Alamofire (서드파티 라이브러리)

- HTTP 네트워킹을 위해 스위프트 기반으로 개발된 비동기 라이브러리로 iOS와 macOS에서 사용 가능하다.
  - 오직 swift로만 작성된 프레임워크이다.
- url세션에 기반한 라이브러리로 네트워킹 작업을 단순화하고 네트워킹을 위한 다양한 메서드와 json 파싱 등을 제공한다.
- URLSession에 비해 <span style="color:#9fb584">**코드를 간소화할 수 있어 가독성이 좋으며 직관적인 사용법**</span>을 가지고 있다.
- 서버로 보낼 요청을 간편하게 구성 가능하다.
- 서버의 응답 컨텐츠 타입에 맞추어 사용 가능한 다양한 메소드를 제공한다.
- 장점 : 코드가 간결해지고 가독성이 높아지며, JSON 파싱, 파라미터 인코딩, 인증 등 추가 기능을 쉽게 사용할 수 있다.
- 단점 : 앱에 서드파티 라이브러리를 추가해야 하므로, 업데이트에 신경을 써야 한다.

``` swift
let url = APIConstrants.baseURL + "/member/memberLogin"
        
let body : Parameters = [
    "memberId" : id,
    "memberPw" : pw
]

let dataRequest = AF.request(url, method: .post, parameters: body, encoding: JSONEncoding.default, headers: header)

dataRequest.responseData(completionHandler: { response in
    switch response.result {
    case .success:
        if let data = response.value {
            print(data)
            guard let decodedData = try? JSONDecoder().decode(Member.self, from: data) else {
                completion(false)
                return
            }
            self.member = decodedData
                UserDefaults.standard.set(2, forKey: "userType")
            UserDefaults.standard.set(decodedData.name, forKey: "name")
            UserDefaults.standard.set(decodedData.id, forKey: "pk")
                UserDefaults.standard.set(-1, forKey: "shelterPk")

            completion(true)
        }
    case .failure:
        print("error : \(response.error!)")
    }
})
```

<br/>

#### 서드파티 라이브러리(Alamofire) 사용 이유

네트워크 요청과 처리를 단순화하고, 유지보수성과 코드 안정성을 크게 높일 수 있어 개발 효율성이 향상된다. 특히, 앱이 복잡한 네트워크 통신을 많이 수행해야 하는 경우 Alamofire 같은 서드파티 라이브러리를 사용하는 것이 큰 이점이다.

1. 코드 간결화와 가독성 향상
 - Alamofire는 URLSession의 복잡한 설정과 비동기 작업을 간결한 API로 대체해 코드 가독성을 높이고, 코드 길이를 줄여준다.
 - 특히 JSON 파싱이나 파라미터 인코딩 등 자주 사용되는 네트워크 작업을 단순화하여, 코드 작성이 더 직관적이고 빠르다.
2. 추가 기능 제공
  - Alamofire는 URLSession에 비해 다양한 기능을 기본으로 제공한다.
  - 예를 들어, 파라미터 인코딩, 인증 처리, 파일 업로드 및 다운로드, 요청 재시도 기능 등 실무에 자주 필요한 기능들이 내장되어 있어, 별도로 구현하지 않아도 된다.
3. 비동기 코드 처리의 간편함
  - Alamofire는 클로저 기반의 API를 사용하여 비동기 작업 처리를 간단하게 만든다.
  - URLSession으로 직접 처리할 때의 복잡한 콜백 체인이나 에러 처리 로직을 줄여주고, 코드의 흐름이 더 직관적이다.
4. 유지보수와 협업 효율성 향상
  - Alamofire는 널리 사용되는 오픈소스 라이브러리로, 업데이트가 활발하며 커뮤니티와 문서 지원도 풍부하다.
  - 이를 통해 코드의 유지보수성과 확장성을 높일 수 있으며, 협업 시 다른 개발자들도 쉽게 이해하고 사용할 수 있다.
5. 테스트 지원
  - Alamofire는 네트워크 요청을 테스트 목적으로 모킹(Mock)할 수 있는 기능을 제공한다.
  - 이를 통해 테스트 작성이 간편해지고, 네트워크 의존성을 낮추어 개발 과정에서의 테스트 효율을 높일 수 있다.


<br/>

#### Swift Concurrency와 Alamofire의 비교

1. 코드 간결성과 가독성
  - Swift Concurrency
    - async/await 문법은 비동기 코드를 동기 코드처럼 작성할 수 있어, 비동기 처리 코드의 가독성이 좋다.
    - URLSession을 활용해 비동기 네트워크 통신을 쉽게 구현할 수 있어 <span style="color:#9fb584">**기본적인 네트워크 요청에 적합**</span>하다.
  - Alamofire
    - 클로저 기반의 API를 통해 비동기 코드 작성이 간결하며, 특히 JSON 파싱이나 요청 파라미터 인코딩 등에서 코드의 가독성을 높이는 기능을 제공힌다.
    - 네트워크 통신에 필요한 다양한 기능을 제공하므로, <span style="color:#9fb584">**복잡한 네트워크 처리에서는 Alamofire가 더 직관적**</span>일 수 있습니다.
2. 추가 기능 제공 여부
  - Swift Concurrency
    - async/await와 URLSession만으로는 <span style="color:#9fb584">**파라미터 인코딩, 네트워크 상태 모니터링, 요청 재시도 등 다양한 기능이 기본 제공되지 않는다.**</span>
    - 추가 기능이 필요할 경우 직접 구현해야 하므로, <span style="color:#9fb584">**상대적으로 Alamofire에 비해 기능 확장성이 떨어질 수 있다.**</span>
  - Alamofire
    - Alamofire는 파라미터 인코딩, 인증, 파일 업로드/다운로드, 네트워크 모니터링 등의 기능이 내장되어 있어, <span style="color:#9fb584">**네트워크 요청의 다양한 요구사항을 손쉽게 처리할 수 있다.**</span>
    - 이러한 추가 기능 덕분에 Alamofire는 복잡한 네트워크 통신이 필요한 경우에 적합하다.
3. 성능과 네이티브 지원
  - Swift Concurrency
    - 네이티브로 지원되기 때문에, <span style="color:#9fb584">**추가적인 라이브러리 의존성 없이 네트워크 요청을 구현**</span>할 수 있다.
    - 네이티브로 최적화되어 있으며, 성능 측면에서도 유리하다.
    - 특히 iOS 15 이상을 지원하는 앱에서는 Swift Concurrency를 사용해 네이티브 성능을 최대한 활용할 수 있다.
  - Alamofire
    - Alamofire는 URLSession을 기반으로 동작하므로, Swift Concurrency와 같은 네이티브 최적화 이점을 직접 제공하지는 않는다.
    - 그러나, <span style="color:#9fb584">**Alamofire는 비동기 네트워크 작업을 최적화해 설계**</span>되어 있어 성능적으로도 충분히 우수하며, 다양한 기능을 제공하므로 기능 확장성 측면에서 이점이 있다.
4. 호환성과 지원 기기
  - Swift Concurrency
    - iOS 15 이상에서만 지원된다.
  - Alamofire
    - Alamofire는 iOS 12 이상의 환경에서 사용할 수 있어, 더 넓은 범위의 기기를 지원할 수 있다.

➡️ 결론 : 앱의 지원 대상 iOS 버전과 네트워크 요구사항에 따라 선택한다.

- Swift Concurrency가 유리한 경우
  - 최신 iOS 버전(iOS 15 이상)만을 지원하는 앱에서, 간단한 네트워크 요청 및 응답 처리가 필요한 경우
  - 추가적인 네트워크 기능이 필요 없고, 네이티브 성능과 코드 간결성을 우선시할 경우
- Alamofire가 유리한 경우
  - 다양한 네트워크 관련 기능(인증, 파라미터 인코딩, 파일 업로드/다운로드 등)이 필요하거나, 복잡한 네트워크 요청을 자주 사용하는 경우
  - 하위 iOS 버전 지원이 필요한 경우
  - 네트워크 처리가 복잡해지고, 유지보수와 확장성을 위해 이미 구축된 서드파티 라이브러리를 활용하고자 할 때


<br/>

### Combine 프레임워크 (iOS 13 이상)

- Apple이 제공하는 <span style="color:#9fb584">**리액티브 프로그래밍 프레임워크**</span>이다.
- 비동기 데이터를 처리하고, 네트워크 요청 결과를 구독하고 반응형으로 처리할 수 있게 한다.
- <span style="color:#9fb584">**네트워크 요청과 결과를 하나의 데이터 스트림으로 다룰 수 있으며**</span>, URLSession과 함께 사용하여 네트워크 데이터를 처리할 수 있다.
- 장점: 데이터 스트림 기반의 코드 작성이 가능해 비동기 처리가 직관적이고, 여러 비동기 작업을 결합하기 쉬워진다.
- 단점: Combine은 iOS 13 이상에서만 사용할 수 있으며, 초기에 러닝 커브가 다소 있을 수 있다.

``` swift
import Combine
import Foundation

var cancellables = Set<AnyCancellable>()
let url = APIConstrants.baseURL + "/member/memberLogin"

URLSession.shared.dataTaskPublisher(for: url)
    .map { $0.data }
    .decode(type: [Post].self, decoder: JSONDecoder())
    .sink(receiveCompletion: { completion in
        switch completion {
        case .finished:
            print("Finished successfully")
        case .failure(let error):
            print("Error: \(error)")
        }
    }, receiveValue: { posts in
        print("Received posts: \(posts)")
    })
    .store(in: &cancellables)
```

<br/>

### Swift Concurrency (iOS 15 이상)

- Swift 5.5부터 도입된 Swift Concurrency는 <span style="color:#9fb584">**async/await를 사용하여 비동기 네트워크 요청을 더 간결하게 작성할 수 있으며, 에러 처리를 throws 키워드로 처리할 수 있다.**</span>
  - 에러 던지기: try await와 throws를 활용해 에러가 발생하면 자동으로 catch 블록에서 처리된다.
  - HTTP 상태 코드 및 데이터 파싱 에러 확인: 상태 코드와 JSON 파싱 오류를 do-catch 블록 내에서 간결하게 처리할 수 있다.
- 기존의 비동기 콜백 기반의 복잡한 코드를 간소화 한다.
- URLSession도 async/await와 함께 사용할 수 있도록 업데이트되었다.
- 장점: async/await를 통해 비동기 코드의 가독성과 유지보수성이 향상되며, 비동기 코드의 흐름을 쉽게 이해할 수 있다.
- 단점: iOS 15 이상에서만 지원되므로, 구형 기기 지원이 필요한 앱에는 적용하기 어렵다.

``` swift
import Foundation

func fetchData() async {
    guard let url = URL(string: urlString + "/annotations") else {
              print("Invalid URL")
              return
    }


    do {
        // 1. 네트워크 요청
        let (data, response) = try await URLSession.shared.data(from: url)
        
        // 2. HTTP 상태 코드 확인
        if let httpResponse = response as? HTTPURLResponse {
            guard 200...299 ~= httpResponse.statusCode else {
                print("HTTP Error: Status code \(httpResponse.statusCode)")
                return
            }
        }

        // 3. JSON 파싱
        let jsonObject = try JSONSerialization.jsonObject(with: data, options: [])
        print("Received JSON: \(jsonObject)")

    } catch {
        print("Network or Parsing Error: \(error.localizedDescription)")
    }
}

Task {
    await fetchData()
}
```

<br/> 

## ✏️ HTTP와 HTTPS의 차이점, 그리고 iOS에서의 보안 통신 방법

---

### HTTP (HyperText Transfer Protocol)

- 클라이언트와 서버 간 통신(인터넷에서 하이퍼텍스트를 교환)을 위한 통신 규칙 세트이다.
  - 네트워크 통신을 작동하게 하는 기본 기술이다.
- 데이터를 암호화하지 않고 전송한다.
  - HTTP 메시지는 일반 텍스트이므로, 권한이 없는 당사자가 데이터를 읽고 변조할 수 있다.
  - 송/수신측 서로 데이터가 완전한지 확인하지 않는다.
  - 송/수신측 서로 신뢰성 있는 상대인지 확인하지 않는다.
- 80번 포트를 사용한다.
- 일반적인 HTTP 통신은 TCP로 3-way 핸드쉐이크를 하여 연결을 만든 직후 바로 시작된다.
  - 연결이 완성되면 바로 HTTP 통신을 한다.

데이터를 클라이언트와 서버끼리만 확인할 수 있도록, 데이터를 암호화할 것.
서버의 신분을 확인할 수 있는 인증 과정을 거칠 것.


### HTTPS (HyperText Transfer Protocol Secure)

- HTTP의 확장 버전으로, 브라우저와 서버가 데이터를 전송하기 전 안전하고 암호화된 연결을 설정한다.
- HTTP와 TCP 사이 TLS 보안 레이어를 거친 후에 HTTP 통신을 한다.
- 모든 데이터를 SSL/TLS로 암호화하여 전송한다.
- 대칭키, 비대칭키 암호화 방식을 모두 사용하여 빠른 연산 속도와 안정성을 보장한다.
  - HTTPS 연결 과정(Hand-Shaking)에서 먼저 서버와 클라이언트 간에 세션키를 교환한다. 
  - 여기서 세션키는 주고 받는 데이터를 암호화하기 위해 사용되는 대칭키이며, <span style="color:#9fb584">**데이터 간의 교환에는 빠른 연산 속도가 필요하므로 세션키는 대칭키로 만들어진다.**</span>
  - 세션키를 클라이언트와 서버가 어떻게 교환하는 과정에서 비대칭키가 사용된다.
  - 즉, 처음 연결을 성립하여 안전하게 세션키를 공유하는 과정에서 비대칭키가 사용되는 것이고, 이후에 데이터를 교환하는 과정에서 빠른 연산 속도를 위해 대칭키가 사용되는 것이다.
- 443번 포트를 사용한다.

<br/> 


<details>
<summary>대칭키, 비대칭키, 디지털 서명, CA(Certificate Authority)와 인증서 💡</summary>
<div markdown="1">

#### 대칭키

- 클라이언트와 서버가 동일한 키를 사용해 암/복호화를 진행한다. 
- 키가 노출되면 위험하지만 연산 속도가 빠르다.
- 디지털 서명이 불가능하다.

<br/> 

#### 비대칭키

- 1개 쌍으로 구성된 공개키와 개인키를 암/복호화 하는데 사용한다. 
- 키가 노출되어도 비교적 안전하지만 연산 속도가 느리다.
- 암호화키와 복호화키가 다르다. 
- 개인키로 암호화한 것은 공개키로만 복호화할 수 있고, 공개키로 암호화한 것은 개인키로만 복호화할 수 있다.
  - ex) A ➡️ B 데이터 전송
  - A가 B의 공개키로 데이터를 암호화하여 보낼 경우 B는 자신의 개인키로 이를 복호화함으로서 자신에게 전송한 데이터임을 알 수 있다.
  - A가 B에게 A의 개인키로 데이터를 암호화하여 보내고 B는 A의 공개키로 데이터를 복호화함으로서 A가 보낸 데이터인 것을 알 수 있다.(디지털 서명)
    - 전송 과정에서 C가 A의 공개키로 데이터를 확인할 수 있지만, 출처는 속일 수 없다.

<br/> 

#### 디지털 서명

- 네트워크에서 송신자의 신원을 증명하는 방법으로, 송신자가 자신의 비밀키로 암호화한 메시지를 수신자가 송신자의 공용 키로 해독하는 과정이다.
- 즉, A가 자신의 개인키로 암호화한 것을 B가 복호화함으로서 B는 A의 신원을 확인할 수 있는 것을 말한다.
- 변경을 방지하는 것이 아닌 변경을 검출하는 것이다.
- 데이터 무결성
  - 데이터가 수정되면 다른 서명이 생성되기 때문에 데이터가 전송되는 동안 변경되지 않았음을 검증할 수 있다.
- 부인 방지
  - 서명을 작성할 수 있는 개인키는 자신만 가지고 있기 때문에 서명한 사실을 부인할 수 없다.

<br/> 

#### CA(Certificate Authority)와 인증서

- 클라이언트에서 서버가 신뢰할 수 있는 대상인지 확인하는 방법이다.
- 서버가 CA에 신원을 제출하면 CA가 이를 검증한 뒤 전자 서명(CA의 개인키로 암호화한 내용)이 들어간 `인증서`를 서버 측에 건내준다.
  - 인증서에는 CA의 정보, 서버의 암호화 방식, CA의 `전자서명`, 그리고 `서버의 랜덤생성 데이터`, `서버의 공개키` 등의 정보가 담겨있다.
- 서버는 해당 인증서를 클라이언트에게 전달한다.
- 클라이언트의 웹브라우저에는 CA의 목록과 공개키를 가지고 있는데, 이를 이용하여 인증서를 복호화한다.
- 복호화가 완료될 경우 서버가 신뢰할 수 있는 대상임을 알 수 있다.

</div>
</details>


<br/> 

#### SSL/TLS

- HTTP와 TCP 사이에서 동작하는 보안 레이어이다.

#### SSL(Secure Sockets Layer) 

- 암호화 기반 인터넷 보안 프로토콜이다.
- 서버와 클라이언트 애플리케이션이 네트워크로 통신하는 과정에서 전달되는 모든 데이터를 암호화하고 특정한 유형의 사이버 공격도 차단한다. 
- 데이터를 가로채려해도 복호화가 거의 불가능하다. 
- SSL은 클라이언트와 서버간에 핸드셰이크를 통해 `인증`이 이루어진다. 
- 또한 데이터 무결성을 위해 데이터에 디지털 서명을 하여 데이터가 의도적으로 도착하기 전에 조작된 여부를 확인한다.

#### TLS (Transport Layer Security) 

- SSL의 업데이트 버전이다.
- 클라이언트와 서버거 TCP에서 3-way handshake로 연결을 만들고 나면, TLS가 작동한다.
- 클라이언트와 서버 간 신원 확인, 암호화 통신 준비를 순서대로 진행한다.
- 위의 handshake 과정을 끝내고 나면 실제 통신이 이뤄진다.
  - application layer에서 발생한 모든 데이터들이 Session key에 의해 암호화되어 전송된다.

#### TLS 통신 - 클라이언트와 서버 간 신원 확인 (Handshake Protocol)

##### 1. ClientHello `client ➡️ server` 

- 클라이언트가 서버에게 자신이 생성한 랜덤데이터, 사용 가능한 암호화 & 압축 방식 목록 등을 서버에게 전달한다.

##### 2. ServerHello `server ➡️ client`

- 서버가 클라이언트에게 응답하는 메시지이다. 
- ClientHello에서 자신이 사용할 암호화, 압축 방식 등을 확인한다. 
- 그 후 클라이언트에게 자신이 생성한 랜덤데이터, 세션 ID와 함께 선택화 암호화 & 압축 방식을 클라이언트에게 보낸다.

##### 3. Certificate `server ➡️ client`

- 서버는 클라이언트에게 자신의 인증서를 보내고, 클라이언트가 이를 검증한다. 
- 이 과정에서 클라이언트는 CA의 신뢰성을 확인한다. 

##### 4. Server Key Exchange `server ➡️ client`

- 인증서 안에 서버의 공개키가 없는 경우 해당 메시지를 보낸다.

##### 5. Server Hello Done `server ➡️ client`

- Certificate까지 전송 했으면 해당 메시지를 클라이언트에게 전송한다.

#### TLS 통신 - 암호화 통신 준비 (Handshake Protocol)

##### 1. Client Key Exchange `client ➡️ server`

- 클라이언트는 서버에게 `pre-master secret`을 전달한다. 
- 클라이언트는 자신의 랜덤 데이터와 인증서에 담겨있던 서버의 랜덤 데이터를 이용해 pre-master secret를 생성한다.

##### 2. Change Cipher Spec `client ➡️ server`

- Change Cipher Spec Protocol이다.
- 핸드쉐이크에 의해 협상된 암호화 방식이 적용됨을 알린다. 
- 이제부터 암호화하여 통신한다는 의미이다.

##### 3. Encrypted handshake message(finished) `client ➡️ server`

- 핸드쉐이크가 종료됨을 알린다.

##### 4. Change Cipher Spec `server ➡️ client`

##### 5. Encrypted handshake message(finished) `server ➡️ client`

<br/>

#### pre-master secret

- 세션 단계에서 데이터를 실제로 주고 받을 때 데이터들을 암호화하기 위해 사용되는 것이다. 
- 클라이언트와 서버는 동일한 pre-master secret을 이용해 `master secret`을 만들고 master secret을 이용해 `session key(대칭키)`를 만든다. 
- session key로 데이터들을 암호화하여 사용한다.
- 즉, <span style="color:#9fb584">**서버와 클라이언트는 동일한 pre-master secret로 동일한 session key를 생성해서 데이터 암호화에 사용**</span>한다.
- 클라이언트의 입장에서 중요한 것은 pre-master secret을 안전하게 전송하는 것이다.
  - 그렇지 않을 경우 제 3자가 통신을 가로챌 수 있다.
- 이를 위해 pre-master secret을 서버의 공개키로 암호화하는 방법을 사용한다. 
  - 서버의 공개키는 이전 과정에서 서버가 보낸 인증서나 Server Key Exchange 메세지에서 확인할 수 있다.
- 서버는 암호화된 것을 자신의 개인키로 복호화함으로서 pre-master secret을 확인할 수 있다. 

<br/>


#### HTTPS 연결 과정

1. 클라이언트(브라우저)가 서버로 최초 연결을 시도한다.
2. 서버는 공개키(인증서)를 브라우저에게 넘겨준다.
3. 브라우저는 인증서의 유효성을 검사하고 `pre-master secret`을 전달한다.
4. 클라이언트와 서버는 동일한 pre-master secret을 이용해 `master secret`을 만들고 master secret을 이용해 `session key(대칭키)`를 만든다.
5. 브라우저는 세션키를 보관하며 추가로 서버의 공개키로 세션키를 암호화하여 서버로 전송한다.
6. 서버는 개인키로 암호화된 세션키를 복호화하여 세션키를 얻는다.
7. 클라이언트와 서버는 동일한 세션키를 공유하므로 데이터를 전달할 때 세션키로 암호화/복호화를 진행한다.


<br/> 

### iOS에서의 보안 통신 방법

- iOS 9.0, macOS 10.11 이상부터 <span style="color:#9fb584">**App Transport Security (ATS)**</span>가 도입되어 앱이 기본적으로 안전한 네트워크 연결(HTTPS)을 사용하도록 권장한다.
- 모든 앱과 앱 확장 프로그램에 대해 개인 정보 보호화 데이터 무결성을 향상시킨다.
- 기본적으로 암호화되지 않은 정보(HTTP)를 허용하지 않는다. 
  - HTTP 통신은 OS 내부에서 강제적으로 차단된다.
  - HTTPS 사용 권장
- HTTPS라도 최소 보안 사양을 충족하지 못하면 연결을 차단한다.
  - 서버는 TLS(Transport Layer Security) 프로토콜 버전 1.2 이상을 지원해야 한다.
  - 적어도 2048비트 이상의 RSA 키 또는 256비트 이상의 ECC(Elliptic-Curve) 키가 있는 SHA256을 인증서에 사용해야 한다.
  - 암호 연결은 허용된 암호 목록으로 제한한다.
- 필요한 경우 예외를 등록할 수 있지만, 보안을 저하시키는 문제가 있다.
- `URLSession`, `CFURL`, `NSURLConnection` API를 이용해 데이터를 주고받을 때 ATS 기능을 기본적으로 사용하게 된다.

<br/> 

## ✏️ HTTP 프로토콜의 특징과 HTTP/1.1과 HTTP/2의 차이점

---

### HTTP 특징

- <span style="color:#9fb584">**클라이언트 서버 구조**</span>이다.
  - 클라이언트는 서버에 요청을 보내고 응답이 올 때까지 대기한다.
  - 서버는 클라이언트에서 받은 요청에 대한 결과를 만들어 응답한다.
- <span style="color:#9fb584">**무상태(Stateless) 프로토콜**</span>이다.
  - 서버가 클라이언트의 상태를 보존하지 않는다.
  - 응답과 요청이 독립적이다.
  - 서버 확장성이 높다.
    - 항상 같은 서버에 연결될 필요가 없기 때문에 무한한 서버 증설이 가능하다.
  - 클라이언트가 추가 데이터를 전송해야 한다.
  - 로그인과 같이 유저의 상태를 유지해야하는 서비스라면, 브라우저 쿠키, 서버 세션, 토큰 등을 이용해 상태를 유지해야 한다.
- HTTP 1.0을 기준으로 HTTP는 기본적으로 <span style="color:#9fb584">**비연결성(Connectionless)**</span>이다.
  - TCP/IP는 기본적으로 연결을 유지한다.
    - 클라이언트가 요청을 보내지 않아도 계속 연결을 유지해야 한다.
    - 연결을 유지하는 서버 자원이 계속 소모된다.
  - 비연결성을 가지는 HTTP에서는 실제 요청을 주고 받을 때만 연결을 유지하고 응답을 주고 나면 TCP/IP연결을 끊는다.
    - 요청 ➡️ TCP/IP 연결 ➡️ 응답 ➡️ TCP/IP 연결 종료
    - <span style="color:#9fb584">**최소한의 자원으로 서버를 유지**</span>할 수 있다.
  - 일반적으로 초 단위 이하의 빠른 속도로 응답한다.
    - 1시간 동안 수천 명이 서비스를 사용해도 실 서버에서 처리하는 요청은 수십 개 이하로 매우 작다.
    - 웹 브라우저에서 연속해서 검색 버튼을 누르지 않기 때문이다.
  - 비연결성은 트래픽이 많지 않고 빠른 응답을 제공할 수 있는 경우 효율적이다.
  - 트래픽이 많고 큰 규모의 서비스를 운영할 경우 비효율적이다.

<br/>

#### HTTP 1.0

- 매 요청마다 연결과 종료를 반복해야 한다.
  - TCP/IP 연결을 매번 새로 맺어야하므로 3 way handshake 로 인한 응답 지연이 발생한다.
  - 웹 브라우저로 사이트를 요청하면 HTML, CSS, JavaScript, 추가 이미지 등 수많은 자원이 함께 다운로드되는데, 이러한 자원들을 각각 보낼 때마다 연결을 끊고 다시 연결하고를 반복하는 것은 매우 비효율적이다.

<br/>

#### HTTP 1.1

- `Persistent connection` 모델과 더 발전한 `HTTP Pipelining` 모델이 추가되었다.
- Persistent connection은 연결의 지속 시간을 설정했다.
  - 설정해둔 연결 지속 시간에는 1.0과 같은 연결/종료 과정을 거치지 않아도 된다.
  - TCP 특성상 요청 후 응답을 기다려야하고, 대기 시간 동안에는 동작을 수행할 수 없다.
- HTTP Pipelining은 Persistent connection의 문제점을 개선했다.
  - 클라이언트는 요청을 응답에 상관없이 보내고 서버에서는 <span style="color:#9fb584">**요청이 들어온 순서대로**</span> 응답을 보낸다.
  - 클라이언트에서는 응답의 순서가 잘못되거나 누락되었을 경우 해당 요청을 다시 보낸다.
  - 요청을 한 번에 보내 왕복 시간(Round-Trip Time)을 줄일 수 있어 응답 지연(Latency)시간을 줄여준다.
  - HOL(Head Of Line) Blocking 문제가 발생한다.
    - 같은 큐에 있는 패킷이 첫번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상이다.
    - 패킷 처리 속도가 지연되고 최악의 경우 드랍될 수 있다.
    - ex) 1번(처리시간 : 100초), 2번(20초), 3번(30초) 요청이 도착했을 때 
    - 서버는 2, 3번 요청을 빠른 시간에 처리할 수 있지만 1번 응답을 먼저 보내야 하므로 2, 3번 요청에 대한 응답이 blocking 된다.
- 무거운 헤더 문제가 발생한다.
  - 매 요청마다 중복되는 헤더를 전송하고 쿠키 정보도 요청마다 헤더에 포함하여 전송한다.
  - 요청한 데이터보다 헤더 값이 더 커지는 상황이 발생한다.
  - 불필요한 정보를 주고 받는 자원 낭비가 발생한다.

<br/>

#### HTTP 2.0 용어 설명

![http2.0](/assets/img/http2.0.png)

- Stream : 연결된 Connection 내에서 하나 이상의 메시지를 양방향으로 주고 받는 양방향 바이트 흐름으로 요청과 응답을 양방향으로 오가는 논리적 연결 단위이다.
- Message : HTTP 1.1과 마찬가지로 하나의 요청과 응답을 구성하는 단위로 다수의 Frame으로 이뤄진 배열 라인이다.
- Frame : HTTP 2.0에서 가장 작은 통신 단위로 헤더/데이터가 포함되어 있다.
- 요청/응답 메시지를 나눈 여러 Frame이 특정 Stream에 속하여 여러 Stream은 하나의 Connection에 속한다.

#### HTTP 2.0 주요 기능

- Binary Framing
  - 1.1에서 text형식으로 전달되던 요청, 응답 메시지가 프레임 단위로 나눠지고 바이너리 형식으로 인코딩 된다.
    - 바이너리는 컴퓨터가 이해하기 쉽기 때문에 파싱, 전송 속도가 향상된다.
- Multiplex Streaming
  - 모든 통신은 단일 TCP Connection에서 수행되며, 한 Connection안에서 원하는 수만큼의 양방향 Stream을 전달할 수 있다.
    - 한 커넥션에서 여러 스트림이 동시에 요청/응답을 수행한다.
    - 여러 요청을 병렬적으로 처리할 수 있다.
      - 각 스트림의 요청/응답에 고유 식별자가 존재하여 프레임이 전달되었을 때 해당 프레임이 어느 스트림의 것인지 식별할 수 있다.
      - 요청/응답 프레임이 각각 어떤 스트림에 속하는지 알 수 있기 때문에 순서와 상관없이 요청/응답을 보내며 병렬 처리가 가능하다.
  - 각 스트림은 고유 식별자와 우선순위 정보를 포함한다.
  - 각 메시지는 하나 이상의 프레임으로 구성된 논리적 HTTP 메시지(요청/응답)이다.
  - 서로 다른 Stream의 Frame은 서로 인접하지 않으며, 각 Frame의 헤더에 포함된 Stream 식별자를 통해 재조립된다.
    - 프레임을 전달받은 쪽에서는 프레임의 헤더에 있는 스트림 식별자를 통해 프레임을 재조립하여 요청/응답 메시지를 확인할 수 있다.
- 스트림 우선 순위 지정
  - HTTP 메세지를 여러 Binary Frame으로 분할하고, 여러 Frame을 멀티플렉싱 할 수 있게되면서 요청과 응답이 동시에 병렬적으로 이루어져 속도가 빠르다.
  - 하나의 커넥션에 여러 요청/응답이 섞이며 스트림의 우선 순위를 지정할 필요가 생기게 되었다.
  - 응답에 대한 우선 순위를 설정한다.
- 헤더 압축
  - 헤더에 중복된 정보들이 많아 리소스 낭비가 심했던 1.1을 개선했다. 
  - Header Table과 Huffman Encoding을 사용하는 HPACK 알고리즘을 사용한다.
  - 클라이언트와 서버는 각각 Header Table을 관리한다.
  - 이전 요청과 동일한 필드는 table의 index만 보내고 변경되는 값은 Huffman Encoding 후 보냄으로써 Header의 크기를 경량화 한다.
  - 정적 테이블은 공용 HTTP 헤더 필드를 제공하고 동적 테이블은 특정 연결시에만 값에 따라 업데이트 된다.
- Sever Push
  - 특정 HTML문서를 클라이언트에서 요청하면 HTML 문서에 포함된 리소스들(CSS, JS)을 추가로 요청할 것이라는 예측을 할 수 있다.
  - 클라이언트에서 HTML문서에 포함된 리소스들에 대한 추가 요청을 보내지 않아도 서버에서 알아서 전송해주는 것이다.
  - 클라이언트가 요청하기 전 리소스들을 Push 해주는 방법으로 클라이언트의 요청을 최소화해 성능을 향상시킨다.

<br/> 

#### HTTP 2.0 한계

- TCP로인한 여전한 RTT(Round Trip Time)가 발생한다.
  - 멀티플렉싱으로 성능을 향상시켰지만, HTTP 2.0도 여전히 TCP위에서 동작한다. 
  - Handshake에 대한 RTT로 지연이 발생한다.
- TCP 자체의 Head Of Line Blocking
  - TCP에서 패킷이 유실되거나 오류가 발생할 경우 전달을 보장하기 때문에 패킷을 재전송한다.
  - 재선송이 발생하면 패킷의 순서가 역전되지 않도록 후속 패킷이 대기하게 된다.
  - 즉, TCP 상에서 패킷을 보낼 때 먼저 보낸 패킷에서 손실이 발생하면 뒤 패킷이 블락된다. (HOL Block)
  - HTTP 2.0에선 애플리케이션 계층의 HOLB를 해결했지만, 전송 계층에서의 HOLB는 여전히 발생하게된다.
- TCP의 혼잡 제어
  - TCP는 혼잡 제어를 수행한다.
    - 혼잡 제어 : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법
  - 이는 네트워크 상황이 좋을 때 사실상 불필요한 지연을 발생시킨다.

<br/> 

#### HTTP 3.0

- HTTP 1.1과 HTTP 2.0은 TCP를 전송에 사용하지만, HTTP/3은 UDP(QUIC)를 사용한다.
- HTTP 3.0은 HTTP 2.0가 가지는 장점들을 모두 가지면서 TCP가 가지는 원초적인 단점을 보완하는 것을 중점으로 개발되었다.
- RTT를 제로 수준으로 줄였고, 패킷 손실에 대한 빠른 대응, 사용자 IP가 바뀌어도 연결이 유지되는 것이 특징이다.

<br/> 

## ✏️ TCP와 UDP의 특징과 차이점

---

### TCP 전송 제어 프로토콜 (Transmission Control Protocl)

- 인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜이다.
  - IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다.
- 연속성보다 신뢰성있는 전송이 중요할 때에 사용한다.
- 논리적 경로(가상회선방식)를 설정한다.
  - 패킷들이 여러 경로가 있음에도 하나의 경로(논리적 경로, 가상회선)로 전달된다.
- 연결 지향 방식으로 패킷을 교환한다.
  - 연결 기반으로 데이터를 전송하는 동안 수신자와 발신자 사이 연결을 설정하고 이를 유지한다.
  - 3-way handshaking 과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제한다.
- 높은 신뢰성을 제공한다.
  - 패킷 손실, 중복, 순서 바뀜 등이 없도록 보장한다.
  - 데이터를 전송할 때 오류를 검사하여 전송된 데이터가 목적지에 온전하게 도달하도록 보장한다.
  - 데이터 수신 여부를 확인하고 첫 번째 전송이 실패한 경우 재전송을 시도한다.
- 흐름 제어 기능을 제공한다.
  - 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지한다.
- 혼잡 제어 기능을 제공한다.
  - 네트워크 내의 패킷 수가 과도하게 증가하지 않도록 방지한다.
- 많은 대역폭을 사용하며 UDP보다 속도가 느리다.
  - 신뢰성 제공, 흐름 제어, 혼잡 제어로 인한 속도 저하
- 양방향 통신이며, 서버와 클라이언트가 1:1로 연결된다.
  - 전이중(Full-Duplex), 점대점(Point to Point) 방식
  - 전이중 : 전송이 양방향으로 동시에 일어날 수 있다.
  - 점대점 : 각 연결이 정확히 2개의 종단점을 가지고 있다.

### UDP 사용자 데이터그램 프로토콜 (User Datagram Protocol)

- 데이터를 데이터그램 단위(독립적인 관계를 지니는 패킷)로 처리하는 프로토콜이다.
- 신뢰성보다는 연속성이 중요한 서비스에서 사용한다.
- 서버와 클라이언트가 1대1, 1대N, N대M 등으로 연결될 수 있다.
- 비연결형 서비스로 데이터그램 방식을 제공한다.
  - 연결을 위해 할당되는 논리적인 경로가 없다.
  - 각각 패킷은 다른 경로로 전송되고, 독립적인 관계를 지닌다.
  - 특정 순서로 메시지를 전송하지 않는다.
- TCP보다 더 빠르고 효율적이다.
- 헤더가 단순하다.
  - 헤더는 고정 크기의 8바이트(TCP : 20바이트)만 사용한다.
- 신뢰성이 없다.
  - 데이터 수신 여부를 확인하지 않는다. (확인 응답 없음)
  - 전송이 온전하게 도착한다고 보장할 수 없다.
    - 일부 패킷이 손실될 수 있지만 발신차 측에서 확인할 수 없다.
  - 헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
  - 흐름 제어를 위한 피드백을 제공하지 않는다.
- 실시간 응용 및 멀티캐스팅이 가능하다.
  - 빠른 요청과 응답이 필요한 실시간 응용에 적합하다.
  - 브로드캐스트 및 멀티캐스트 기능을 통해 하나의 UDP 전송을 여러 수신자에게 한 번에 전송할 수 있다.

<br/> 

### ✏️ 연결 지향형 프로토콜과 비연결 지향형 프로토콜은 무엇인가요?

#### 연결 지향적 프로토콜 

- 클라이언트와 서버가 연결된 상태에서 데이터를 주고받는 프로토콜을 의미한다.
- 연결을 계속 유지하기 위한 비용이 든다.
- 프로토콜에 의해 연속적으로 패킷 상태 정보를 유지한다.
- 1:1 연결
- ex) TCP

<br/> 

#### 비연결형 프로토콜 

- 연결을 위해 할당되는 논리적인 경로가 없고, 각각의 패킷은 다른 경로로 전송되며, 독립적인 관계를 지닌다.
- 연결을 유지하는 비용이 들지 않는다.
- 매번 새롭게 연결이 성립되기 때문에 필요한 경우 매 연결 시 자신이 누구인지 알려줘야 한다. 
- 1대1, 1대N, N대M 연결
- ex) IP, UDP

> IP프로토콜은 비연결 프로토콜이지만 IP 프로토콜을 이용하는 TCP 프로토콜은 연결지향 프로토콜이다. <br/>TCP프로토콜을 이용하는 HTTP 프로토콜은 비연결 프로토콜이다.
<br/> 

### ✏️ TCP의 3-way handshake 과정은 어떻게 이루어지나요?

1. 먼저 Open 한 클라이언트가 SYN(synchronize sequence numbers)를 보내고 `SYN_SENT` 상태로 대기한다.
2. `LISTEN` 상태의 서버는 상태를 `SYN-RECEIVED`로 바꾸고 SYN과 응답 ACK를 보낸다.
3. SYN과 응답 ACK를 받은 클라이언트는 `ESTABLISHED` 상태로 변경하고 서버에게 응답 ACK를 보낸다.
4. 응답 ACK를 받은 서버는 `ESTABLISHED` 상태로 변경한다.

#### 상태

- `CLOSED` :연결 수립을 시작하기 전의 기본 상태 (연결 없음)
- `LISTEN` : 포트가 열린 상태로 연결 요청 대기 중
- `SYN-SENT` : SYN을 요청한 상태
- `SYN-RECEIVED` : SYN 요청을 받고 상대방의 응답을 기다리는 중
- `ESTABLISHED` : 연결 수립이 완료된 상태, 서로 데이터를 교환할 수 있다.

#### TCP 연결 해제 과정 4-way handshake

1. `ESTABLISHED` 상태의 클라이언트가 앱에서 close 시그널을 받는다.
2. 클라이언트가 FIN을 보내고 `FIN-WAIT-1` 상태로 대기한다.
3. `ESTABLISHED` 상태의 서버는 `CLOSE-WAIT`으로 상태를 바꾸고 응답 ACK를 전달한다.
   1. 동시에 해당 포트에 연결되어 있는 애플리케이션에게 close를 요청한다.
4. ACK를 받은 클라이언트는 상태를 `FIN-WAIT-2`로 변경한다.
5. close 요청을 받은 서버 애플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트로 보내 `LAST_ACK` 상태로 바꾼다.
6. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME-WAIT`으로 상태를 바꾼다.
   1. `TIME-WAIT`에서 일정 시간이 지나면 `CLOSED` 된다. 
7. ACK를 받은 서버도 포트를 `CLOSED`로 닫는다.

#### 상태

- `FIN-WAIT-1` : 자신이 보낸 FIN에 대한 ACK를 기다리거나 상대방의 FIN을 기다린다.
- `FIN-WAIT-2` : 자신이 보낸 FIN에 대한 ACK를 받았고, 상대방의 FIN을 기다린다.
- `CLOSE-WAIT` : 상대방의 FIN(종료 요청)을 받은 상태. 상대방 FIN에 대한 ACK를 보내고 어플리케이션에 종료를 알린다.
- `LAST-ACK` : COLSE-WAIT 상태를 처리 후 자신의 FIN 요청을 보낸 후 FIN에 대한 ACK를 기다리는 상태.
- `TIME-WAIT` : 모든 FIN에 대한 ACK를 받고 연결 종료가 완료된 상태. 새 연결과 겹치지 않도록 일정 시간 기다린 후 CLOSED로 전이 한다.
- `CLOSED` : 연결 수립을 시작하기 전의 기본 상태 (연결 없음)

<br/> 

### ✏️ 어떤 상황에서 UDP를 사용하는 것이 적합한가요?

- 실시간 통신이 필요한 온라인 게임, 비디오나 오디오 스트리밍 서비스 같은 곳에서 UDP를 사용한다.
- 데이터의 순서나 완전성보다 빠른 데이터 전송 속도가 중요하기 때문이다.
  - 온라인 게임에서는 플레이어의 움직임이나 상태 변경이 실시간으로 전송되어야한다.
  - 비디오나 오디오 데이터의 경우, 약간의 패킷 손실이 발생해도 전체적인 사용자 경험에 큰 영향을 주지 않는다.

> TCP는 데이터의 정확성이 중요한 웹 애플리케이션, 이메일 전송, 파일 전송 등에 주로 사용한다. 데이터 패킷의 순서와 신뢰성이 중요하기 때문이다. 

<br/> 

## ✏️ 소켓 통신

---

> 소켓 : 소켓은 떨어져 있는 두 호스트를 연결해주는 도구로써 인터페이스 역할을 한다. 데이터를 주고 받을 수 있는 구조체로 소켓을 통해 데이터 통로가 만들어진다. 네트워크 통신을 하려면 각 TCP/IP 계층의 협력을 통해 이뤄진다. 소켓은 그 중 전송 계층과 응용 계층 사이에 있는 인터페이스이다. 즉, 통신의 시작점과 종료점 end-point라고 볼 수 있다. 소켓을 이용해 TCP나 UDP에 접근할 수 있다. 현재 대부분의 통신은 인터넷 프로토콜(TCP, UDP)에 기반하고 있으므로 대부분의 네트워크 소켓은 인터넷 소켓이다.<br/>
> IP : 전 세계 컴퓨터에 부여된 고유 식별 주소<br/>
> 포트 Port : 네트워크 상에서 통신하기 위해 호스트 내부적으로 프로세스가 할당받아야 하는 고유한 숫자이다. 한 호스트 내에서 네트워크 통신을 하고 있는 프로세스를 식별하기 위해 사용되는 값으로 같은 호스트 내에서 서로 다른 프로세스가 같은 포트 넘버를 가질 수 없다. 즉, 같은 컴퓨터 내에서 프로그램을 식별하는 번호이다.

- 네트워크를 통해 서버-클라이언트 양쪽에 링크를 생성 한 후 해당 링크를 통해 데이터를 주고 받는 기술이다.
- HTTP 통신은 클라이언트가 요청을 보낼 때만 서버가 응답하고 그 과정이 끝나면 연결이 종료되는 단방향 통신이다.
- 소켓 통신은 클라이언트가 요청을 보내 서버와의 포트를 열어 <span style="color:#9fb584">**연결을 유지하여 통신하는 방식으로 양방향 통신**</span>이다.
  - HTTP 통신의 데이터 요청을 계속 해야 하는 단점을 해결한다.
- 서버에서 임의의 포트 번호를 설정하고 클라이언트에서 해당 포트로 접속하면 서버와 클라이언트가 연결된다.
  - 연결이 되어 있어 데이터가 들어오면 바로바로 처리 가능하다.
- <span style="color:#9fb584">**실시간 통신**</span>으로, 실시간으로 데이터를 주고 받는 상황이 필요한 경우 사용한다.
  - 채팅 및 스트리밍 등
- 가볍고 간결하여 빠른 속도로 데이터를 전달한다.
- TCP (스트림 소켓) / UDP (데이터 그램 소켓) 방법이 있다.

<br/> 

### TCP 소켓

- 연결형 프로토콜로 연결 후 통신할 수 있다.
- 데이터를 메시지 형태로 보내기 위해 IP와 함께 사용한다.
- 패킷 추적 관리를 한다.
  - 나눠진 데이터의 손실 여부를 판단하고 재조립하는 과정을 추적한다.
  - 패킷은 온전한 1개의 데이터가 아니다.
  - 데이터는 여러 개로 나눠 보내지며 나눠진 데이터는 다시 온전히 합쳐져야 한다.
    - 데이터 ➡️ Segmentation  ➡️ Segment ➡️ 패킷
- 패킷을 수신할 때마다 ACK 패킷을 재전송하고 재전송을 받지 못하면 일정 시간 후 다시 해당 패킷을 재전송하여 안정성을 높인다.
- UDP에 비해 전송 속도가 느리다.
- 3-way handshaking으로 연결을 설정하고 4-way handshaking으로 해제한다.

<br/> 

### UDP 소켓

- 비연결형 프로토콜으로 연결 없이 데이터를 전송만 한다.
- 데이터를 데이터그램(독립적 단계를 지닌 패킷) 단위로 처리한다.
- TCP는 일대일 교환이면 UDP는 일대일/일대다/다대다 통신 교환 방식이다.
- TCP에 비해 신뢰 및 안정성이 낮다.
- 데이터 순서 변경이나 데이터가 유실될 가능성도 많지만 속도가 빠르다.

<br/> 

<details>
<summary>URLSessionWebSocket 💡</summary>
<div markdown="1">

### URLSessionWebSocket

- Web Socket : 하나의 TCP 접속에 전이중 통신 채널을 제공하는 컴퓨터 통신 프로토콜이다. 
  - HTTP나 HTTPS 위에서 동작하도록 설계되었으며, 포트는 80번 혹은 443번이다. 
  - HTTP 프로토콜과 호환이 된다.
  - OSI 7계층 기준으로 소켓은 인터넷 프로토콜에 기반하므로 TCP, UDP가 속한 Network계층에 위치하며 웹 소켓은 TCP에 의존하지만 HTTP에 기반하므로 Application계층에 위치한다.
  - TCP에 기반한 소켓 통신은 바이트 스트림을 통한 데이터 전송이므로 바이트로 이루어진 데이터를 다뤄야하지만, 웹소켓 통신은 어플리케이션 계층에 기반하기 때문에 메시지  형식의 데이터를 다루게 된다.
  - 웹 소켓은 TCP 소켓과 구분되는 것이 아니라 TCP 소켓의 추상화된 형태이다.
- WebSockets API를 통해 서버로 메세지를 보내면, 별다른 API 요청 없이 응답을 수신한다.
- 접속에만 HTTP를 사용하고 그 후 통신은 WebSockets 독자적인 프로토콜을 사용한다.
- WebSockets은 header가 작기 때문에 overhead가 적다.
- iOS 13부터 WebSocket 지원을 시작했고, `URLSessionWebSocketTask`을 사용해 웹 소켓을 사용할 수 있다.
  - Starscream, SwiftWebsocket과 같은 third-party 라이브러리에 의존하지 않아도 된다.
  - 타사 라이브러리를 사용하는 오버헤드가 없다.
- `URLSessionWebSocketTask`는 URLSessionTask의 하위클래스로 구성된다.
- WebSocket 프레임의 형태로 TCP와 TLS 형태로 메시지 지향의 전송 프로토콜을 제공한다.
- RFC 6455에 정의된 WebSocket Protocol을 따른다.

</div>
</details>

## ✏️ REST API와 iOS에서의 네트워크 요청 및 응답 처리 방법

---

### REST API (Representational State Transfer)

- REST 원칙에 따라 설계된 API로, 자원을 이름으로 구분해 정보를 주고받을 수 있도록 한 것이다.
- REST는 웹 서비스 설계 아키텍처로 클라이언트와 서버 간 상호 작용을 단순화하고 표준화하는데 중점을 둔다.
- REST는 행위, 자원, 메시지로 구성되어 있다.
  - 행위 Verb : HTTP 메소드 (GET, POST 등)
  - 자원 Resource : 서버에 접근하기 위한 URL (Base URL이라는 공통적인 주소와 각 API별로 추가적인 주소를 붙여서 구분)
  - 메시지 Representation : 서버에서 요청한 결과값 (보통 JSON, XML 형태)
- HTTP를 사용하여 데이터를 생성, 읽기, 업데이트, 삭제(CRUD)하는 표준 데이터베이스 기능을 수행한다.
- 요청 헤더와 매개변수 역시 메타데이터, 권한 부여, URI, 캐싱, 쿠키 등의 식별자 정보를 포함하기에 REST API 호출에서 중요하다. 
- 요청 헤더와 응답 헤더는 일반적인 HTTP 상태 코드와 함께 사용된다.

> iOS에서 서버 통신을 한다는 것은 클라이언트가 서버의 URI를 가지고 HTTP 메서드를 포함한 요청을 보내면, JSON 형태의 데이터를 받게 되는 것을 의미한다.<br/>
= 자원(Resource)으로 접근해서, 행위(Verb)를 서버에게 알려주고, 메시지(JSON)를 클라이언트가 받는 프로세스

<br/> 

#### REST API 특징

- `무상태성 stateless` : 각 요청은 클라이언트 상태를 서버에 저장하지 않고 독립적으로 처리된다.
  - 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다.
  - 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.
  - 서버 확장성을 높이고 클라이언트와 서버 간 의존성을 줄인다.
- `클라이언트-서버 구조` : 클라이언트와 서버는 독립적으로 개발될 수 있으며 클라이언트는 서버가 제공하는 API를 호출하여 필요한 데이터를 얻는다.
  - REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분된다.
  - 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어든다.
- `캐시 가능` : RESTful 응답은 HTTP 캐시 메커니즘을 사용할 수 있어 성능을 향상시킨다.
  - REST의 가장 큰 특징 중 하나는 HTTP 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능하다.
  - 따라서 HTTP가 가진 캐싱 기능이 적용 가능하다.
  - HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
- `계층형 구조` : REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있다.
  - 클라이언트와 서버 사이에 미들웨어(PROXY, 게이트웨이 같은 네트워크 기반의 중간매체)를 사용할 수 있다.
  - 시스템의 확장성과 보안성을 높인다.
- `Uniform 유니폼 인터페이스(통일된 인터페이스)` : 일관된 URI, HTTP 메소드, 상태 코드, 헤더 등을 사용해 클라이언트가 서버와 상호작용할 수 있다.
- `Self-descriptiveness (자체 표현 구조)`:  REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다.

<br/> 

#### REST API 사용 이유

- `단순성과 명확성` : REST는 HTTP 프로토콜을 기반으로 표준을 따른다. 학습하기 쉽고, 이해하기 쉽다.
- `유연성` : 다양한 클라이언트(웹, 모바일 앱 등)가 동일한 API를 사용할 수 있어 코드 재사용성을 높인다.
- `확장성` : 무상태성 덕분에 서버를 쉽게 확장할 수 있다.
- `성능` : 캐시를 사용함으로써 네트워크 성능을 최적화할 수 있다.

<br/> 

### iOS에서의 네트워크 요청 및 응답 처리 방법

- iOS에서는 REST API를 사용해 네트워크 요청을 처리할 때 주로 URLSession을 사용한다.
- 요청 생성 : URL, HTTP 메서드, 헤더, 바디 등을 설정
- 요청 전송 : URLSession을 사용하여 비동기적으로 요청 전송
- 응답 처리 : 서버로부터 응답을 받고, Codable 프로토콜을 사용해 JSON 등을 파싱하여 데이터 처리

<br/> 

### ✏️ iOS에서 URLSession을 사용하여 네트워크 요청을 보내는 방법은 무엇인가요?

- `URLSession`클래스의 `dataTask` 메소드를 사용하여 `data task`를 생성하여 네트워크 요청을 수행한다.

<br/> 

## ✏️ REST API에서 HTTP 메서드들의 차이점

---

- `GET` : 서버로부터 데이터 조회
- `POST` : 서버에 새로운 데이터 생성
- `PUT` : 서버의 데이터를 **전부** 업데이트
- `DELETE` : 서버의 데이터를 삭제
- `PATCH` : 서버의 데이터를 **부분적**으로 변경할 때 사용

<br/> 

## ✏️ HTTP 상태 코드에 대해서 설명해주세요.

---

- 응답 메시지의 상태라인에 있는 상태 코드를 보고 서버의 처리 결과를 파악할 수 있다.
- 상태 코드는 세 자리 숫자로 되어 있으며 첫 번째 숫자는 HTTP 응답의 종류를 구분하는데 사용한다.
- 나머지 두 자리 숫자는 세부적인 응답 내용을 구분하는데 사용한다.
- `1**` : Informational
  - 임시 응답으로 현재 클라이언트의 요청까지 처리되었으니 계속 진행하라는 의미이다.
  - HTTP 1.1버전부터 추가되었다.
- `2**` : Success
  - 클라이언트의 요청이 서버에서 성공적으로 처리되었다는 의미이다.
- `3**` : Redirection
  - 완전한 처리를 위해 추가 동작이 필요한 경우이다.
  - 주로 서버 주소 또는 요청한 URI의 웹 문서가 이동되었으니 해당 주소로 다시 시도하라는 의미이다.
- `4**` : Client Error
  - 없는 페이지를 요청하는 등 클라이언트의 요청 메시지 내용이 잘못된 경우를 의미한다.
- `5**` : Server Error
  - 서버에서 메시지 처리에 문제가 발생한 경우로 서버의 부하, DB 처리 과정 오류 등이 발생하는 경우를 의미한다.

### 상세 코드

- `100 (Continue 계속)` : 클라이언트가 요청 해더에 `Expect: 100-continue`를 보내고 서버가 이를 처리할 수 있으면 해당 코드로 응답한다. 
- `101 (Switching Protocols 프로토콜 전환)` : 프로토콜을 HTTP 1.1에서 업그레이드할 때 업그레이드 응답 헤더에 표시한다.
- `102 (Processing 처리중)` : 서버가 처리하는데 오랜 시간이 예상되어 클라이언트에서 타임 아웃이 발생하지 않도록 해당 응답 코드를 보낸다.

<br/>

- `200 (OK 성공)` : 서버가 요청을 성공적으로 처리하였다.
- `201 (Created 생성됨)` : 요청이 처리되어서 새로운 리소스가 생성되었다.
- `202 (Accepted 허용됨)` : 요청은 접수하였지만, 처리가 완료되지 않았다.
- `203 (Non-Authoritative Information 신뢰할 수 없는 정보)` : 응답 헤더가 오리지널 서버로부터 제공된 것이 아니다.
  - 프록시 서버가 응답 헤더에 주석을 붙인 경우 사용된다.
- `204 (No Content 콘텐츠 없음)` : 처리를 성공하였지만, 클라이언트에게 돌려줄 콘텐츠가 없다.
  - 응답에 헤더만 있고 바디가 없다.
  - DELETE 요청에 대한 응답으로 많이 사용된다.
- `205 (Reset Content 콘텐츠 재설정)` : 처리를 성공하였고 브라우저의 화면을 리셋하라.
- `206 (Partial Content 일부 콘텐츠)` : 콘텐츠의 일부만을 보낸다.
- `207 (Multi-Status 다중 상태)` : 처리 결과의 스테이터스가 여러 개이다.
  - 성공을 뜻하지만, 각각의 처리 결과가 성공인지는 바디를 봐야 알 수 있다.

<br/>

- `300 (Multiple Choices 여러 선택 항목)` : 선택 항목이 여러 개 있다.
  - 서버에서 콘텐츠를 결정하지 못하고 클라이언트에게 복수 개의 링크를 응답할 때 사용한다.
- `301 (Moved Permanently 영구 이동)` : 지정한 리소스가 새로운 URI로 이동하였다.
- `302 (Found 다른 위치 찾음)` : 요청한 리소스를 다른 URI에서 찾았다.
- `303 (See Other 다른 위치 보기)` : 다른 위치로 요청하라.
- `304 (Not Modified 수정되지 않음)` : 마지막 요청 이후 요청한 페이지는 수정되지 않았다.
- `305 (Use Proxy 프록시 사용)` : 지정한 리소스에 액세스하려면 프록시를 통해야 한다.
- `307 (Temporary Redirect 임시 리다이렉션)` : 임시로 리다이렉션 요청이 필요하다.

<br/>

- `400 (Bad Request 잘못된 요청)` : 요청의 구문이 잘못되었다.
- `401 (Unauthorized 권한 없음)` : 지정한 리소스에 대한 액세스 권한이 없다.
- `403 (Forbidden 금지됨)` : 지정한 리소스에 대한 액세스가 금지되었다.
- `404 (Not Found 찾을 수 없음)` : 지정한 리소스를 찾을 수 없다.

<br/>

- `500 (Internal Server Error 내부 서버 오류)` : 서버에 에러가 발생하였다.
- `501 (Not Implemented 구현되지 않음)` : 요청한 URI의 메소드에 대해 서버가 구현하고 있지 않다.
- `502 (Bad Gateway 불량 게이트웨이)` : 게이트웨이 또는 프록시 역할을 하는 서버가 그 뒷단의 서버로부터 잘못된 응답을 받았다.

<br/> 

## ✏️ 네트워크 요청 시 에러 처리는 어떻게 하나요?

---

- 네트워크 요청 시 에러 처리는 네트워크 통신의 신뢰성을 높이고, 사용자에게 적절한 피드백을 제공하기 위해 매우 중요하다.
- Swift에서 네트워크 요청 에러는 URLSession의 completion handler를 통해 전달되며, 주로 인터넷 연결 문제, 서버 오류, 데이터 파싱 오류 등 다양한 상황을 다룬다.

### 1. 에러 객체 확인

- 요청 결과의 completion handler에서 에러 객체가 전달되는지 확인하여 에러 발생 여부를 우선 판단한다.
- completion handler의 error가 nil이 아닌 경우, 네트워크 요청 중 연결 오류 또는 타임아웃 등의 문제가 발생했음을 의미한다.
- error.localizedDescription을 사용하여 오류 메시지를 확인할 수 있다.

### 2. HTTP 상태 코드 확인

- response의 HTTP 상태 코드를 검사하여, 정상적인 응답인지 또는 특정 오류 코드(예: 404, 500 등)인지 확인한다.
- 200...299: 성공 범위
- 400...499: 클라이언트 오류 (예: 잘못된 요청, 인증 실패)
- 500...599: 서버 오류 (예: 서버 내부 오류)


### 3. 데이터 파싱 오류 확인

- data가 존재하는 경우, JSON 파싱이나 모델 디코딩을 수행한다.
- Swift의 Codable을 사용하는 경우, decode 중 발생하는 오류를 do-catch 블록으로 처리할 수 있다.
- 데이터 형식이 예상과 다른 경우 발생하는 오류를 처리하여 앱이 크래시되지 않도록 한다.


### 4. 에러 메시지 사용자에게 전달

- 네트워크 에러 발생 시 사용자에게 적절한 메시지를 표시하여 상태를 알릴 수 있다.
- 에러가 발생하면 UIAlertController 등을 사용해 사용자에게 에러 메시지를 알리는 것이 좋다.

``` swift

// URLSessionConfiguration 설정, URLSession 생성
let session = URLSession(configuration: .default)
// URL 설정
guard let url = URL(string: urlString + "/annotations") else {
          print("Invalid URL")
          return
}

// URLRequest 설정
var request = URLRequest(url: url)
request.httpMethod = "POST"
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("\(userId)", forHTTPHeaderField: "userId")
request.httpBody = requestBody

// 데이터 작업 생성 및 실행
session.dataTask(with: request) { (data: Data?, response: URLResponse?, error: Error?) in
  // 1. 에러 객체 확인
  guard error == nil else {
      print("Error occur: insertAnnotation error calling POST - \(error.localizedDescription)")
      return
  }

  // 2. HTTP 상태 코드 확인
  guard let data = data, let response = response as? HTTPURLResponse {
    switch httpResponse.statusCode {
        case 200...299: // 성공 범위
            print("Success: Status code \(httpResponse.statusCode)")
        case 400...499:
            print("Client error: Status code \(httpResponse.statusCode)")
        case 500...599:
            print("Server error: Status code \(httpResponse.statusCode)")
        default:
            print("Unexpected status code: \(httpResponse.statusCode)")
        }
  }
  
  let dateDecoder = JSONDecoder()
  dateDecoder.dateDecodingStrategy = .formatted(self.dateformat)
  // 3. 데이터 파싱 오류 확인
  guard let decodedData = try? dateDecoder.decode(ResponseModel<Annotation>.self, from: data) else {
      print("Error: insertAnnotation JSON parsing failed")
      return
  }
  
  if decodedData.status == 201 {
      self.annotation = decodedData.data!
      completion(self.annotation)
  }
  else {
      print("insertAnnotation failed")
  }
// 작업 시작
}.resume()
```

> 네트워크 에러의 주요 유형<br/>
> 네트워크 연결 오류: 인터넷이 연결되지 않았거나 서버에 접근할 수 없는 경우<br/>
> 타임아웃 오류: 네트워크 응답이 지연되거나 서버가 응답하지 않는 경우<br/>
> 클라이언트 오류: 잘못된 요청 형식이나 잘못된 URL 등, 주로 400번대 HTTP 상태 코드와 관련된 오류<br/>
> 서버 오류: 서버 내부 문제로 인해 발생하는 오류, 주로 500번대 HTTP 상태 코드와 관련된 오류<br/>
> 데이터 파싱 오류: 응답 데이터를 JSON 또는 Codable 타입으로 파싱할 때 형식이 일치하지 않는 경우 발생하는 오류


<br/> 
<br/> 

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source