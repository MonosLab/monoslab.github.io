---
title: "[ ETC ] 웹소켓" 
sub: post
author: Kwangsoo Seo
date: 2022-10-28 06:00:00 +0900
categories: [정보, 포맷]
tags: [HTTP, WebSocket, 웹소켓, Comet, 비교]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post17.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post17)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# 웹소켓   
웹소켓은 HTML5에 포함된 표준 기술로서 HTTP 환경에서 서버와 클라이언트간에 지속적인 연결을 통해 실시간으로 전이중방식(Full-Duplex)의 통신을 가능하게 하는 프로토콜입니다.
> * 단방향(Simplex) 통신 : 한쪽 방향으로만 전송이 가능한 방식 (예) 라디오, TV   
> * 전이중(Full-Duplex) 통신 : 동시에 양방향 전송이 가능한 방식으로, 전송량이 많고, 전송 매체의 용량이 클 때 사용 (예) 전화, 전용선을 이용한 데이터 통신  
> * 반이중(Half-Duplex) 통신 : 양방향 전송이 가능하지만 동시에 양쪽 방향에서 전송할 수 없는 방식 (예) 무전기, 모뎀을 이용한 데이터 통신   


HTTP와 마찬가지로 OSI 제7계층에 속하며, 제4계층의 TCP에 의존적입니다. 2011년 IETF에 의해 RFC 6455로 표준화 되었으며, W3C에 의해 웹 IDL의 웹소켓 API가 표준화 되었습니다.   
> 웹 IDL은 웹 브라우저에 구현되는 Application Programming Interface(API; 응용 프로그램프로그래밍 인터페이스)를 기술하기 위한 Interface Description Language(IDL; 인터페이스 기술 언어) 형식입니다.   


# 1. HTTP와 WebSocket의 비교   

### HTTP   
서버와 클라이언트가 서로 연결을 유지하며 데이터를 관리하면 자원이 필요하기 때문에 요청이 올때만 응답을 함(Stateless)   
a. 클라이언트의 요청이 있을 때마다 매번 새로운 커넥션을 생성하고 작업이 끝나면 커넥션이 닫힘. (Half-Duplex)   
b. 형식을 따르기만 하면 쉽게 해석 가능   
c. 인터넷 역사상 가장 성공적인 프로토콜(즉시 사용할 수 있는 엄청난 수의 서버/클라이언트 애플리케이션들과 역시 엄청난 수의 (게다가 품질도 뛰어난)라이브러리들을 가지고 있음.)   

### WebSocket   
한번의 연결으로 지속적으로 서로간 데이터를 보낼 수 있음 (StateFull)   
a. HTML5 표준 기술, 사용자의 브라우저와 서버 사이의 동적인 양방향 연결 채널을 구성. (Full-Duplex)   
b. Header가 상당히 작아 Overhead가 적음.   
c. 형식이 정해져 있지 않아 해석이 어려움.   
d. UTF8 포멧의 메시지 스트림만 허용 (참고 : 일반 소켓 통신의 경우 Bytes 스트림을 사용)   

### 공통점   
a. OSI 모델 7계층, 4계층의 TCP에 의존   
b. 기본적으로 80포트, 443포트 위에 동작(변경 가능)   
c. HTTP 프록시 및 중간 층을 지원하도록 설계   

### 차이점   
a. 클라이언트에 의해 요청을 받는 방식이 아닌, 서버가 내용을 클라이언트에 보내는 표준화된 방식을 제공   
b. 연결이 유지된 상태에서 메시지들을 오갈 수 있게 허용함   
c. 양방향 대화 방식   
d. 통신은 TCP 포트 80(TLS 암호화 연결의 경우 443)를 통해 수행되며 방화벽을 통해 웹이 아닌 인터넷 연결을 차단하는 일부 환경에 도움.   
e. TCP 위에서 메시지 스트리밍을 가능케 함.    
f. TCP 단독으로는 메시지의 상속 개념 없이 바이트 스트림을 다룬다. (?)   
g. WebSocket 프로토콜은 접속 확립에 HTTP를 사용하지만, 그 후 통신은 WebSocket 독자의 프로토콜로 이루어짐.   

# 2. 웹소켓 이전의 실시간 통신 방법(HTTP Comet)   
Comet은 HTTP상에서 데이터를 push하기 위한 방식 자체를 일컫는 모든 기술을 말합니다.  가장 대표적인 것들은 아래의 3가지가 있습니다.   

### polling   
- 서버로 일정 주기로 요청   
- 실시간 통신에서는 언제 통신이 발생할지 예측이 불가하여, 불필요한 요청과 커넥션이 발생   

### long polling   
- 서버에 요청하고 응답을 받기 전까지 연결을 유지.   
- 많은 양의 메시지가 발생할 경우 polling과 유사.   

### (Multipart) streaming   
- 서버에 요청을 보내고 연속적으로 데이터를 수신.   
- 클라이언트에서 서버로의 데이터 송신이 어려움.   

> Multipart streaming은 multipart/x-mixed-replace를 이용해서, 연결을 끊지 않고 하나의 HTTP 연결에서 데이터를 보내는 방식입니다.. 서버측은 Content-type를 multipart/x-mixed-replace로 하고 스트림 메시지의 boundary를 구분하기 위한 boundary문자를 정의하면 됩니다.   

# 3. 웹 소켓 동작 방법   
연결 수립 과정은 HTTP를 이용합니다.   

- 최초 접속시 HTTP 프로토콜 위에 HandShaking을 위해 HTTP Header를 사용 (기본 포트 HTTP : 80, HTTPS : 443 이용)
```html   
GET /mychat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat
Sec-WebSocket-Version: 13
Origin: http://example.com
```   
\<HTTP기반의 연결을 WebSocket기반으로 upgrade하겠다고 요청하는 과정\>   
- HTTP를 웹소켓 프로토콜로 바꾸기 위해 Protocol switching 과정을 진행   
```html   
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```
\<HTTP를 WebSocket으로 switching 하겠다고 응답하는 과정\>   
- HTTP 대신 WS와 WSS 프로토콜로 데이터 송/수신(UTF8인코딩 데이터를 0x00과 0xff 사이에 데이터를 넣어 보냄)   
b. 연결 수립 후 서버와 클라이언트 간에 TCP/IP 기반 웹 소켓 연결이 이루어지고, 일정 시간이 지나면 HTTP 연결은 자동으로 끊김   
c. 프레임으로 구성된 메시지라는 논리적 단위로 송/수신   

# 4. 웹소켓 프로토콜의 데이터 프레임
![WebSocketDataFrame](https://monoslab.github.io/assets/img/posts/websocket_data_frame.png) 

RSV1-3 는 사용되지 않습니다. 이 필드들은 확장 프로토콜 또는 미래를 위해 예약되었습니다.

MASK 비트는 메세지가 인코딩되어있는지의 여부를 나타냅니다.클라이언트가 서버로 보내는 메세지는 항상 마스킹되어야합니다. 따라서 서버는 클라이언트로부터 받은 이 필드가 항상 1임을 기대할 수 있습니다. (만약 클라이언트가 마스킹되지 않은 메세지를 보낸다면 서버는 연결을 종료해야 합니다. 참고 : section 5.1 of the spec ). 서버가 클라이언트에게 보내는 메세지는 MASK 필드가 항상 0이고 데이터는 마스킹되지 않은 상태여야 합니다. 마스킹이 어떻게 이루어지는지 / 마스킹된 메세지를 어떻게 디코딩하는지는 나중에 설명합니다. (주의: wss 연결이라고 하더라도 클라이언트가 보내는 패킷은 항상 마스킹되어야 합니다.)

opcode 필드는 뒤따라오는 payload 데이터가 어떠한 포멧인지를 나타냅니다. 0x0은 continuation, 0x1은 텍스트(항상 UTF-8), 0x2는 바이너리 데이터 / 나머지 값은 "컨트롤 코드"에 사용됩니다. (컨트롤 코드에 대해서는 나중에 다루게 됩니다.) 현재 버전의 웹소켓 프로토콜에서는 0x3 / 0x7 / 0xB~0xF는 아무런 의미도 지니고있지 않습니다.

FIN 비트는 이 메세지가 마지막임을 나타냅니다. 만약 FIN 비트가 0이라면 서버는 뒤에 더 따라오게 될 메세지들까지 수신해야 합니다. FIN 비트가 1일 경우에는 전체 메세지가 수신되었으므로 이를 처리합니다. 이 부분에 대해서는 뒤에 다시 설명합니다.

Payload 길이 알아내기
수신한 프레임으로부터 payload 데이터를 읽기 위해서는 payload length 필드를 읽어야 합니다. 불행히도 이는 약간 복잡한 작업을 거치는데 아래 순서대로 처리해 주세요.

9번째부터 15번째까지의 비트를 읽습니다. 이를 unsigned integer로 취급한 다음 값이 125거나 이보다 작을 경우 이 자체가 payload length 입니다. 이 경우에는 2, 3 과정은 필요 없습니다. 하지만 126이면 2번으로, 127일 경우 3번으로 가주세요
다음 16비트를 읽습니다. 이를 unsigned integer로 처리하고 payload length 값으로 사용합니다.
다음 64비트를 읽습니다. 이를 unsigned integer로 처리하고 payload length 값으로 사용합니다. (최상위 비트는 항상 0이어야 합니다.)
마스킹된 Payload 데이터 디코딩하기
MASK 비트가 설정되어 있디만 32비트 사이즈의 Masking-Key 필드 또한 존재하게 됩니다. 아래 의사코드는 Payload 데이터를 ENCODED / Masking-Key 필드를 MASK 라고 가정하고 데이터를 디코딩하는 방법을 보여줍니다. ENCODED값을 0부터 순회하면서 MASK의 i % 4에 해당하는 1바이트와 XOR 연산을 수행합니다.

# 5. 조건에 따른 적절한 활용   

### http   
- 실시간 업데이트가 필요하지 않을 때   
- 단건의 데이터 요청일 때   
- Restful 서비스로 데이터를 가져올 때   
- 단방향 통신(Half-Duplex)   
- 대표적인 사용 예 : Ajax를 이용한 개발   

### WebSocket   
- 실시간 업데이트가 필요할 때   
- 지속적으로 데이터를 주고 받을 때   
- 양방향 통신(Full-Duplex)   
- 대표적인 사용 예 : 게임, 채팅 앱 등   

# 6. 참고 자료
* https://velog.io/@gth1123/http-vs-socket   
* https://blog.naver.com/pje0721/222835392496   
* https://www.joinc.co.kr/w/man/12/websocket
* https://ws-pace.tistory.com/104 <그림으로 잘 설명됨>   
* https://kbj96.tistory.com/46  ==> https://blog.scaleway.com/iot-hub-what-use-case-for-websockets/   
* https://velog.io/@ranja/%EC%9B%B9-%EC%86%8C%EC%BC%93%EA%B3%BC-HTTP%EC%9D%98-%EC%B0%A8%EC%9D%B4   
* https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_servers   
* https://doozi0316.tistory.com/entry/WebSocket%EC%9D%B4%EB%9E%80-%EA%B0%9C%EB%85%90%EA%B3%BC-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95-socketio-Polling-Streaming <http header가 잘 설명되어 있음>   
