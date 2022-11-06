# HTTP와 웹

## ✒︎ HTTP를 통한 클라이언트와 서버간 통신 큰그림
1. 웹 브라우저에서 URL을 HTTP request message로 만들어서 서버로 송신한다.
2. HTTP 메세지는 Application layer에서 Socket을 통해 Transport, Network, Data link, Physical layer를 거쳐 서버로 전달된다.
3. 각 layer를 거치는 과정에서 http message는 TCP/IP 패킷으로 감싸지게됨.
4. 서버에서 해당 패킷을 decapsulation하게되고, http message를 해석한 후, HTTP response message를 생성하여 다시 클라이언트로 보내준다.
5. 참고로 자바 서블릿에서는 HttpServletResponse 객체의 out buffer에 data를 append 해주고, 그것을 웹서버로 보낼 떄 HTTP Response message로 변환하게 된다. 웹서버는 그것을 받아서 클라이언트로 보내는것!


## ✒︎ HTTP(Hyper Text Transfer Protocol) 기본 지식
- 하이퍼 링크와 하이퍼 텍스트
  - 하이퍼 링크는 밑줄로 되어있는 링크를 의미, 특정 파일의 위치를 지정 가능
  - 하이퍼 텍스트는 하이퍼 링크를 나타낼수 있는 텍스트
- HTTP는 TCP/IP 5 layer의 최상단인 Application layer에서 사용하는 프로토콜. 원래는 Hyper Text를 전송하려고 만들어 졌으나, 이제는 거의 모든 형태의 데이터를 전송하는데 쓰인다.
  - HTML, text, image, JSON, XML ....
- 서버와 클라이언트간 뿐만 아니라 서버와 서버간 데이터 송수신에도 사용됨.
- HTTP 1.1버전에 필요한 거의 모든 기능이 있고, 1.1버전의 성능을 개선한것이 HTTP 2버전, TCP대신 UDP를 사용한것이 3버전.(1.1, 2버전은 TCP로 동작함)

## ✒︎ HTTP 특징
- Stateless
  - Stateless란 클라이언트의 상태를 저장하지 않는다는것.
    - A라는 클라이언트가 서버로 처음 request를 보내고 나서 다시 서버로 요청을 보낼때, 서버에서는 A라는 클라이언트가 요청을 보냈다는 사실을 기억하지 못함. A라는 클라이언트가 왔다라는 정보를 저장하지 않기 때문임. 
    - stateless의 장점은 대용량 트래픽을 대비해 서버를 여러개 두는 상황에서 나오는데, A-1서버와 통신하다 A-1서버가 죽어버려도 A-2서버에 요청하면 필요한 응답을 받을 수 있음. 즉 scale out(하나의 서버에서 하던 일을 여러 서버가 하도록 수평 확장을 하는것)에 유리하다.
    - stateless의 단점으로는 한번에 전송되는 data 양이 많아질 수 있는 점이다. 요청 한번에 보낼때 필요한 응답을 위한 파라미터들을 모두 갖춰서 보내야 하기 때문.
    - 로그인같이 상태가 유지되어야 하는 작업의 경우, 서버 세션등을 사용하는데, 이 상태 유지는 최소한으로 사용해야 한다.

- Connectionless
  - Connectionless란 기본적으로 요청/응답 후 연결을 끊는것.
  - Connectionless의 장점은, 여러 클라이언트가 요청을 보낼 때 연결을 유지하지 않기 때문에 서버의 자원 사용량을 최소화 할 수 있음.
    - 10만명이 사용하더라도 동시에 클릭하는 경우는 10만보다 훨씬 적기 때문에 서버 자원을 효율적으로 사용 가능.
  - Connectionless의 단점은, 요청/응답당 연결을 계속 새로 만들어야 하는것인데, TCP의 경우 연결을 맺을 때 3 way handshake를 한다.
    - 즉 연결 할때마다 3 way handshake를 해야함.
    - 또한 html을 받은 후에, 그 html 내부의 js나 image등등을 추가로 가져올때도 3 way handshake를 하기 때문에 낭비가 발생
  - Connectionless의 단점을 극복하기 위해 HTTP 지속연결(persistent connection)을 사용
    - html을 요청하고, 웬만한 html을 모두 해석할때 까지 연결을 유지하여 js나 img등을 추가로 가져오는데 3 way handshake의 낭비가 없도록 함.

## ✒︎ HTTP Message 구조
### ❆ HTTP 메세지 구조
```
start-line 
*( header-field CRLF )  
CRLF  
[ message-body ]  

참고로 CRLF는 줄바꿈임
```


### ❆ 요청 메세지
- start-line
  - method path HTTP_version
    - path는 /member?id="jes" 형태. 절대경로 사용
- Header
  - HTTP body를 제외한 전송에 필요한 모든 정보 
  - 수많은 표준 header 요소들이 있음
- Message body
  - 실제 전송할 데이터, 거의 모든 데이터 전송 가능


### ❆ 응답 메세지 
- start-line
  - HTTP_version status_code reason_phrase
    - status_code는 200성공, 400클라이언트오류 등
    - reason_phrase은 사람이 알아먹을 수 있는 짧은 텍스트 
- Header
  - HTTP body를 제외한 전송에 필요한 모든 정보 
  - 수많은 표준 header 요소들이 있음
- Message body
  - 실제 전송할 데이터, 거의 모든 데이터 전송 가능

### ❆ HTTP 표준 header 정리


## ✒︎ HTTP Method
### ❆ URI는 리소스 식별만 하고, 행위는 메소드로 분리한다.
- URI: /members/{id}  -> id에 해당하는 멤버를
- Method: C(POST) R(GET) U(PUT,PATCH) D(DELETE) 해주세요 

### ❆ HTTP 메소드 종류
- GET
  - 캐싱때문에 조회에 유리.
- POST
  - 신규 리소스 등록을 하거나 리소스 생성 없이 어떤 프로세스를 처리할때 사용
  - POST로 모든것을 할수 있음, 즉 애매하면 POST.
- PUT
  - 리소스를 완전히 대체하며, 없으면 생성하기
  - /members/100 + PUT = id 100인 유저를 완전히 대체하세요 (클라이언트가 리소스 위치를 정확히 알고 URL을 지정한다)
  - (POST와 차이) /members + POST = 어디든지 신규 회원을 생성하세요
- PATCH
  - PUT의 '완전히'를 보완.
  - 수정할 일부 데이터만 보내서 일부만 대체.
- DELETE
  - 데이터 삭제
- HEAD
  - 조회이고, 메세지 부분을 제외하고 상태줄과 헤더만 반환함
- OPTIONS
  - 타겟 리소스나 서버와 통신할때 어떤 통신 옵션(method, header, content type)을 지원하는지 알 수 있음
  - 특정 Http 메소드로 요청을 보내기 전에 OPTIONS 메소드를 미리 보내보고, 응답을 확인한 후, 내 통신 옵션이 가능 한지를 판단하고 보낸다.


### ❆ HTTP 메소드 속성
- Safe(안전) Methods
  - 내 http메소드가 리소스를 '변경' 하지 않으면 안전, 변경하면 안전x
  - get이나 head는 조회만 하니까 안전, 나머지는 안전x
- Idempotent(멱등) Methods
  - 나 혼자만 "같은요청"을 여러번 했을때 돌아오는 결과가 같으면 멱등하다.
    - GET으로 같은 내용을 조회하면 같은 결과가 나온다
    - PUT, PATCH으로 같은 내용을 대체하면 같은 결과가 대체된다
    - DELETE로 같은 내용을 삭제하면 같은 결과가 삭제된다
    - ## POST는????
      - 멱등하지 않음.
      - 같은 내용으로 회원가입 = 중복 회원가입
      - 같은 내용으로 주문 = 중복 주문
  - 멱등이란 개념은 왜 존재하나?
    - 서버측에서 TIMEOUT등의 이유로 정상 응답을 주지 못했을 때 클라이언트 쪽에서 '자동'으로 재요청을 해도 될지 말지에 대한 '근거'로써 작용함!! (자동 복구 메커니즘 구현시 중요)
- Cacheable(캐시가능) Methods
  - 캐시: 같은 리소스를 계속 다운 받으면 비효율적이니 웹 브라우저가 내 로컬pc에 리소스를 저장하는것
  - 응답 결과를 웹브라우저가 캐시해서 사용해도 되는가에 대한것
    - GET, HEAD, POST, PATCH -> 캐시 가능
    - 실제로는 GET, HEAD정도만 캐시로 사용. (POST, PATCH는 구현이 어려움)


## 참고자료
인프런 김영한님 http 강의


계속..