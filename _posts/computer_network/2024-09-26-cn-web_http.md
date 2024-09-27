---
title: "Web and Http"
excerpt: ""

categories:
  - Computer Network
tags:
  - [tag1, tag2]

permalink: /categories/computer_network/web_and_http/

toc: true
toc_sticky: true

date: 2024-09-26
last_modified_at: 2024-09-26
---

## 🦥 Web and HTTP
### Web
웹은 서버에 웹 객체(HTML file, JPEG image, Java applet, audio file...)를 저장하여, 요청이 들어오면 서버에서 URL에 해당하는 객체를 사용자에게 보내주는 Application(Network)이다.
<br>

### HTTP (Hypertext Transfer Protocol)
![web](/assets\images\posts_img\network\web_overview.png)
HTTP는 웹의 Application Layer Protocol로, 클라이언트-서버 모델을 기반으로 동작한다.<br>

- 클라이언트(client)<br>
    클라이언트는 웹 브라우저로서, HTTP 프로토콜을 사용하여 웹 서버에 요청을 보낸다.

- 서버(server) <br>
    웹 서버는 클라이언트로부터 HTTP 요청을 받으면, 요청된 웹 객체(HTML 파일, 이미지 파일 등)를 찾아 클라이언트에게 응답한다.

<br>

## HTTP의 특징

### TCP를 사용
- **TCP 연결 설정**<br>
    클라이언트(웹 브라우저)가 TCP 연결을 통해 서버와 통신한다. 클라이언트는 portnumber: 80을 사용하여 서버에 연결을 요청하고, 이때 소켓(socket)을 생성한다. 이후, 서버는 클라이언트의 TCP 연결 요청을 수락한다.

- **HTTP 메시지 교환**<br>
    TCP 연결이 설정된 후, HTTP 메시지가 클라이언트와 서버간에 교환된다. 이 메시지들은 웹 페이지 요청과 응답으로 이루어진다.

- **TCP 연결 종료**<br>
    HTTP 메시지 교환이 완료되면, TCP 연결이 종료된다. 이는 일회성 연결로, 한 번의 요청과 응답이 끝나면 TCP 연결도 닫히게 된다.

<br>

### Stateless 
HTTP는 상태를 유지하지 않는(stateless) 프로토콜로써, 서버는 과거의 클라이언트 요청에 대한 정보를 저장하지 않으며 매번 새로운 요청으로 간주한다.

<br>

#### 왜 상태를 유지하지 않는가?
- 상태를 유지하는 프로토콜은 복잡성을 증가 시킨다. 이전 요청의 history를 관리하고, 서버와 클라이언트 간의 일관성을 유지해야 하기 때문에, 이를 처리하는 데 추가적인 리소스가 필요하다.

- 만약 서버나 클라이언트 중 하나가 크래시(다운) 되거나 문제가 발생했을 때, 두 측의 상태 정보가 불일치할 수 있기 때문에, 이를 해결하려면 상태를 다시 맞추는 작업이 필요하므로 복잡성이 증가한다.

<br><br>


## HTTP의 두 가지 연결 유형

### Non-persistent HTTP
하나의 TCP 연결에 최대 한 개 객체가 전송되는 연결 방법으로 과정은 다음과 같다.

1. **TCP 연결 열기**<br>
    클라이언트는 서버에 요청을 보내기 위해 TCP 연결을 열고, 이 과정에서 클라이언트와 서버 간에 TCP 연결이 설정된다.
2. **객체 전송**<br>
    하나의 TCP 연결에서 최대 한 개의 객체만 전송한다. 예를 들어, 클라이언트가 HTML 파일을 요청하면 해당 TCP 연결에서 오직 HTML 파일 하나만 전송된다.
3. **TCP 연결 닫기**<br>
    하나의 객체가 전송되고 나면, TCP 연결이 닫힌다. 이후 추가로 다른 객체를 다운로드하려면 새로운 TCP 연결을 열어야 한다.

<br>

> 웹 페이지는 일반적으로 HTML 파일뿐만 아니라 이미지, CSS, JavaScript 파일 등 여러 객체로 구성된다. 이러한 다수의 객체를 다운로드하기 위해서는 매번 새로운 TCP 연결을 열어야 하며, 이는 성능 저하로 이어질 수 있다.

<br><br>

#### Non-persistent HTTP의 Response time
![response time](/assets\images\posts_img\network\response_time.png)

- RTT : 클라이언트에서 서버로 작은 패킷이 전송되고, 그 응답이 돌아오기까지 걸리는 시간
<br>

1. 클라이언트가 서버에 TCP 연결을 요청하고, 1 RTT 후에 연결이 설정
2. 그 다음, 클라이언트는 서버에 파일을 요청하고, 1 RTT 후에 서버로부터 첫 번째 응답
3. 마지막으로 파일 전송 시간이 걸리며, 그 후 파일이 클라이언트에 도착

> 따라서 한 개의 객체당 Response time은 2RTT + File trasmission time

<br><br>



### Persistent HTTP

1. **TCP 연결 열기**<br>
    클라이언트가 서버에 요청을 보내기 위해 TCP 연결을 연다.
2. **여러 객체 전송**<br>
    클라이언트는 하나의 TCP 연결을 통해 여러 객체를 서버로부터 받을 수 있다. 예를 들어, 웹 페이지의 HTML 파일과 이미지 파일을 한 번의 연결로 전송받을 수 있다.
3. **TCP 연결 닫기**<br>
    모든 객체를 전송한 후에야 TCP 연결이 닫힌다. 이는 non-persistent http보다 네트워크 리소스를 절약하고, 성능이 향상된다.


