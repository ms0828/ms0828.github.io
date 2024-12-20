---
title: "Application Layer - Video Streaming and CDNs"
excerpt: ""

categories:
  - Computer Network
tags:
  - [tag1, tag2]

permalink: /categories/computer_network/video_streaming/

toc: true
toc_sticky: true

date: 2024-10-16
last_modified_at: 2024-10-16
---

# 🦥 Video streaming and Content distribution networks
비디오를 스트리밍하는 것은 인터넷 대역폭에서 가장 큰 트래픽을 차지한다. <br> 
이번 챕터는 비디오 스트리밍 서비스를 어떻게 많은 사용자에게 효율적으로 제공할지를 다루고 있다.<br>
아래는 효율적인 비디오 스트리밍 서비스 제공을 위한 도전과제이다.<br>

1. 사용자 마다 다른 네트워크 환경
  - 사용자마다 다른 대역폭과 네트워크 환경을 갖고 있는데, 어떻게 동일한 서비스를 제공할 것인가?
2. 규모
  - 하나의 큰 서버로 유지할 것인가? -> X
    - 네트워크 병목 현상 발생
    - 여러 지역에 분산된 사용자에게 동일한 서비스 제공 어려움


<br><br>

## Video Streaming
사용자가 서버에 저장된 비디오를 스트리밍할 때, 발생할 수 있는 문제점은 다음과 같다.<br>

1. 대역폭 변동
  - 네트워크 혼잡 수준이 시간에 따라 변하기 때문에 서버에서 클라이언트로 가는 대역폭이 일정하지 않음
2. 패킷 손실과 지연
  - 네트워크 혼잡으로 인해 패킷 손실과 지연이 발생 가능
  - 재생이 지연되거나 화질 저하 발생 가능

<br>
따라서 원할한 비디오 스트리밍 서비스를 제공하기 위해 다음과 같은 여러 방법이 사용되고 있다.

<br><br>

### Video Data의 bit를 줄이는 방법
가장 원초적인 접근으로 전송할 비디오의 비트 수를 줄이는 방법이 있으며, 핵심 아이디어는 연속된 이미지의 중복성을 이용하여 전송할 비트 수를 최소화 하는 것이다. 

1. Spatial Coding
  - 한 이미지 내부에서 발생하는 색상 중복에 대하여 각각의 값을 보내는 대신 해당 색상 값과 반복되는 픽셀 수 만 전송하는 방식이다.
2. Temmporal Coding
  - 연속되는 이미지들 사이에서 발생하는 중복에 대하여, 현재 프레임 이미지와 다음 프레임의 이미지의 차이만 전송하는 방식이다.

<br><br>

### Video Data를 Encoding 하는 방법
또는 비디오를 효율적으로 압축하는 방법이 있으며, Video를 Encoding 한다는 것은 비디오 데이터를 디지털 형식으로 변환하고 압축하는 과정을 말한다. 이를 통해 비디오 파일의 크기를 줄이거나, 특정 장치나 네트워크 환경에 맞춰 비디오를 최적화 할 수 있다.<br>
비디오를 Encoding 하는 방법은 다음과 같다.

1. CBR (Constant Bit Rate)
  - 고정 비트율을 의미하며, 비디오 인코딩 속도가 일정하게 유지
  - 즉, 영상의 복잡도나 변화 정도와 상관없이 항상 동일한 속도로 데이터가 전송
  - 따라서 사용자는 비디오를 스트리밍할 때 일정한 네트워크 대역폭을 사용
2. VBR (Variable Bit Rate)
  - 가변 비트율을 의미하며, 비디오 인코딩 속도가 영상의 복잡도에 따라 변동
  - 영상에서 많은 변화가 있거나 복잡한 장면에서는 더 높은 비트율을 사용하고, 덜 복잡한 장면에서는 비트율 사용
  - 더 효율적인 네트워크 자원 사용 가능 (상황에 따른 대역폭 사용 가능)

<br><br>


### Client Buffer와 Playout delay

![video1](/assets\images\posts_img\network\video1.png)

클라이언트에서 비디오 재생이 시작되면, 재생 속도는 원래 비디오 녹화 시의 속도와 일치해야 한다. 하지만 네트워크 지연이 변동하기 때문에, 이러한 지연을 보정하기 위해서는 **클라이언트 측 버퍼(client-side buffer)**가 필요하다. 이 버퍼는 재생 중단 없이 비디오를 재생할 수 있도록 돕는다.<br>

또한 원할한 재생에 필요한 데이터가 버퍼에 담길 때 까지 재생 지연(Playout delay)을 통해 네트워크 지연에 대응한다.<br>

<br><br>


### DASH (Dynamic, Adaptive Streaming over HTTP) (동적 적응형 스트리밍)
비디오를 여러 청크(chunks)로 나누어, 네트워크 상황에 맞게 낮은 비트율과 높은 비트율로 인코딩된 버전들을 제공하는 것을 말한다. 따라서 DASH는 네트워크 상태에 따라 적응적으로 비트율을 조절하여 최적의 비디오 스트리밍 품질을 제공하는 프로토콜이다.

DASH를 사용한 스트리밍을 서버와 클라이언트 측 역할을 분리하면 다음과 같다.

1. 서버
  - 비디오 파일을 여러 개의 청크로 나누어 저장한다. (각 청크는 다른 비트율로 인코딩 됨)
  - 매니페스트 파일 생성
    - 이 파일은 각각의 청크에 대한 URL을 제공하여 클라이언트가 해당 청크를 요청할 수 있도록 도움
2. 클라이언트
  - 클라이언트는 주기적으로 서버-클라이언트 간 대역폭을 측정
  - 현재 사용 가능한 대역폭을 기반으로 적절한 비트율의 청크를 선택
  - 매니페스트 파일을 참고하여 하나의 청크씩 요청

<br><br>


## CDN (Content distribution networks)
처음 개요에서 제시한 도전과제 중, 스트리밍 네트워크의 규모에 대해서 가장 원초적인 접근이 서버를 대형화 시키는 것이었다. 그러나 한 개의 서버를 대형화하여 유지하는 것은 다음과 같은 문제점이 있다.

1. 단일 실패 지점
  - 이 서버가 문제가 생기면 전체 서비스에 영향을 미침
2. 네트워크 혼잡
  - 하나의 서버로부터 수많은 사용자에게 동시에 데이터를 전송하려면 네트워크 혼잡이 발생
3. 클라이언트까지의 긴 경로
   - 서버와 클라이언트 사이의 물리적 거리가 멀면 데이터 전송 시간이 오래 걸리고 성능이 저하
4. 여러 개의 복사본 전송
  - 같은 비디오가 여러 사용자가 요청할 때마다 네트워크를 통해 반복적으로 전송됨.(네트워크 자원의 낭비)
 
<br>

위를 해결하기 위해 여러 서버에 분산된 **콘텐츠 배포 네트워크(CDN)**을 배치한다. 따라서 사용자 위치에 상관없이 안정적인 스트리밍 서비스를 제공하고 네트워크 부담을 분산시킨다.<br>

> CDN의 핵심 아이디어는 지리적으로 분산된 여러 사이트에 비디오 복사본을 저장하여 상황에 맞게 클라이언트에게 제공하는 것이다. 따라서 서버의 단일 실패 지점이나 네트워크 혼잡 문제를 해결할 수 있고, 확장성이 뛰어나다.

<br><br>

### CDN의 작동 방식
1. 서버가 CDN 노드에 콘텐츠의 복사본 저장
2. 클라이언트 요청
  - 요청은 가장 가까운 CDN 노드로 라우팅되며, 그곳에서 클라이언트는 콘텐츠 복사본을 받아옴
  - 네트워크 경로가 혼잡할 경우 다른 노드의 복사본을 선택할 수 있으며, 이는 여러 경로를 통해 트래픽 분산이 가능함을 의미

<br>

#### 매니페스트 파일
위를 가능하게 하는 것이 매니페스트 파일이다. 매니페스트 파일은 콘텐츠(비디오)를 요청했을 때 클라이언트에게 반환되며, 이는 콘텐츠의 여러 복사본에 대한 정보를 제공하고, 어느 CDN 노드에 어떤 콘텐츠가 있는지 알려준다. 이를 통해서 클라이언트는 가장 가까운 위치(또는 가장 원할한)의 CDN을 선택하여 콘텐츠를 제공받을 수 있다. <br>




<!--chapter3 24p 까지-->