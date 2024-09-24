---
title: "Computer Network Introduction"
excerpt: "간단 용어 정리"

categories:
  - Computer Network
tags:
  - [tag1, tag2]

permalink: /categories/computer_network/introduction/

toc: true
toc_sticky: true

date: 2024-09-09
last_modified_at: 2024-09-09
---

## 🦥 Computer Network Introduction

### What is Network?
네트워크는 두 대 이상의 컴퓨터가 서로 연결되어 데이터를 주고받을 수 있는 환경을 말하며 규모와 종류에 따라 여러가지로 나뉜다.
- 로컬 네트워크 (LAN) : 동일한 물리적 위치(예: 회사 또는 가정) 내의 컴퓨터들 간의 네트워크
- 광역 네트워크 (WAN) : 광역 네트워크(WAN)는 지리적으로 떨어진 여러 장소를 연결하는 네트워크

#### 네트워크의 기본 요소
- Node : 네트워크에 연결된 장치나 컴퓨터 (혹은 End system or Host or network edge 라고 불림)
- Switch : 동일한 네트워크 내에서 데이터를 교환하는 장치로, 네트워크 안의 장치들 간의 통신을 관리
- Router : 네트워크 간 데이터를 전달하고 올바른 경로를 선택하는 장치
- Protocol : 네트워크 상에서 데이터를 주고받는 규칙과 형식, 예를 들어 TCP/IP, HTTP가 있음


<br>


### What is the Internet?
- 인터넷은 전 세계적으로 연결된 컴퓨터 네트워크들의 거대한 집합를 말한다.<br>
- 수많은 개별 네트워크와 장치들이 서로 통신하고 정보를 주고받을 수 있도록 하는 글로벌 네트워크이다.<br>
- 인터넷은 TCP/IP 프로토콜을 사용하여 데이터를 전송하며, 웹 브라우징, 이메일, 파일 전송 등의 다양한 서비스들을 지원한다.

![internet](/assets\images\posts_img\network\internet_introduction.png)


<br><br>


### What is a protocol?
- 컴퓨터 네트워크에서 데이터를 주고받기 위한 일련의 규칙과 절차를 말한다. <br>
- 각기 다른 시스템, 장치 간에 통신이 원활하게 이루어지도록 하기 위해 표준화된 규칙을 제공한다.
- 데이터가 어떻게 형식화되고, 전송되고, 수신되는지를 정의함으로써 다양한 네트워크 장치들이 서로간의 차이에도 불구하고 일관되게 통신할 수 있도록 한다.

#### 프로토콜의 예
1. TCP/IP (Transmission Control Protocol/Internet Protocol): 인터넷의 기본 프로토콜로, 데이터를 안정적으로 전송하고 네트워크 간의 라우팅을 관리
  - TCP는 연결 기반 프로토콜로, 데이터가 손실 없이 순서대로 도착하도록 보장
  - IP는 데이터를 목적지 주소로 라우팅하는 역할을 함

2. HTTP (HyperText Transfer Protocol): 웹 브라우저와 웹 서버 간에 데이터를 주고받기 위한 프로토콜
3. FTP (File Transfer Protocol): 네트워크를 통해 파일을 전송하기 위한 프로토콜로, 서버와 클라이언트 간 파일 업로드 및 다운로드를 지원
4. DNS (Domain Name System): 도메인 이름을 IP 주소로 변환하여, 사용자가 기억하기 쉬운 도메인 이름으로 웹사이트에 접속할 수 있도록 돕는 프로토콜


<br><br>


### Network Edge
Network Edge는 네트워크의 경계 부분으로, 주로 Host로 정의되는 사용자 장치(클라이언트)와 서버를 말한다.

![edge](/assets\images\posts_img\network\network_edge.png)

Hosts : 네트워크에 연결된 장치들을 의미하며 데이터를 주고받는 주체를 말함
  - Client : 서비스를 요청하는 장치
  - Sever : Client의 요청을 처리하고 응답을 보내는 장치 (ex : 웹 서버, 이메일 서버 등)


<br>

