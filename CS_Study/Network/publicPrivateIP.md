# Public IP, Private IP

## 존재 이유?
- IPv4 주소의 부족함을 해결하기 위함

## Network Address Translation (NAT)
- 패킷의 IP를 다른 IP주소로 맵핑하는 장치
- 외부 인터넷과 통신할때는 public IP로 통신하고, 내부끼리 통신할때는 변환된 private IP로써 통신한다. 내부에서 외부로 통신할때는 NAT장치를 통해서 private IP를 public IP로 바꾸어 통신함