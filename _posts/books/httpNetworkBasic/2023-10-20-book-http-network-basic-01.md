---
title: "[그림으로 배우는 Http&Network Basic] 01-05"

excerpt: "그림으로 배우는 Http&Network Basic"
categories: [Book, 그림으로 배우는 Http&Network Basic]
tags: [Book, 그림으로 배우는 Http&Network Basic]
date: 2023-10-20
last_modified_at: 2023-10-20
render_with_liquid: false
---

# 그림으로 배우는 Http & Network Basic - 우에노 센

김영한님의 인프런 강의 - [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.notion.so/HTTP-92f661217cfc4ebdb49d76a454cacbb6?pvs=4) 을 들으면서 천재 개발자 최야호씨가 추천해준 웹, 네트워크 입문서가 생각이 났고,
강의 내용을 복기하는 데도 큰 도움이 될 것 같아 읽어보기 시작했다.  + [강의 리뷰 보러가기](https://yeondori.github.io/categories/http-basic/)

책의 **목차**는 다음과 같다.

1. **웹과 네트워크의 기본에 대해 알아보자** - HTTP란? HTTP의 발전 과정, 계층 구조, URI 등
2. **간단한 프로토콜 HTTP** - HTTP 프로토콜의 구조
3. **HTTP 정보는 HTTP 메시지에 있다** - 리퀘스트와 리스폰스의 동작 방식
4. **결과를 전달하는 HTTP 상태 코드** - 상태 코드 설명
5. **HTTP와 연계하는 웹 서버** - 멀티 도메인, 중계 서버
6. **HTTP 헤더**
7. **웹을 안전하게 하는 HTTPS** - HTTPS 구조
8. **누가 액세스하고 있는지를 확인하는 인증** - 인증의 구조
9. **HTTP에 기능을 추가한 프로토콜** 
10. **웹 콘텐츠에서 사용하는 기술** 
11. **웹 공격 기술** - 웹 사이트 공격 유형과 영향

책을 읽거나 강의를 수강하기 전에 목차를 먼저 살펴 보고 흐름을 파악하려고 노력하는 편인데 목차가 꽤 많은 편이라 당황했다. 
그렇지만 정말 쉽게 잘 풀어서 설명하는 책이라 하루 만에 읽을 수 있었다.

~~대출 기한이 2일 남아서 급하게 읽기 시작한 거지만~~

위 목차의 각 챕터별로 내용을 정리해보고자 한다.

## 1. 웹과 네트워크의 기본에 대해 알아보자.

### HTTP, 프로토콜, TCP/IP
- **HTTP(HyperText Transfer Protocol** 클라이언트에서 서버까지의 일련의 흐름을 결정하는 프로토콜으로, 웹은 HTTP라는 약속을 사용한 통신으로 이루어져 있다.


- **프로토콜(Protocol)** 서로 다른 하드웨어와 운영체제 등을 가지고 서로 통신하기 위해 필요한 규칙. 케이블 규격, IP 주소 지정 방법, 웹 표시 순서 등이 있음


- **TCP/IP** 인터넷과 관련된 프로토콜들을 모은 것을 총칭하며, 네트워크는 일반적으로 TCP/IP 프로토콜에서 작동한다. HTTP는 TCP/IP 중 하나이다. 계층이라는 개념이 매우 중요한데, 변경 사항이 있을 때 사양이 변경된 해당 계층만 바꾸면 되므로 계층 간의 독립성과 자유성을 유지할 수  있기 때문

### TCP/IP 흐름과 구조

- **TCP/IP 통신의 흐름과 TCP/IP 4계층 구조**
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/fb411311-738a-4139-a120-57ed7ba450fb)

### HTTP와 IP, TCP, DNS

다음의 세 TCP/IP 프로토콜은 HTTP와 관계가 깊다.

#### IP(Internet Protocol) 

네트워크 층에 해당하며 개개의 패킷을 상대방에게 전달하는 역할을 한다. 
전달을 위해서는 각 노드에 부여된 주소이자 변경 가능한 IP주소와 각 네트워크에 할당된 고유한 주소이자 변경 불가한 MAC 주소가 중요하다. 
IP통신은 MAC 주소에 의존하며, 목적지를 찾아가고자 할 때는 ARP(Address Resolution Protocol)를 이용하여 IP 주소를 바탕으로 MAC 주소를 조사할 수 있다. 

#### TCP(Transfer Control Protocol) 
트랜스포트 층에 해당하며 신뢰성 있는 바이트 스트림 서비스(용량이 큰 데이터를 보내기 쉽게 분해하여 관리하는 것)를 제공한다. 
대용량의 데이터를 보내기 쉽게 작게 분해하여 상대방에게 보내고, 정확하게 도착했는지 확인하는 역할을 담당한다. 
통신의 신뢰성을 보증하기 위해 다양한 시스템응 갖추고 있으며 그 중 쓰리웨이 핸드셰이킹이 대표적이다.

#### DNS(Domain Name System) 
IP 주소 대신 이름을 사용하여 컴퓨터를 지정할 수 있는데, IP 주소와 도메인명 간의 확인을 제공하는 프로토콜이다.

### 리퀘스트 - 리스폰스 예시

- 클라이언트가 "http:/hackr.jp/xss" 웹페이지를 보고자 할 때는 다음의 과정을 거친다.

  ```text
  [클라이언트 측]
    1. DNS - "hackr.jp"의 주소를 알려준다.
    2. HTTP - 웹 서버에 보낼 HTTP 메시지를 작성한다.
    3. TCP - 통신하기 쉽도록 HTTP 메시지를 패킷으로 분해한다. 조각내 일련번호를 부여하고 상대방에게 패킷을 보낸다.
    4. IP - 상대가 어디에 있는지 찾아 중계해 가며 배송한다.

    [서버 측]
    5. TCP - 상대방으로부터 패킷을 수신해 도착한 패킷을 조립(일련번호를 참고)한다.
    6. HTTP - 웹 서버에 대한 리퀘스트 내용을 처리한 후 TCP/IP의 통신 순서대로 클라이언트에 반환한다.
  ```
### URI(Unifor Resource Identifiers)와 URL(Uniform Resource Locator) 
리소스를 식별하기 위해 문자열 전반을 나타내는 URI에 비해 URL은 리소스의 네트워크 상의 위치를 나타내는 URI의 서브셋이다. 


- **URI 포맷**
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/8ef2847c-a07b-436e-8c6a-b696c459cac4)

## 2. 간단한 프로토콜 HTTP

HTTP는 리소스를 요구하는 **클라이언트와** 리소스를 제공하는 **서버 간에 통신**을 하며, 클라이언트의 **리퀘스트**에 대한 응답으로 서버의 **리스폰스**가 송신된다. 
리퀘스트 없이는 리스폰스가 송신될 수 없다.

### 리퀘스트/리스폰스 메시지 구조

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/f668a23f-d526-4938-bbe6-dcd1a15cdbb1)

### Stateless와 쿠키

HTTP는 많은 데이터를 빠르고 확실하게 처리하는 **범위성(scalavility)** 를 확보하기 위해 상태를 유지하지 않는 **Stateless** 프로토콜이다. 
HTTP 프로토콜 레벨에서는 이전에 보냈던 리퀘스트나 되돌려준 리스폰스에 대해 기억하지 않는다. 

상태를 유지하지 않으므로 서버의 CPU나 메모리같은 리소스의 소비를 억제할 수 있으며 프로토콜을 단순하게 해준다.
  
그러나 로그인 정보를 유지해야 하는 경우처럼, 상태를 유지할 필요가 있는 경우가 있다. 
  
  -> 스테이트리스 특징은 남겨둔 채 문제를 해결하기 위해 **쿠키(Cookie)** 등장

**쿠키** 는 리퀘스트와 리스폰스에 정보를 추가하여 클라이언트의 상태를 파악하게 해주는 시스템이다. 
서버에서 리스폰스로 보내진 Set-Cookie라는 헤더 필드에 의해 쿠키는 클라이언트에 보존되고, 다음 번에 클라이언트가 같은 서버로 리퀘스트를 보낼 때는 자동으로 이 쿠키 값을 넣어 송신한다. 
서버는 클라이언트의 쿠키를 확인로 서버 상의 기록을 확인한 뒤 이전 상태를 파악한다.

### 리퀘스트 URI로 리소스 식별
HTTP는 URI를 사용하여 인터넷 상의 리소스를 지정한다. 리퀘스트 URI를 지정하는 방법으로는 다음이 있다.


  - 모든 URI를 리퀘스트 URI에 포함한다.
  - Host 헤더 필드에 네트워크 로케이션을 포함한다.
  - 리퀘스트 URI에 [*]을 지정한다. - 서버 자신에게 리퀘스트를 송신하는 경우

### HTTP 메소드
 
| 메소드     | 설명                                                                                                                                              | 리퀘스트 예시                                                                                   | 리스폰스 예시                                                                                                                                                               | 제공하고 있는 HTTP 버전 |
|:--------|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------|
| GET     | 리소스 획득. <br/>리퀘스트 URI로 식별된 리소스를 가져오도록 요구                                                                                                        | GET /index.html HTTP /1.1<br/>Host: www.hackr.jp <br/>와 같은 형식                             | index.html 리소스를 되돌려 준다.<br/>텍스트이면 그대로 반환, 프로그램이면 실행해서 출력된 내용을 돌려준다.                                                                                                   | 1.0 1.1        |
| POST    | 엔티티 전송. GET도 가능하나 일반적으로는 POST를 사용한다.                                                                                                            | POST /submit.cgi HTTP /1.1<br/>Host: www.hackr.jp<br/>Content-Length: 1500(1500 바이트 데이터)  | submit.cgi가 수신한 데이터의 처리 결과를 되돌려준다.                                                                                                                                    | 1.0 1.1        |
| PUT     | 파일 전송. 리퀘스트 중에 포함된 엔티리를 리퀘스트 URI로 지정한 곳에 보존하도록 요구.<br/>보안 상의 문제로 일반적인 사이트에서는 보통 사용되지 않고, 인증 기능과 짝을 이루거나 웹끼리 연계하는 설계 양식 사용 시에 이용한다.              | PUT /example.html HTTP /1.1<br/>Host: www.hackr.jp<br/>Content-Length: 1500(1500 바이트 데이터) | 상태 코드 204 No Content 리스폰스를 되돌려준다.                                                                                                                                     | 1.0 1.1        |
| HEAD    | 메시지 헤더 취득. GET과 같은 기능이지만 메시지 바디는 돌려주지 않는다. <br/>URI 유효성과 리소스 갱신 시간을 확인하는 목적 등으로 사용.                                                             | HEAD /index.html HTTP /1.1<br/>Host: www.hackr.jp                                         | index.html에 관련된 리스폰스 헤더를 돌려준다.                                                                                                                                        | 1.0 1.1        |
| DELETE  | 파일 삭제. PUT과 반대로 동작하며 리퀘스트 URI로 지정된 리소스의 삭제를 요구. 일반적인 사이트에서는 보통 이용되지 않고, 인증 기능과 짝을 이루거나 REST를 사용하는 경우에 사용                                        | DELETE /example.html HTTP /1.1<br/>Host: www.hackr.jp                                     | 상태 코드 204 No Content의 리스폰스를 되돌려준다. example.html은 서버상에서 삭제되어 있다.                                                                                                       | 1.0 1.1        |
| OPTIONS | 제공하고 있는 메소드의 문의                                                                                                                                 | OPTIONS * HTTP /1.1<br/>Host: www.hackr.jp                                                | HTTP /1.1 200 OK<br/>Allow:GET,POST,HEAD,OPTIONS<br/>(서버가 제공하고 있는 메소드를 되돌려준다)                                                                                         | 1.1            |
| TRACE   | 경로 조사. Web 서버에 접속해 자신에게 통신을 되돌려 받는 루프백을 발생시킨다. 리퀘스트에 "Max-Fowards" 라는 헤더 필드에 수치를 포함시켜 서버를 통과할 때마다 수치를 줄여가고, 수치가 0이 된 곳을 끝으로 상태 코드 200 OK를 되려준다. | TRACE / HTTP /1.1<br/>Host: hackr.jp<br/>Max-Forwards: 2                                  | HTTP /1.1 200 OK<br/>Content-Type: message/http<br/>Content-Lenght:1024 <br/><br/>Trace / HTTP /1.1<br/>Host: hackr.jp<br/>Max-Forwards:2(리퀘스트 내용을 리스폰스에 포함해서 되돌려준다.) | 1.1            |
| CONNECT | 프록시에 터널링 요구. TCP 통신을 터널링 시키기 위해 사용되며 주로 SSL이랑 TSL등의 프로토콜로 암호화 된 것을 터널링 시키기 위해 사용되고 있다. <br/>메소드의 양식은 `CONNECT 프록시 서버: 포트 HTTP 버전` 으로 이루어져 있다.   | CONNECT proxy.hackr.jp:8080 HTTP /1.1<br/>Host: proxy.hackr.jp                            | HTTP /1.1 200 OK (그 뒤에 터널링을 개시)                                                                                                                                       | 1.1 |

### 지속 연결과 파이프라인화

HTTP 초기 버전에서는 HTTP 통신을 한 번 할 때마다 TCP에 의해 연결과 종료를 해야 할 필요가 있었는데, HTTP가 널리 보급 되어감에 따라 다량의 이미지를 포함한 문서 등이 늘어났고, 리퀘스트를 보낼 때마다 매번 연결과 종료를 하는 불필요한 과정을 해결하기 위해 **지속 연결(Persistent Connections)** 이 고안되었다. 

이는 어느 한 쪽이 명시적으로 연결을 종료하지 않는 이상 TCP 연결을 계속 유지하는 것을 의미하며, TCP 커넥션의 연결과 종료를 반복되는 오버헤드를 줄여주어 서버에 대한 부하가 경감되고 웹 페이지를 빨리 표시할 수 있다는 이점이 있다. 

지속 연결으로 인해 리퀘스트를 병행해서 보내는 **파이프라인화(HTTP Pipelining)** 가 가능해졌다.
일일이 리스폰스를 기다리지 않고 다음 리퀘스트를 보낼 수 있다. 

## 3. HTTP 정보는 HTTP 메시지에 있다.

### HTTP 메시지 구조
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/a4542526-cd63-4589-bf4a-cdcd7ef1d0c5)

### 인코딩
**인코딩(변환)** 은 HTTP로 데이터를 전송할 경우 전송 효율을 높이기 위한 작업으로, 컴퓨터에서 인코딩 처리를 해야하기 떄문에 CPU 등의 리소스는 보다 많이 소비하게 된다.

HTTP 메시지 바디는 리퀘스트/리스폰스에 관한 엔티티 바디를 운반하는 역할을 하는데, 기본 적으로 메시지 바디와 엔티티 바디는 같지만 전송 코딩이 적용된 경우에는 엔티티 바디의 내용이 변화하기 때문에 메시지 바디와 달라진다.
  
메시지 바디는 HTTP 통신의 기본 단위로 옥텟 시퀀스(Octet Sequence, octet은 8비트)로 구성되며 통신을 통해 전송되고, 엔티티는 리퀘스트랑 리스폰스의 페이로드(payload, 부가물)로 전송되는 정보로 엔티티 헤더 필드와 엔티티 바디로 구성된다.

### 콘텐츠 코딩(Content Codings) 
엔티티 정보를 유지한 채로 압축해 송신하고, 수쉰한 클라이언트 측에서 디코딩한다. 
주요 콘텐츠 압축에는 gzip, compress, deflate, identify(인코딩 없음)이 있다.

### 청크 전송 코딩(Chunked transfer Coding) 
사이즈가 큰 데이터를 전송하는 경우 엔티티 바디를 분할하여 전송한다. 
엔티티 바디를 청크(덩어리)로 분해하고 청크 사이를 16진수로 사용해 단락을 표시한 후 엔티티 바디 끝에 0(CR+LF)를 기록해 둔다. 청크 전송 코딩된 엔티티 바디는 수신한 클라이언트 측에서 디코딩한다.

### 멀티 파트
**MIME(Multipurpose Internet Mail Extensions)** 은 이미지 등의 바이너리 데이터를 아스키 문자열에 인코딩하는 방법과 데이터 종류를 나타내는 방법 등을 규정한 다목적 인터넷 메일 확장 사양이다. 
텍스트나 영상, 이미지와 같은 여러 종류의 다른 데이터를 수용하는 방법(**멀티파트, Multipart**)을 사용한다. 
HTTP 메시지로 멀티파트를 사용할 경우 Content-Type 헤더 필드를 사용한다. 

[RFC2046 참고](https://www.rfc-editor.org/rfc/rfc2046)

### 레인지 리퀘스트(Range Request) 
대용량의 이미지와 데이터를 다운로드하는 경우, 중간에 커넥션이 끊어지면 처음부터 다시 다운로드를 해야 하는 문제를 해결하기 위해 리줌(Resume) 기능이 필요해졌다.
리줌을 통해 이전에 다운로드를 한 곳에서부터 다운로드를 재개할 수 있게 되었는데, 이를 위해서는 엔티티의 범위를 지정해서 리퀘스트하는 레인지 리퀘스트가 필요하다. 

Range 헤더 필드를 사용해 리소스의 바이트 레인지를 지정할 수 있으며, 리스폰스는 상태 코드 206 Partial Content 로 돌아온다. 
복수 범위의 경우 multipart/byteranges로 돌아오며, 레인지 리퀘스트를 지원하지 않는 서버의 경우 상태 코드 200 OK 로 완전한 엔티티가 되돌아온다.

### 콘텐츠 네고시에이션(Content Negotiation) 
한국어판, 영어판과 같이 내용은 같으나 여러 개의 페이지를 지닌 웹 페이지가 있다. 
제공하는 리소스의 내용에 대해서 더욱 적합한 것을 제공하고 받기 위해 클라이언트와 서버가 교섭하는 것을 콘텐츠 네고시에이션이라고 한다. 
다음의 세 가지로 구분된다.

#### 서버 구동형 네고시에이션(Server-driven Negotiation) 
서버 측에서 콘텐츠 네고시에이션을 하는 방식으로, 서버 측에서는 리퀘스트 헤더 필드의 정보를 참고해 처리한다. 

#### 에이전트 구동형 네고시에이션(Agent-driven Negotiation) 
클라이언트 측에서 콘텐츠 네고시에이션을 하는 방식으로, 브라우저에 표시된 선택지 중에 유저가 수동으로 선택한다. 
PC버전/모바일 버전처럼 JavaScript를 사용해 웹 페이지에서 자동으로 정하는 경우도 있다. 

#### 트랜스페어런트 네고시에이션(Transparent Negotiation) 
서버 구동형과 에이전트 구동형을 혼합한 것으로 서버와 클라이언트가 각각 콘텐츠 네고시에이션을 한다.

## 4. 결과를 전달하는 HTTP 상태 코드

클라이언트가 서버에게 리퀘스트를 보낼 때, 서버 측에서 그 결과가 어떻게 되었는지 알려주는 것을 **상태 코드** 라고 한다.

  |     | 클래스           | 설명                      | 대표 코드                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
  |:----|:--------------|:------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | 1xx | Informational | 리퀘스트를 받아들여 처리중          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
  | 2xx | Success       | 리퀘스트를 정상적으로 처리          | **200 OK** 클라이언트가 보낸 리퀘스트를 서버가 정상처리했음. 상태 코드와 함께 되돌아 오는 정보는 메소드에 따라 다르다.<br/>**204 No Content** 서버가 리퀘스트를 받아 처리하는 데는 성공했지만 리스폰스에 엔티티 바디를 포함하지 않는다. 클라이언트에서 서버에 정보를 보내는 것으로 족한 경우에 사용됨.<br/>**206 Partial Conent** Range에 의해 범위가 지정된 리퀘스트에 의해 서버가 부분적 GET 리퀘스트를 받았음. 리스폰스에는 지정된 범위의 엔티티가 포함됨                                                                                                                                                                                                                                                                             |
  | 3xx | Redirection   | 리퀘스트를 완료하기 위해 추가 동작이 필요 | **301 Moved Permanently** 리퀘스트된 리소스에는 새로운 URI가 부여되어 있으므로 이후로는 그 리소스를 참조하는 URI를 사용해야 함을 나타낸다.<br/>**302 Found** 301과 비슷하지만 301은 영구적인 이동인 반면 302는 일시적이며 앞으로 이동될 가능성이 있다. **303 See Other** 리퀘스트에 대한 리소스는 다른 URI에 있기 때문에 GET 메소드를 사용해서 얻어야 함을 의미. 302와 같은 기능이지만 리다이렉트 장소를 GET으로 얻어야 한다고 명확하게 명시한다는 차이가 있음<br/>**304 Not Modififed** 클라이언트가 조건부 리퀘스트를 했을 때 리소스에 대한 액세스는 허락하지만 조건이 충족되지 않음을 표시. 리스폰스 바디에 어떤 것도 포되서는 안되며 리다이렉트와는 사실 관계가 없다.<br/>**307 Temporary Redircet**                                                                                                    |
  | 4xx | Client Error  | 서버는 리퀘스트 이해 불가능         | **400 Bad Request** 리퀘스트 구문이 잘못되었음. 리퀘스트 내용을 재검토하고 나서 재송신할 필요가 있으며 브라우저는 이것을 200 OK와 같이 취급한다.<br/>**401 Unauthoroizezd** 송신한 리퀘스트에 HTTP 인증(BASIC, DIGEST 인증) 정보가 필요하다는 것을 나타내며, 리스폰스를 되돌리는 경우에는 리퀘스트 된 리소스에 적용되는 challenge를 포함한 WWW-Authenticate 헤더 필드를 포함할 필요가 있다. 최초의 401인 경우는 인증을 위한 다이얼로그가 표시되며 두번째 리스폰스에 경우에는 인증에 실패했음을 표시한다. <br/>**403 Forbidden** 리퀘스트된 리소스의 액세스가 거부되었음. 거부의 이유를 명확하게 하는 경우에는 엔티티 바디에 기재해서 유저 측에 표시한다. 퍼미션이 부여되지 않은 경우나 액세스 권한 문제 등이 포함된다.<br/>**404 Not Found** 리퀘스트한 리소스가 서버상에 없다는 것을 나타내며, 서버 측에 해당 리퀘스트를 부하고 싶은 이유를 분명히 하고 싶지 않은 경우에도 이용한다. |
  | 5xx | Server Error  | 서버는 리퀘스트 처리 실패          | **500 Internal Server Error** 서버에서 리퀘스트를 처리하는 도중에 에러가 발생하였음. 웹 애플리케이션에 에러가 발생한 경우 일시적인 경우도 있다.<br/>**503 Service Unavailable** 일시적으로 서버가 과부하 상태이거나 점검중이기 때문에 현재 리퀘스트를 처리할 수 없음. 상태가 해소되기까지 시간이 걸리는 경우에는 Retry-After 헤더 필드에 따라 클라이언트에 전달하는 것이 바람직하다.                                                                                                                                                                                                                                                                                                                     |

클래스의 정의만 지킨다면 상태 코드를 변경하거나 서버 독자의 상태 코드를 만들어도 무관하다. [RFC2616](https://www.rfc-editor.org/rfc/rfc2616)과 WebDAV의 확장을 포함하면 상태 코드는 60가지 이상이지만 실제로 자주 사용되고 있는 것은 14가지 정도이다.
  
상태 코드가 현재 상황과 불일치할 수도 있음을 유념하자. 

## 5. HTTP와 연계하는 웹 서버

### 가상 호스트
HTTP/1.1에서는 하나의 HTTP 서버에 여러 개의 웹 사이트를 실행할 수 있다. 
고객마다 다른 도메인을 가지고 다른 웹사이트를 실행할 수 있게 하기 위해 **가상 호스트(Virtual Host)** 라는 기능을 사용한다. 
이를 이용하면 물리적으로는 서버가 1대지만 가상으로 여러 개가 있는 것처럼 설정하는 것이 가능하다.

같은 IP 주소에서 다른 호스트명과 도메인을 가진 여러 개의 여러 웹 사이트가 실행되는 가상 호스트 시스템 때문에, HTTP 리퀘스트에서는 호스트명과 도메인 명을 완전히 포함한 URI를 지정하거나 Host 헤더 필드에서 지정해주어야 한다.

### 통신 중계 프로그램
HTTP는 클라이언트와 서버 이외에 프록시(Proxy), 게이트웨이(Gateway), 터널(Turnel)과 같은 통신을 **중계**하는 프로그램과 서버를 연계하는 것도 가능하다. 
  이러한 프로그램과 서버는 다음 서버에 리퀘스트를 중계하고, 서버로부터 받은 리스폰스를 클라이언트에 반환한다.<br><br>

#### 프록시
서버와 클라이언트의 양족 역할을 하는 중계 프로그램으로 클라이언트로부터 리퀘스트를 서버에 전송하고, 서버로부터의 리스폰스를 클라이언트에 전송한다. 

클라이언트로부터 받은 리퀘스트 URI를 변경하지 않고 그 다음의 리소스를 가지고 있는 서버에 보내며, 리소스 본체를 가진 서버를 **오리진 서버(Origin Server)** 라고 부른다. 
오리진 서버로부터 되돌아온 리스폰스는 프록시 서버를 경유해 클라이언트에 돌아온다.

프록시 서버를 여러 대 경우하는 것도 가능하며 중계 시에는 Via 헤더 필드에 경유한 호스트 정보를 추가한다.

캐시를 사용해 네트워크 대역을 효율적으로 사용하고, 조직 내 특정 웹 사이트에 대한 액세스 제한, 액세스 로그를 획득하는 정책을 지키려는 목적으로 사용하는 경우가 있다.

프록시는 다음과 같이 캐시 여부와 메시지 변경 여부로 구분된다.

##### 캐싱 프록시(Cashing Proxy)
프록시 서버 상에 리소스 캐시를 보존해 두는 타입의 프록시로, 프록시에 다시 같은 리소스에 리퀘스트가 온 경우 오리진 서버로부터 리소스를 획득하는 것이 아니라 캐시를 리스폰서로서 되돌려 준다.

##### 투명 프록시(Transparent Proxy)
프록시로 리퀘스트와 리스폰스를 중계할 때 메시지 변경을 하지 않는다. 반면 변경을 가하는 타입의 프록시는 비투과 프록시라고 한다.

#### 게이트웨이
다른 서버를 중계하는 서버로, 클라이언트로부터 수신한 리퀘스트를 리소스를 보유한 서버인 것처럼 수신하여 경우에 따라서는 클라이언트가 상대가 게이트웨이인 것을 인지하지 못하는 경우도 있다.
동작은 프록시와 유사하나 게이트오ㅞ이의 경우에는 그 다음 서버가 HTTP 서버 이외의 서비스를 제공하는 서버가 된다. 
클라이언트와 게이트웨이 사이를 암호화하는 등으로 안전하게 접속함으로써 통신의 안정성을 높이는 역할을 한다.

#### 터널
서로 떨어진 클라이언트와 서버 사이를 중계하며 접속을 주선하는 중계 프로그램으로, SSL과 같은 암호화 통신을 통해 서버와 안전하게 통신하고자 할때, 안전한 통신로를 확립하기 위해 사용한다.
터널은 통신하고 있는 양쪽 끝의 접속이 끊어질 때 종료한다. 

### 캐시(Cache)
캐시는 프록시 서버와 클라이언트의 로컬 디스크에 보관된 리소스의 사본을 가리킨다. 
캐시를 이용하면 리소스를 가진 서버에 액세스하는 것을 줄일 수 있으므로 통신량과 통신 시간을 절약할 수 있다.

캐시 서버는 프록시 서버 중 하나로 캐싱 프록시로 분류되며 서버로부터의 리스폰스를 중계하는 시점에 프록시 서버 상에 리소스의 사본을 보존한다.

#### 캐시의 유효성
같은 캐시를 계속해 사용하다보면 오리진 서버에 있는 원래 리소스가 갱신되는 경우에도 낡은 리소스를 그대로 보내게 될 수 있다. 
캐시를 가지고 있더라도 클라이언트의 요구나 캐시의 유효 기간 등에 의해 오리진 서버에 리소스의 유효성을 확인하거나 새로운 리소스를다시 획득하러 가는 경우가 있다.

#### 클라이언트의 캐시
캐시 서버뿐만 아니라 클라이언트가 사용하는 브라우저에서도 캐시를 가질 수 있다.
인터넷 익스플로러에서 클라이언트가 보존하는 캐시를 인터넷 임시 파일이라고 부르며, 브라우저가 유효한 캐시를 가진 경우 서버에 액세스하는 게 아니라 로컬 디스크로부터 불러온다.
캐시 서버와 마찬가지로 리소스가 오래된 것으로 판단된 경우에는 오리진 서버에 유효성을 확인하거나 새로운 리소스를 다시 획득하러 간다.