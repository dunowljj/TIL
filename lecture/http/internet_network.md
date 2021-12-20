1\. IP
===================
## 1.1 IP(Internet Protocol)의 문제점
    1. 서버가 꺼져있어도 모른채 전송
    2. 중간에 데이터 소실(노드가 갑자기 꺼지는 등)되어도 모름
    3. 순서가 보장되지 않음
    4. 다양한 어플리케이션을 한 pc에서 사용할 때 패킷을 어떻게 구분?
## 1.2 프로토콜 계층
    1. 프로그램이 메세지 생성
    2. Socket 라이브러리를 통해 전달
    3. 메세지 데이터를 포함한 TCP 정보 생성
    4. IP 패킷 생성, TCP 포함
    5. LAN 카드에서 Ethernet frame 덧씌움
***
\
2\. TCP, UDP
===================
## 2.1 TCP(Transmission Control Protocol)의 역할
    1. 연결지향 - 3 way handshake
        - (c->s)SYN - (s->c)SYN,ACK - (c->s)ACK
        - SYN(Synchronize Sequence Numbers) -> 접속 요청
        - ACK(Acknowledge) -> 알겠다
    2. 순서 보장
        - 순서 틀리면 다시 요청
        - cf) driver
    3. 데이터 전달 보증
        - 응답으로 확인
    4. port 정보
## 2.2 UDP(User Datagram Protocol)
    1. IP와 유사, 체크섬 정도 추가
    2. 데이터 전달 및 순서 보장x, 단순하고 빠름
    3. 최근 조명받음. HTML의 빠른 로드
    4. 어플리케이션 간의 구분
***
\
3\. PORT, DNS
===================
## 3.1 PORT
    ▪︎ 같은 IP 내에서 프로세스 구분
        - 0~65535 : 할당 가능
        - 0~1023 : 잘 알려진 포트, 사용x 권장
        - FTP - 20, 21
        - TELNET - 23
        - HTTP - 80
        - HTTPS - 443
## 3.2 DNS(Domain Name System)
### 3.2.1 IP의 문제점
    1. 길고 외우기 힘듦
    2. 주소가 변경될 가능성
### 3.2.2 DNS
    1. 도메인 명을 DNS서버에 등록해 놓음(ex. google.com)
    2. 주소가 바뀌어도 상관 없음
***