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


<br><br>

## HTTP Message
### HTTP Request Message
![request](/assets\images\posts_img\network\request.png)
HTTP Request Message는 human-readable format인 ASCII로 작성되며, 위와 같은 형식을 따른다.

<br>

#### Request Message의 주요 메서드 4가지
이 메서드들은 각각 다른 목적에 맞게 데이터를 주고받는 데 사용된다.
<br>

1. Post method
- 웹 페이지에서 주로 폼 입력을 처리할 때 사용
- 입력된 사용자의 데이터를 서버로 전송하며, 이 데이터는 HTTP POST 요청 메시지의 Entity body에 포함
2. Get method
- 주로 서버로부터 웹 객체를 요청할 때 사용
- 사용자 데이터를 HTTP GET 요청 메시지의 URL 필드에 포함하며, 일반적으로 URL 뒤에 '?' 문자를 붙여 쿼리 매개변수로 전달
3. Head method
- HTTP GET 메서드로 요청한 URL에 대해 서버가 반환할 헤더만 요청할 때 사용
- 실제 콘텐츠는 요청하지 않고, 헤더 정보만 반환
4. Put method
- 서버에 새 파일(객체)을 업로드할 때 사용
- 지정된 URL에 있는 파일을 완전히 대체하여 POST 요청 메시지의 본문에 포함된 내용으로 교체

<br><br>

### HTTP Response Message
![response](/assets\images\posts_img\network\response.png)
HTTP Response Message는 human-readable format인 ASCII로 작성되며, 위와 같은 형식을 따른다.

<br><br>

#### HTTP response status codes
각 상태 코드는 서버가 클라이언트의 요청에 대해 응답할 때 첫 번째 줄에 나타나는 코드로써, 서버가 요청을 처리한 결과를 클라이언트에게 알리는 중요한 역할을 한다.

1. 200 OK
- 요청이 성공했으며, 요청한 객체는 이후 메시지에 포함
2. 301 Moved Permanently
- 요청한 객체가 이동되었으며, 새로운 위치가 메시지의 Location 필드에 지정
3. 400 Bad Request
- 서버가 요청 메시지를 이해하지 못했음을 나타냄. 클라이언트가 잘못된 형식으로 요청을 보낸 경우 발생
4. 404 Not Found
- 요청한 문서를 서버에서 찾을 수 없음을 나타냄. 잘못된 URL로 요청이 발생했을 때 주로 반환
5. 505 HTTP Version Not Supported
- 서버가 클라이언트에서 요청한 HTTP 프로토콜 버전을 지원하지 않는 경우 발생하는 오류

<br><br><br>


## Maintaining user/service State : cookies
웹 사이트와 클라이언트(브라우저)는 트랜잭션 간의 상태를 유지하기 위해서 **쿠키(Cookies)** 를 사용
<br>

### Cookies를 활용한 통신의 주안점
1. **첫 번째 HTTP 응답 메시지의 쿠키 헤더 라인**
- 서버가 응답을 보낼 때, 쿠키 정보(ID)를 포함한 헤더를 보냄
2. **다음 HTTP 요청 메시지의 쿠키 헤더 라인**
- 클라이언트(브라우저)가 서버로 요청을 보낼 때, 이전에 받은 쿠키 정보를 함께 보냄
3. **사용자의 호스트(컴퓨터)에 저장된 쿠키 파일**
- 클라이언트의 브라우저가 쿠키를 관리하고 사용자 컴퓨터에 저장
4. **웹 사이트의 백엔드 데이터베이스**
- 서버 측에서는 쿠키에 저장된 ID 정보를 데이터베이스에 저장하여 사용자의 세션 상태를 추적

<br>

### Cookies의 동작
![cookies](/assets\images\posts_img\network\cookies.png)

> 1. 첫 번째 HTTP 요청이 사이트에 도착하면, 사이트는 고유한 ID(쿠키)를 생성하고 그 ID를 데이터베이스에 저장
> 2. 이후 같은 사이트로 보내는 모든 HTTP 요청에는 이 쿠키 ID가 포함되어 사이트는 해당 사용자를 식별

<br>

### Cookies의 용도
1. authorization - 사용자가 인증된 상태를 유지
2. shopping carts - 온라인 쇼핑 시, 장바구니 정보 유지
3. recommendations - 사용자 맞춤형 추천 시스템
4. user session state - 사용자 세션 유지


<br><br><br>


## Web Caches
### Proxy Server
Web Cache는 사용자가 웹 사이트를 방문할 때 웹 페이지, 이미지 등 자주 사용되는 콘텐츠를 미리 저장하여, 다음에 동일한 요청이 발생할 때 더 빠르게 응답할 수 있도록 돕는 기술이다.
> 클라이언트와 서버 사이에 위치하여 네트워크 트래픽을 줄이고 웹 페이지 로딩 속도를 개선하는 역할을 한다.

<br>

1. 클라이언트이면서 서버로 작동
- 요청한 클라이언트에 대해 서버 역할
- 원본 서버에 대해서는 클라이언트 역할
2. 보통 **인터넷 서비스 제공자(ISP)**에 의해 설치됨. (대학, 회사, 가정용 ISP 등이 이에 해당)


<br>

### Web Caches를 사용하는 이유
1. 클라이언트 요청에 대한 응답 시간을 단축
- 캐시는 클라이언트에 더 가까이 위치해 있어 요청에 대한 응답 시간이 줄어듬
2. 기관의 네트워크 링크에서 발생하는 트래픽을 줄임
3. 더 효과적으로 콘텐츠를 전달할 수 있게 도와줌
- 리소스가 부족한 제공자도 캐시를 활용해 성능을 향상

<br><br>


### Cache에 대한 Conditional GET
이미 캐시하고 있는 데이터에 대해, 클라이언트가 서버에게 데이터가 최신 상태인지 확인하는 요청 메세지를 말한다.
<br>

![conditional_get](/assets\images\posts_img\network\conditional_get.png)

> 요청에는 **If-modified-since: (date)** 라인이 포함되며, date로 부터 최신화된 항목이 없으면 서버는 **HTTP/1.0 304 Not Modified** 를 반환하고, 최신화된 항목이 있으면 서버는 **HTTP/1.0 200 OK** 와 함께 최신화된 데이터를 반환한다.


