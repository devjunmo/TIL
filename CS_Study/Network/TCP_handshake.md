# TCP Handshake

## ✒︎ TCP Handshake 란?

- TCP는 connection-oriented protocol이다.
- 이것은 통신을 맺고 끊는 과정에서 송신자와 수신자간 연결이 확실히 맺어졌는지 확인을 한다는 의미이다.
- 통신을 맺을때 사용하는 방식은 3-way handshake를, 끊을때 사용하는 방식은 4-way handshake를 사용한다.

## ✒︎ 3-way handshake

![TCP_3wayhandshake2](/img/3way_ref.png)
송수신자간 연결을 맺을 때 사용하는 방법. end point간 상태 변화와 9bit flag의 SYN(synchronize sequence numbers), ACK(acknowledgements)를 통해 표현한 그림이다.

1. 클라이언트에서 서버로(서버는 포트 서비스가 가능한 Listen 상태) 접속을 요청하는 SYN flag 1의 패킷을 보낸다. 클라이언트는 SYN을 보내고 난 후 SYN/ACK 응답을 기다리는 SYN_SENT 상태가 된다.
2. 서버는 SYN을 받고 클라이언트에게 SYN + ACK 플래그를 1로 설정한 패킷을 보내고나서 ACK를 기다리는 SYN_RECEIVED 상태가 된다.
3. 클라이언트는 해당 패킷을 받고 서버에게 ACK를 1로 설정한 패킷을 보낸다.

위 과정을 통해 이후 부터는 연결이 이루어 지고 데이터가 오갈 수 있게 된다. 이 상태를 ESTABLISHED 상태라 한다.

## ✒︎ 4-way handshake

![TCP_4wayhandshake](/img/TCP_4wayHandshake2.jpeg)
송수신자간 연결을 끊을 때 사용한느 방법.

1. 클라이언트에서 서버로 데이터를 다 보낸 상황에서 클라이언트에서 서버와의 연결을 끊기 위해 종료를 위한 FIN 패킷을 보내고, FIN_WAIT1 상태가 된다. (서버에서 먼저 연결을 끊을 수도 있음)
2. 서버는 FIN패킷을 받고, 일단 알았다는 뜻으로 ACK 패킷을 보낸다. 서버는 CLOSE_WAIT 상태가 된다. 그러고 나서 서버가 미처 다 보내지 못한 데이터를 마저 보내는 작업을 한다. 이때 클라이언트의 상태는 "TIME_WAIT" 상태이다.
3. 서버의 통신이 끝나고 연결을 끊을 준비가 되면, FIN 패킷을 클라이언트에 보내고, LAST_WAIT 상태가 된다.
4. 클라이언트는 서버에 ACK를 보내고 연결을 종료하며 서버도 ACK을 받고 연결을 종료한다.

## ✒︎ 참고자료

https://sleepyeyes.tistory.com/4  
https://sh-safer.tistory.com/146
