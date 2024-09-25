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

- application이 뭔데?
- application layer 작동 방법 (예제와 함께)


network application의 예제
- 인스타
- 웹
- 이메일

네트워크를 사용하는 모든 앱을 말함

레이어로 구분했을 때의 장점?


### 대표적인 아키텍처 2개

1. Client-server paradigm
Application Layer에서는 서버와 클라이언트로 이루어짐

Sever
- 서버는 항상 켜져있음 (정보를 요청하는 주체가 클라이언트)
- 고정 IP 주소를 서버가 가지고 있어야 함 (클라이언트가 어디로 요청해야할지)
- 주로 data center에서 관리됨

Client
- 항상 연결되어있을 필요가 없음 (정보를 요청할 때만)
- 고정된 IP를 갖고 있을 필요가 없음


2. Peer-peer architecture
중앙 서버 없이 클라이언트 끼리 연결되어 통신 (클라이언트가 서버가 될 수 있음)
- self scalability - Peer의 수에 따라 서버의 크기가 유동적으로 변동
- 중앙 관리가 아니기 때문에 복잡하고 연결이 불안정할 수 있음


### Processes Communicating
* Process : 어플리케이션을 가동하는 것을 말함

1. client process
  - communication을 요청(initiate)
2. server process
  - communication 연결을 기다림


### Sockets
process는 socket으로 데이터를 주고 받음
Application layer와 transport layer 사이에 존재하는 Gate


### Addressing processes
송수신되는 데이터는 identifier를 가지고 있어야함
- Unique한 32-bits IP Address
- port numbers : precess 마다 지정된 포트가 있고, 포트를 들고 있어야 요청한 프로세스에 데이터가 수신 가능
> 152.235.25.53 : 80    (HTTP server port number : 80)

### Protocol defines
일련의 데이터 송수신 규칙
- 어떤 종류의 메세지인지
- 어떤 형태인지
- 어떤 규칙에 따라 읽고 쓸건지

* HTTP와 같이 전세계 기준의 표준 프로토콜


### What transport service does an app need?
- data integrity : 송수신된 데이터가 무결성인지 
- timing : 요구한 시점까지 데이터가 송수신되는지
- throughput : 원하는 속도로 데이터가 송수신되는지
- security : 보안



### Internet Transport Protocol Service
1. TCP
  - reliable transport : 데이터 송수신의 신뢰성
  - flow control : 서버의 상황에 따라 데이터 송수신 컨트롤
  - ...
2. UDP
  - unreliable transport : 데이터 송수신의 완전한 보장을 하지 않음

UDP는 TCP가 제공하는 다양한 기능을 제공하지 않음
* 그럼 UDP는 왜 사용하는데?
  > TCP는 reliable transport로 인해 connection이 계속 열려있음, 문제가 생겨도 계속 자원이 사용되기 때문에 UDP가 필요할 때가 있음

TCP, UDP 모두 security에 어떠한 기능도 갖고 있지 않음


### Securing TCP
데이터가 소켓을 통과하기 전에 보안처리를 해야함




...
26p 까지