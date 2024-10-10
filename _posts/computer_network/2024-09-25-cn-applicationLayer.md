---
title: "Application Layer"
excerpt: ""

categories:
  - Computer Network
tags:
  - [tag1, tag2]

permalink: /categories/computer_network/application_layer/

toc: true
toc_sticky: true

date: 2024-09-25
last_modified_at: 2024-09-25
---

## 🦥 Application Layer
인터넷 프로토콜 스택의 최상위 계층으로, 사용자와 네트워크 간의 직접적인 상호작용을 담당하는 계층이다.<br>
Application Layer에서 일어나는 모든 작업은 우리가 일상적으로 사용하는 소셜 네트워크나 이메일 등 다양한 인터넷 서비스와 관련있다.

<br><br>

### Application Layer 설계 목표
Application Layer를 설계할 때 핵심이 되는 부분은 다음 두 가지이다.
- end system에서의 어떻게 동작할 것인가? - Client 혹은 Sever와 어떻게 상호작용 할 것인가?
- 네트워크 상에서 다른 Application(with network)과 어떻게 통신할 것인가? - 어떤 프로토콜을 사용할 것인가?
<br>

> Application Layer의 설계에서 중요한 것은 해당 계층에서 네트워크 코어(ex. 라우팅) 등의 하위 계층의 세부사항을 신경쓰지 않고 해당 계층에서의 목표만 잘 이루는 것이다. 이것은 네트워크의 Protocol Layer 설계의 이유이기 때문이다. <br>

<br><br><br>


## Application Layer의 주요 Architecture
Application Layer에서 데이터를 주고 받는 주요 방식에는 다음과 같은 두 가지 방식이 있다.

<br><br>

### Client-Server
![client-server](/assets\images\posts_img\network\client-sever.png)

클라이언트는 서버에 요청을 보내고, 서버는 그 요청에 대한 응답을 처리해 데이터를 제공하는 구조이다. <br>
이 때 서버는 동작하고 있어야 하고 고정된 IP주소(혹은 도메인)을 갖고 있어야 한다.<br> 
주로 웹 브라우징(HTTP), 이메일 송수신(SMTP), 파일 다운로드(FTP) 같은 서비스가 이 구조를 따른다.<br>

<br>

Client-Server 구조에서는 다음과 같은 특징이 있다.
- 중앙 집중화(장점) : 서버는 중앙에서 데이터를 저장, 관리하며 모든 클라이언트는 해당 서버에 연결돼 데이터를 요청하는 구조
- 관리 용이성(장점): 서버에서 모든 데이터와 서비스가 관리되므로 데이터 백업이나 보안이 상대적으로 쉽다.
- 확장성 문제(단점) : 많은 클라이언트가 한 서버에 접속할 때 서버에 부하가 걸릴 수 있기 때문에, 고성능 서버가 필요하거나 부하 분산을 위한 추가 인프라가 필요하다.
- 종속성 문제(단점) : 서버가 다운되면 클라이언트는 서비스를 이용할 수 없게 된다.

<br><br>


### Peer-to-Peer
![p2p](/assets\images\posts_img\network\p2p.png)

각각의 peer(host)가 클라이언트와 서버의 역할을 동시에 수행하는 구조로, 모든 peer가 데이터 제공자이자 수신자로 작동한다.주로 파일 공유 프로그램(토렌트), 블록체인에서 이 방식을 많이 사용한다. <br>
Peer-to-Peer에서 가장 중요한 점은 Client-Server와 다르게 서버에 대해 자가 확장성(self scalability)을 가진다는 것이다.
따라서 고성능 서버의 요구나 서버 증설 등의 비용 문제에서 효율적이다.  
<br>

Peer-to-Peer 구조에서는 다음과 같은 특징이 있다.
- 분산화(장점) : 중앙 서버가 없고 모든 참여자(peer)가 네트워크의 일부로서 동등한 역할을 함으로써, 네트워크가 멈추는 일이 없다.
- 확장성(장점) : 피어 수가 증가할수록 네트워크 용량이 자연스럽게 증가하여 많은 사용자가 있을수록 효율적이다.
- 복잡성(단점) : 분산 구조이기 때문에 데이터의 일관성을 유지하는 데 어려움이 있고 연결이 불안정할 수 있다.

<br><br>



## Processes Communicating
네트워크에서 서로 다른 장치나 동일 장치 내에서 실행되는 프로세스들이 데이터를 주고받으며 통신하는 것을 의미한다.
> 프로세스 간의 통신은 Application Layer의 프로토콜을 통해 이루어진다.

<br>

네트워크 측면에서 봤을 때, Application은 다음 두 가지 프로세스로 구성된다.
1. Client Process
  - 다른 프로세스와 연결을 시도하는 Process
2. Server Process
  - 다른 프로세스와의 연결을 기다리는 Process

<br><br>




## Socket
![soket](/assets\images\posts_img\network\soket.png)

컴퓨터 네트워크에서 Process가 데이터를 주고받기 위해서는 Process(Application Layer)와 네트워크(Transport Layer) 사이의 인터페이스가 필요하다.<br>
이 인터페이스는 Process가 네트워크를 사용할 수 있도록 해주는 역할을 히는데, 이 인터페이스를 제공하는 대표적인 요소 중 하나가 소켓(Soket)이다.
<br>

즉, 소켓은 네트워크 연결을 열고 데이터를 송수신 가능하게 하는 인터페이스 구현체로, 이를 통해 프로세스가 네트워크를 통해 다른 컴퓨터의 프로세스와 통신할 수 있게 한다.
<br>

**소켓의 주요 역할**
- 통신 채널 연결 : 소켓은 네트워크 통신을 위한 양쪽 끝을 정의합니다. (한 프로세스는 데이터의 송신 측이고, 다른 프로세스는 수신 측)
- 프로토콜 지원 : 소켓은 TCP나 UDP와 같은 Transport Layer의 프로토콜을 통해 데이터의 전송 방식을 결정한다.
<br>


**정리**
> 소켓은 Application Layer의 프로세스가 Transport Layer을 통해 데이터를 주고 받을 수 있도록 연결을 설정하고 데이터를 송수신 할 수 있는 인터페이스를 제공한다. 소켓을 통해 데이터는 Transport Layer에서 TCP/UDP 프로토콜을 사용해 다른 네트워크 상의 Application Process로 전달하게 된다. <br>
> - 소켓은 저수준 네트워크 통신을 감추고, 개발자가 직접 네트워크 패킷을 관리하거나 복잡한 프로토콜 처리를 하지 않도록 해준다. 즉, 개발자는 소켓이라는 인터페이스를 사용해 네트워크 통신을 쉽게 구현할 수 있게 된다.




<br><br>


## Addressing processes
네트워크에서 서로 다른 프로세스들이 통신을 하기 위해서는 프로세스가 고유하게 식별되어야 하며, 이를 위한 식별자가 필요하다. <br>
따라서 host 장치들은 유일한 32-bit의 IP 주소를 갖고 있다.
<br>

하지만 하나의 host에서 여러 Process가 실행중 일 수도 있기 때문에, 각 프로세스는 고유 IP에 더불어 **port number** 또한 갖고 있어야 한다.
<br>

- IP 주소는 호스트를 식별하고, 포트 번호는 호스트에서 실행되는 특정 프로세스를 식별
- 각 프로세스는 고유한 **포트 번호(port number)**를 가지고 있고, 이는 같은 호스트에서 실행 중인 여러 프로세스를 구분하는 데 사용됨


<br><br>

## Application Layer Protocol
Application Layer Protocol은 서로 통신하는 Application Process 간의 데이터 교환과 규칙을 정의한다.

다음과 같은 요소가 Application Layer Protocol에서 정의된다.
- 메시지 유형 : 요청(request), 응답(response)과 같은 메세지의 종류
- 메시지 구문 : 메세지의 필드가 어떤 구성을 가지며 필드가 어떻게 구분되는지를 정의<br>
    예를 들어, HTTP 요청 메시지에는 헤더와 본문으로 나뉘는데, 이를 어떤 형식으로 구분하는지 정의
- 메시지 송수신 응답 규칙 : 프로세스가 메시지를 언제 전송해야 하고, 어떤 조건에서 응답을 보내야 하는지에 대한 규칙을 정의<br>
    예를 들어 HTTP에서는 Client가 요청을 보내고 Sever가 이를 처리한 후 적절한 응답의 신호로 (200 OK)를 보내야 함

<br>

Application Layer Protocol는 두 가지 유형이 있다.
- Open protocols(공개 프로토콜) : **RFC(Request for Comments)**에서 정의된 프로토콜로, 누구나 접근하고 사용할 수 있는 프로토콜 (예: HTTP, SMTP 등은 전 세계적으로 사용 가능한 공개 표준 프로토콜)
- Proprietary protocol(전용 프로토콜) : 특정 회사나 조직이 소유하고 있으며, 외부에 공개되지 않는 프로토콜 (프로토콜은 일반적으로 해당 서비스에서만 동작) (예: Skype)

<br><br>




## Transport Layer의 Service
Application Layer는 Process들이 네트워크를 통해 데이터를 주고받을 때 Transport Layer가 제공하는 서비스를 필요로 한다.
<br> 
따라서 Application Layer는 다음과 같은 Transport Layer가 제공하는 서비스의 특징을 요구 한다.
1. 데이터 무결성(Data Integrity) : 정확하고 신뢰할 수 있는 데이터를 보장 받을 수 있는지를 의미
    > 파일 전송과 같은 application은 100% 정확한 데이터 전송이 필요하나 오디오 스트리밍 같은 application은 일부 데이터 손실을 허용
2. Timing : Application이 얼마나 짧은 지연시간을 필요로 하는지를 의미
3. 처리량(Throughput) : Application이 얼마나 많은 데이터를 얼마나 빠르게 전송할 수 있어야 하는지를 의미
4. 보안(Security): Application이 데이터를 얼마나 안전하게 전송해야 하는지를 의미


<br><br>


## Transport Layer Protocol
Transport Layer에서 제공하는 Protocol인 TCP와 UDP는 각각 다른 목적과 장단점을 가지고 있으며, 네트워크 상에서 다양한 애플리케이션 요구를 충족시킨다.<br>
<br>

### TCP의 특징
  - **신뢰성 있는 전송 (Reliable transport)**
    > TCP는 데이터가 정확하고 순서대로 전송되도록 보장한다. 손실된 데이터 패킷은 자동으로 재전송되며, 수신 측에서 패킷을 재정렬해 보낸다.
  - **흐름 제어 (Flow control)**
    >  송신 측이 데이터를 너무 빠르게 보내 수신 측이 과부하되지 않도록 조정한다. 송신 측은 수신 측의 상태를 확인하고, 수신 측이 처리할 수 있는 양만큼 데이터를 전송한다.
  - **혼잡 제어 (Congestion control)**
    > 네트워크가 혼잡할 때, TCP는 송신 속도를 조절하여 네트워크 과부하를 방지한다. 
  - **연결 지향 (Connection-oriented)**
    > TCP는 클라이언트와 서버 간에 연결을 설정한 후 데이터를 주고받는다. 이는 데이터를 안전하게 전송하기 위한 3-way handshake 과정을 포함한다.

**TCP가 제공하지 않는 것**
  > TCP는 타이밍, 최소 처리량 보장, 보안은 제공하지 않는다. 이는 주로 실시간 통신이나 보안이 필요한 애플리케이션에서는 별도의 메커니즘이 필요함을 의미한다.

<br><br>


### UDP의 특징
  - **신뢰성 없는 전송 (Unreliable data transfer)**
    > UDP는 데이터를 보내는 즉시 전송되며 수신이 보장되지 않는다. 즉, 패킷 손실이나 순서가 뒤바뀌는 상황이 발생해도 이를 자동으로 복구하지 않는다.
  - **그 밖에도 UDP는 TCP가 제공하는 다양한 기능을 제공하지 않음**
    > UDP는 흐름 제어, 혼잡 제어, 타이밍 보장, 최소 처리량 보장, 보안, 연결 설정을 제공하지 않는다.

<br>

* 그럼 UDP는 왜 사용하는데?
UDP는 매우 간단하고 빠르게 데이터를 전송하는 대신, 데이터 전송의 신뢰성을 희생하는 Protocol이다. <br>
연결 설정과 확인 과정이 없기 때문에 실시간 성능이 중요한 애플리케이션(예: 온라인 게임, 스트리밍, VoIP)에서 자주 사용된다.

> 데이터가 약간 손실되거나 순서가 중요하지 않은 경우, UDP는 매우 유용하다. 예를 들어, 실시간 음성 통화에서는 빠른 전송이 중요하며, 몇 개의 데이터 손실이 발생해도 큰 영향을 미치지 않기 때문에 Resource를 많이 잡아먹는 TCP보다 UDP가 유리하다.


* 따라서 애플리케이션의 요구 사항에 따라 적합한 Protocol이 선택된다.

<br><br>




### 수업 시간에 언급한 내용 몇 가지

1. integrity vs throughput이 중요할지 나머지 Layer를 선택할 수 있다.
2. protocol은 무엇을 정의하느냐?
- Type (request, response)
- syntax(문법) (http request message general format 참고, 어떤 필드가 어떤 순서로 정의되는지)
- semantic : 각 필드가 어떤 정보를 포함하는지 정의  
- rules : 각 메세지에 어떻게 처리할건지 정의
3. 프로토콜을 만들 때 무엇이 중요한지에 따라 다르게 구현 (1번과 동일한 맥락)
4. TCP
- Reliable(no error, no loss, in order)
- flow control -> 받을 사람이 받을 만큼만 보냄
- congestion control -> 네트워크가 소화할만큼만 보냄 (속도를 조절)
5. UDP
- TCP가 가지는 기능이 없지만 빠르다.
- 실시간 속도가 중요시 되는 통신에 사용 (게임 등)
