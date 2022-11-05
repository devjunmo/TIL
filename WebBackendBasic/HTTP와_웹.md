# HTTP와 웹

## HTTP를 통한 클라이언트와 서버간 통신 큰그림
1. 웹 브라우저에서 URL을 HTTP request message로 만들어서 서버로 송신한다.
2. HTTP 메세지는 Application layer에서 Socket을 통해 Transport, Network, Data link, Physical layer를 거쳐 서버로 전달된다.
3. 각 layer를 거치는 과정에서 http message는 TCP/IP 패킷으로 감싸지게됨.
4. 서버에서 해당 패킷을 decapsulation하게되고, http message를 해석한 후, HTTP response message를 생성하여 다시 클라이언트로 보내준다.
5. 참고로 자바 서블릿에서는 HttpServletResponse 객체의 out buffer에 data를 append 해주고, 그것을 웹서버로 보낼 떄 HTTP Response message로 변환하게 된다. 웹서버는 그것을 받아서 클라이언트로 보내는것!


## HTTP(Hyper Text Transfer Protocol) 기본 지식
- 하이퍼 링크와 하이퍼 텍스트
  - 하이퍼 링크는 밑줄로 되어있는 링크를 의미, 특정 파일의 위치를 지정 가능
  - 하이퍼 텍스트는 하이퍼 링크를 나타낼수 있는 텍스트
- HTTP는 TCP/IP 5 layer의 최상단인 Application layer에서 사용하는 프로토콜. 원래는 Hyper Text를 전송하려고 만들어 졌으나, 이제는 거의 모든 형태의 데이터를 전송하는데 쓰인다.
  - HTML, text, image, JSON, XML ....
- 서버와 클라이언트간 뿐만 아니라 서버와 서버간 데이터 송수신에도 사용됨.
- HTTP 1.1버전에 필요한 거의 모든 기능이 있고, 1.1버전의 성능을 개선한것이 HTTP 2버전, TCP대신 UDP를 사용한것이 3버전.(1.1, 2버전은 TCP로 동작함)

## HTTP 특징
- Stateless
  - Stateless란 클라이언트의 상태를 저장하지 않는다는것.
    - A라는 클라이언트가 서버로 처음 request를 보내고 나서 다시 서버로 요청을 보낼때, 서버에서는 A라는 클라이언트가 요청을 보냈다는 사실을 기억하지 못함. A라는 클라이언트가 왔다라는 정보를 저장하지 않기 때문임. 
    - stateless의 장점은 대용량 트래픽을 대비해 서버를 여러개 두는 상황에서 나오는데, A-1서버와 통신하다 A-1서버가 죽어버려도 A-2서버에 요청하면 필요한 응답을 받을 수 있음. 즉 scale out(하나의 서버에서 하던 일을 여러 서버가 하도록 수평 확장을 하는것)에 유리하다.
    - stateless의 단점으로는 한번에 전송되는 data 양이 많아질 수 있는 점이다. 요청 한번에 보낼때 필요한 응답을 위한 파라미터들을 모두 갖춰서 보내야 하기 때문.
    - 로그인같이 상태가 유지되어야 하는 작업의 경우, 서버 세션등을 사용하는데, 이 상태 유지는 최소한으로 사용해야 한다.

계속..