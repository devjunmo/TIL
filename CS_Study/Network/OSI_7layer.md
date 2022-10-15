# OSI 7 layer

## 1. 네트워크 레이어의 큰 그림

### ✒︎ 네트워크 아키텍쳐 종류들
![network layer](/img/NetworkModel.png)

현재는 가장 오른쪽의 update 버전 TCP/IP 모델을 주로 사용한다. 때문에 정리는 5 layer를 메인으로 할것 

### ✒︎ TCP/IP 5 layer model
![network layer](/img/TCP_IP_5layer.png)

네트워크 시스템은 Layered Architecture를 따르고, 각 레이어를 거치며 data가 encoding 또는 decoding 되어 데이터를 송수신 하게 된다.

위 그림의 경우, 데이터를 송신을 위한 과정을 나타낸 것이고, 각 layer에서 송신에 필요한 데이터를 추가하며 데이터를 encoding하게 된다. 수신받는 컴퓨터나 라우터에서는 위 과정을 반대로 decoding하는 과정을 수행하게 되고 encoding된 데이터를 각 layer에서 양파 껍질을 까듯 decoding 하며 확인한다.

## 2. 각 layer에 대한 설명

### ✒︎ 1계층: Physical Layer
- 0과 1의 나열을 전선으로 전송 할수 있는 형태인 아날로그 신호로 encoding 하거나, 신호를 0과 1로 decoding 하는 시스템 
- PHY칩에 구현되어 있다. 즉 1계층 모듈은 물리적 회로이다.
- 라우터를 통해 서로다른 네트워크를 연결하고, 스위치를 통해 정보의 목적지를 확인하여 정보를 특정 컴퓨터에 똑똑하게 전달한다.  
![network layer](/img/switch_Router.png)  
그림을 보면, 라우터를 통해 두개의 서로다른 네트워크가 연결되고 있고, 동일한 네트워크 안에 스위치가 목적지를 똑똑하게 확인하여 전달 해 주는 모습을 볼수있다.
- 이런식으로 전세계 모든 컴퓨터를 연결한것이 인터넷이라 하고, 국가간 연결은 해저 케이블을 통해 연결 한다.


(계속)



## ✒︎ 참고 자료 출처 
https://microchipdeveloper.com/tcpip:tcp-ip-five-layer-model
https://www.youtube.com/watch?v=1pfTxp25MA8
https://yohanpro.com/posts/%EB%9D%BC%EC%9A%B0%ED%84%B0%EC%9D%98%20%EA%B5%AC%EC%A1%B0/




