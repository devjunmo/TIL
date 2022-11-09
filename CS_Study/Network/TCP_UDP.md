# TCP(Transmission Control Protocol)와 UDP(User Datagram Protocol)

## ✒︎ 4계층 (전송계층)

- Transport layer는 end point(사용자)간 데이터를 특정 포트번호에 해당하는 프로세스에 전송하는 계층
- 데이터를 전달함에 있어 신뢰성이 중요한 경우(email, 웹페이지 등) TCP를 쓰고, 신뢰성보다는 속도적인 측면이 중요한 경우(영상, 오디오 등) UDP를 쓴다.

## ✒︎ TCP(Transmission Control Protocol)

- TCP는 신뢰성 있는 데이터 전송을 가능하게 해준다.
- 사용자간 데이터의 순차 전송을 보장하며(순서 뒤바뀜 없다는것), 데이터 손실 없이 (데이터가 손실나면 다시 요청)안정적으로 전달한다.
- TCP는 양방향 통신으로 3 way handshake 방식으로 통신을 시작하며, 4 way handshake 방식으로 통신을 종료한다.
- TCP는 가상회선 패킷 교환방식을 사용한다.
  - 세그먼트 또는 데이터그램 단위를 전송하기 전에 가상 경로를 설정하고, 동일한 message에 속한 데이터들은 동일한 경로로 순차적으로 전송된다.
  - 속도면에선 UDP의 데이터 그램 패킷 교환방식보다 느리지만, 순서를 보장할 수 있는 장점이 있다.
- TCP 프로토콜 안에서 윗 레이어에서 내려온 데이터를 쪼개어 상대방에게 전송한다. 쪼갠 데이터에는 TCP 헤더가 붙고, 그 단위를 Segment라 한다.
- TCP의 또다른 특징으로는 흐름제어, 혼잡제어, 오류감지를 수행하는것을 들수있다.
  - 흐름제어: 송신자는 한번에 보낼 수 있는 데이터의 양을 체크하고, 수신자는 데이터를 어디까지 받았는지 지속적으로 체크한다.
    - Window size = TCP header에 한번에 보낼 수 있는 양을 정한다.
    - Acknowledgment Number = 수신자가 지금까지 받은 데이터 정보를 송신자에게 전달하는 정보 (이 데이터를 바탕으로 송신자는 어디까지 보냈는지 알수있음)
    - Sequence Number = 데이터 순서를 표기한 정보
  - 혼잡제어: End point가 아닌 중간의 네트워크 망의 혼잡함을 극복할 수 있는 방법들
    - Slow start = 연결 초기에 송출량을 낮게잡고 수신자의 수신 상태가 양호 하다면 점차 송출량을 늘리는 방식

### TCP는 데이터에 header를 붙여 보낼 데이터에 TCP 정보를 표현한다.

![TCP_Header](/img/TCP_header.png)

- Source port, Destination port
  - segment의 출발지와 목적지를 나타내는 필드. 4계층이므로 포트번호가 담긴다.
- Sequence number
  - 전송하는 데이터 순서를 의미. 이를 통해 segment들의 순서를 알수있고, 재조립 할수있다.
- Acknowledgment Number
  - 수신부가 어디까지 받았는지 송신부에 알려주어 다음에 보낼 데이터의 시작점을 알수 있도록 한다.
- Data offset
  - 세그먼트의 헤더가 아닌 데이터가 시작되는 위치를 표시
- Reserved 3bit
  - 미래를 위해 예약된 필드, 0으로 채워져야 함
- 9 bit Flags
  - ACK(필드에 값이 있음을 알림, 0이면 무시된다), SYN(상대와 연결을 생성한다는 플래그), FIN(상대와 연결을 종료하고 싶다는 세그먼트임을 의미)
- Window Size
  - 한번에 전송 할수있는 데이터 양을 의미하는 값을 담는다.
- Checksum
  - 오류 검출을 위한 값
- Urgebt pointer
  - 긴급 포인터로 1이면 이 포인터가 가리키는 데이터 우선 처리

## TCP의 문제점?

- 매번 Connection을 연결하기 때문에 시간 손실이 발생한다.
- 패킷이 조금만 손실되어도 무조건 재전송된다. 경우에 따라 (ex, 영상 데이터를 주고받을때) 불합리하다.
- 특정 경우에서 방생하는 불합리를 극복하기 위해 UDP가 사용된다.

## ✒︎ UDP(User Datagram Protocol)

- TCP와는 다르게 단방향 통신이다. 때문에 신뢰성이 떨어지나 전송속도가 일반적으로 빠르다.
- UDP는 Connectionless하다 하는데, 이는 송수신자간 연결 설정 단계인 handshaking이 없다는 의미이다.
- 순차전송 x, 흐름제어 x, 혼잡제어 x이며 체크섬에 의해 에러만 검출해준다.
- UDP는 데이터 그램 패킷 교환방식을 사용한다.
  - 모든 패킷들을 독립적으로 최적의 경로로 전송하고, 도착지에서 재조립 하는 형태이다.
  - 순서는 보장할 수 없지만 속도면에서 가상회선 패킷 교환방식보다 이점이 있다.
- 영상 스트리밍같은 신뢰서이 중요하지 않은 경우 사용한다.
- TCP와 다르게 데이터를 쪼개지 않고 UDP 헤더만 붙여서 데이터를 보낸다.

## 참고자료

https://en.wikipedia.org/wiki/Transmission_Control_Protocol
https://aws-hyoh.tistory.com/entry/TCPIP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0
https://www.youtube.com/watch?v=ikDVGYp5dhg
https://evan-moon.github.io/2019/11/10/header-of-tcp/#:~:text=TCP%EB%8A%94%20%EC%97%AC%EB%9F%AC%20%EA%B0%9C%EC%9D%98%20%ED%95%84%EB%93%9C,%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8%EC%9D%98%20%EC%A0%95%EB%B3%B4%EB%A5%BC%20%EB%82%98%ED%83%80%EB%82%B8%EB%8B%A4.
