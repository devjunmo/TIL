# OSI 7 layer

## 1. 네트워크 레이어의 큰 그림

### ✒︎ 네트워크 아키텍쳐 종류들
![network layer](/img/NetworkModel.png)

현재는 가장 오른쪽의 update 버전 TCP/IP 모델을 주로 사용한다. 때문에 정리는 5 layer를 메인으로 할것 

각 layer에서는 HTTP, TCP, IP와 같은 특정한 '프로토콜'이 수행된다.

### ✒︎ TCP/IP(Transmission Control Protocol/Internet Protocol) 5 layer model
![network layer](/img/TCP_IP_5layer.png)

- 네트워크 시스템은 Layered Architecture를 따르고, 각 레이어를 거치며 data가 encoding(또는 캡슐화) 또는 decoding(또는 비캡슐화) 되어 데이터를 송수신 하게 된다. 위 그림의 경우, 데이터를 송신을 위한 과정을 나타낸 것이고, 각 layer에서 송신에 필요한 데이터를 추가하며 데이터를 encoding하게 된다. 수신받는 컴퓨터나 라우터에서는 위 과정을 반대로 decoding하는 과정을 수행하게 되고 encoding된 데이터를 각 layer에서 양파 껍질을 까듯 decoding 하며 확인한다.

- Protocol Data Unit(PDU)란 각 계층의 데이터 단위를 말함. 
  - 어플리케이션 계층 -> 메세지
  - 전송 계층 -> 세그먼트(TCP), 데이터그램(UDP)
  - 인터넷 계층 -> 패킷(세그먼트에 SP(송신자의 32비트 IP주소), DP(수신자의 32비트 IP주소)가 붙은 데이터 조각)
  - 링크 계층 -> 프레임(맥주소가 붙은 조각)
  - 물리 계층 -> 비트

- 순환 중복 검사 (CRC,cyclic redundancy check)
  - 오류 검증을 통해 데이터의 무결성을 지키기 위함.
  - TCP/IP, MAC 등의 계층에서 오류검증으로 사용


## 2. 각 layer에 대한 설명

### ✒︎ 1계층: Physical Layer
- 0과 1의 나열을 전선으로 전송 할수 있는 형태인 아날로그 신호로 encoding 하거나, 신호를 0과 1로 decoding 하는 시스템 
- PHY칩에 구현되어 있다. 즉 1계층 모듈은 물리적 회로이다.
- 라우터를 통해 서로다른 네트워크를 연결하고, 스위치를 통해 정보의 목적지를 확인하여 정보를 특정 컴퓨터에 똑똑하게 전달한다.  
![network layer](/img/1계층공유기그림.jpeg)  
라우터를 통해 서로다른 네트워크를 연결하고, 스위치를 통해 동일한 네트워크상의 서로다른 컴퓨터에 데이터를 똑똑하게 전달한다. 두개 기능을 합친것을 공유기라 한다. 이런식으로 전세계 모든 컴퓨터를 연결한것이 인터넷이라 하고, 국가간 연결은 해저 케이블을 통해 연결 한다.


### ✒︎ 2계층: Data Link Layer
직접 연결된 서로 다른 2개의 네트워킹 장치 간의 데이터 전송을 담당하는 계층이며, MAC(Media Access Control) 주소를 사용한다. MAC주소는 컴퓨터의 물리적인 주소이고, 3계층의 IP주소로 통신한다는것은 MAC주소간 통신들로 구성된것이다. 택배로 따지면 보내는 주소, 받는 주소를 표현할 때 IP주소를 사용하고, 유통과정에서 중간중간 들르는 주소를 MAC주소라고 생각하면 된다. 2계층에는 이러한 데이터 통신을 잘 할수 있도록 하기위해 Framing, Flow Control, Error Control등의 작업을 수행한다.  
패킷이 

### ✒︎ 3계층: Network Layer
IP, ICMP, ARP가 대표적. 데이터 송신 측면에서, 윗 레이어에서 내려온 데이터에 IP 주소를 추가해 '패킷'으로 만든다. 
![network layer](/img/네트워크3계층그림.jpeg)  
테코톡의 히히님 그림을 참고한 그림이고, 데이터 송신 컴퓨터에서 IP주소를 포함한 패킷을 만들어 하위 계층을 통과해 라우터에 데이터를 전달하고, 라우터에서 라우팅(한 네트워크 안에서 데이터를 최대한 빠르게 보낼 수 있는 최적의 경로를 선택하는 과정)을 통해 다음으로 보낼 장소를 결정한 후 다시 패킷을 만들고, 하위 계층을 통과시켜 데이터를 전달한다.


### ✒︎ 4계층: Transport Layer
포트번호를 사용하여 도착지 컴퓨터의 최종 목적지인 '프로세스'까지 데이터가 "오류없이", "순서대로" 도달 할수 있도록 도와주는 시스템. 여기서 포트번호란 하나의 컴퓨터에 동시에 실행되는 프로세스들이 서로 겹치지 않게 가져야 하는 정수값을 말한다. 목적지 IP까지 왔어도 동시에 실행되는 프로세스중 어디로 들어가야 하는지 알아야 통신이 정상적으로 수행되는 것이고, 때문에 송신 컴퓨터에서 수신 컴퓨터의 프로세스의 포트번호를 알고 지정 해 주어야 한다.

서로다른 호스트(호스트는 IP를 가지고 있고 양방향 통신이 가능한 컴퓨터를 말함)들은 Transport Layer를 통해  msg를 segment 단위로 쪼개진 상태로 주고 받는다. 즉 송신부에서는 메시지를 쪼개어 주고, 수신부에서는 segment들을 조립해 메세지를 수신한다. 이렇게 주고 받기 위해 사용되는 프로토콜이 TCP와 UDP 프로토콜이다.  




### ✒︎ 5계층: Application layer
HTTP, SMTP, SSH, FTP가 대표적, 서비스를 실직적으로 사용자에게 제공하는 계층.  
네트워크 레이어 구조에서 가장 상단의 레이어이고, 어플리케이션 개발에 필요한 여러 부분들을 지원한다. 어플리케이션에는 웹이나 이메일, P2P 서비스 등 여러 서비스들이 있고, 이런 서비스들이 통신을 하기위해 필요한 여러 모듈들이 존재한다. 그중 웹 통신에 사용되는 HTTP 프로토콜도 있고, 5계층과 4계층 사이에 존재하는 Socket도 있다. 소켓을 통해 개발자는 운영체제에 의해 제어되는 Transport layer를 간접적으로 제어할 수 있게 되는데, 네트워크 4계층 이하 구조를 몰라도 소켓을 통해 편하게 통신할 수 있다.  





## ✒︎ 참고 자료 출처 
https://microchipdeveloper.com/tcpip:tcp-ip-five-layer-model
https://www.youtube.com/watch?v=1pfTxp25MA8
https://yohanpro.com/posts/%EB%9D%BC%EC%9A%B0%ED%84%B0%EC%9D%98%20%EA%B5%AC%EC%A1%B0/
https://velog.io/@pistachio02/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%A7%81%ED%81%AC-%EA%B3%84%EC%B8%B5-Data-Link-Layer
https://ddongwon.tistory.com/71

