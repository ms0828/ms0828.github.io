---
title: "(Introduction) Network Layers"
excerpt: ""

categories:
  - Computer Network
tags:
  - [tag1, tag2]

permalink: /categories/computer_network/network_layers/

toc: true
toc_sticky: true

date: 2024-09-17
last_modified_at: 2024-09-17
---

## 🦥 Network Layers
네트워크 상에서 데이터를 전송하는 복잡한 과정을 관리하기 위해서, 네트워크는 Protocol을 기준으로 계층화된 구조를 따른다. 이렇게 계층 구조를 가진 네트워크는 해당 계층의 서비스의 변경이 자유롭고 네트워크 구성 요소들의 의존성을 줄일 수 있다.<br>
> 따라서 현재 네트워크의 구조는 프로토콜을 기준으로 한 계층 구조를 적용시켜 설계되고 있다.

<br>

### Internet Protocol Stack
네트워크에서 사용되는 프로토콜은 계층별로 분류되어 있으며, 이를 표현한 것이 Protocol Stack이다. <br>
Protocol Stack은 5개의 계층으로 구성되어 있으며 각 계층의 모습은 다음과 같다. 

![protocol_layer](/assets\images\posts_img\network\protocol_layer.png)

<br>

### Internet Protocol Stack의 각 계층
1. **Application Layer**<br>
  사용자가 네트워크를 통해 정보를 주고받을 수 있게 해주는 계층으로, 웹 브라우징, 이메일, 파일 전송 등의 애플리케이션이 이 계층에서 작동한다. <br>
  주로 다음과 같은 프로토콜이 사용된다.
     - HTTP
     - FTP
     - SMTP
  <br><br>

2. **Transport Layer**<br>
   데이터가 정확하게 전송되도록 보장하는 계층으로, 데이터를 작은 패킷으로 나누고, 순서대로 재조립하고, 오류가 생겼을 때 재전송을 관리한다. <br>주로 다음과 같은 프로토콜이 사용된다.
     - TCP
     - UDP
  <br><br>

3. **Network Layer**<br>
  데이터를 목적지까지 전달하는 경로를 선택하는 계층으로, IP 주소를 사용해 데이터 패킷이 네트워크 상에서 올바른 목적지로 이동하도록 돕는다. 여기서 주로 사용되는 프로토콜이 IP이다.
  <br><br>

4. **Data Link Layer**<br>
  동일한 네트워크 내에서 인접한 장치들 간의 데이터 전송을 담당하는 계층으로, 패킷을 프레임으로 캡슐화하고 물리 계층에서의 오류 검출 및 수정 기능을 제공한다. 이 계층에서는 이더넷 같은 기술이 주로 사용된다.
  <br><br>

5. **Physical Layer**<br>
  제 물리적 연결을 통해 비트 스트림을 전송하는 계층이야. 케이블, 무선 신호, 네트워크 인터페이스 카드(NIC) 등 하드웨어적인 부분을 다루며, 0과 1로 된 비트가 전기적 신호로 변환되어 전송된다.


  <br><br>