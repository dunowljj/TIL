네트워킹
=============
# 1. 네트워킹
## 1.1. 네트워킹이란?
- 두 대 이상의 컴퓨터를 케이블로 연결하여 네트어크를 구성하는 것
- 데이터 주고받기 혹은 자원 공유하기 위한 노력
- 셀 수 없는 컴퓨터가 인터넷이라는 거대한 네트워크 구성
- 자바에서 java.net 제공
## 1.2. 클라이언트와 서버
- 서버 : 서비스를 제공하는 컴퓨터, 소프트웨어가 실행되는 컴퓨터
- 클라이언트 : 서비스를 이용하는 컴퓨터
- 서버 프로그램과 그것과 연결가능한 클라이언트 프로그램이 있어야 한다
    - 웹브라우저 - 웹 서버
    - 알FTP - FTP서버 접속해서 파일 받기
- 서버기반모델(server-based model) : 네트워크를 구성할 때 전용서버를 두는 것
- P2P모델(peer-to-peer) : 별도의 전용서버없이 각 클라이언트가 서버역할을 동시에 수행하는 것
***

# 2. IP주소
## 2.1. IP
- 컴퓨터(호스트)를 구별하는데 사용되는 고유한 값
- 인터넷에 연결된 모든 컴퓨터는 IP주소를 가짐
- 4byte의 정수로 구성
- IP주소에서 호스트주소와 네트워크주소로 나뉨
    - 네트워크의 구성에 따라 두 주소가 몇 bit를 차지하는지 다름
    - 서로 다른 두 호스트의 IP주소의 네트워크 주소가 같다는 것은 같은 네트워크 포함되어 있다는 의미
- 윈도우에서는 ipconfig.exe 실행 시 IP주소와 서브넷마스크 확인 가능
## 2.2. 네트워크 주소와 호스트 주소
- IP주소와 서브넷 마스크를 비트연산자 '&'로 연산하면 IP주소에서 네트워크 주소만을 뽑아낼 수 있다.
- IP주소에서 네트워크주소가 차지하는 자리수가 많을수록 호스트 주소의 범위가 줄어들기 때문에 네트워크의 규모가 작아진다. 
    - 호스트 자리수가 8개라면 256개의 호스트만 이 네트워크에 포함 가능
## 2.3. InetAddress 클래스
- IP주소를 다루기 위한 클래스
- 680p 메서드
### InetAddress 클래스 예제
***

# 3. URL
## 3.1. 설명
- 인터넷에 존재하는 여러 서버들이 제공하는 자원에 접근할 수 있는 주소를 표현하기 위한 것
- `프로토콜://호스트명:포트번호/경로명/파일명?쿼리스트링#참조` 형태
## 3.2. URL클래스
- URL을 다루기 위한 클래스
- 683p 메서드
    - URL(String spec)/(String protocol, String host, Sting file)/(String protocol, String host, int port, String file)
    - URLConnection openConnection()/(Proxy proxy) : URL과 연결된 URLConnection을 얻는다
    - InputStream openStream()
    - set,toExternalForm,toURI...
## 3.3. URLConnection클래스
### 3.3.1. 설명
- 어플리케이션과 URL간의 통신연결을 나타내는 클래스의 최상위 클래스로 추상클래스
- URLConnection을 사용해서 연결하조가하는 자원에 접근하고 읽고 쓰기를 할 수 있다.
- HttpURLConnection과 JarURLConnection은 URLConnection을 상속받아 구현
- URL의 프로토콜이 http라면 opneConnection()은 HttpURLConenction을 반환한다.
### 3.3.2. 메서드
- 685~686p
- content, 캐시관련 메서드도 있음
### 예제 2
- URL유효하지 않으면 Malformed-URLException
- openStream()과 InputStream 활용. 파일로 데이터 읽는 것과 같음
```
InputStream in = url.openStream();

URLConnection conn = url.openConnection();
InputStream in = conn.getInputStream();
```
### 예제 3
- 이진 데이터를 읽어 파일에 저장하는 예제
***

# 4. 소켓 프로그래밍
## 4.1. 소켓
- 프로세스간의 통신에 사용되는 양쪽 끝단(endpoint)
- 프로세스 통신을 위해 필요
- java.net에서 소켓 프로그래밍 지원
    - 프로토콜에 따라 다른 종류의 소켓 구현하여 제공함
## 4.2. TCP와 UDP 설명
- TCP/IP 프로토콜
    - 이기종 시스템간의 통신을 위한 표준 프로토콜
    - 프로토콜의 집합
    - TCP, UDP 모두 여기에 속함
    - 전송계층
- TCP vs UDP
    - 연결 : 연결 기반(1:1) vs 비연결기반(1:1,1:n,n:n)
    - 데이터 경계 : 구분x(byte-stream) vs 구분(datagram)
    - 신뢰성 : o vs x
    - 속도 : UDP가 더 빠름
    - 사용 : 파일 전송 vs 게임(좀 끊겨도 빨라야함, 순서 바뀐거 무시하면 됨)
    - TCP : 순서보장, 수신여부 확인, 패킷 관리할 피룡 없음

## 4.3. TCP소켓 프로그래밍 (692p)
    1. 서버 프로그램이 서버소켓으로 특정 포트에서 연결요청 처리 준비
    2. 접속할 서버의 IP정보, 포트 정보를 가지고 소켓을 생성해서 서버에 연결 요청
    3. 서버소켓은 요청을 받고 새로운 소켓을 생성해서 클라이언트의 소켓과 연결되도록 함
    4. 클라이언트의 소켓과 생성된 서버의 소켓 일대일 통신
- 서버 소켓은 포트와 결합되어 포트를 통해 원격 사용자의 연결요청을 기다리고, 요청이 올때마다 소켓을 생성하여 상대편 소켓과 통신할 수 있도록 연결
- 실제적인 데이터 통신은 서버소켓 관계없이; 소켓 - 소켓
- 여러개의 소켓이 하나의 포트 공유 가능 / 서버소켓은 포트 독점
- 보통 1023이하는 FTP나 Telnet 같은 기존 다른 통신 프로그램들이 사용 -> 1023이상에서 고르기
## 4.4. Socket과 ServerSocket 클래스
### 4.4.1. 내용 재정리
- 서버소켓: 소켓간의 연결만
- 실제 데이터는 소켓끼리, 주고받는 연결통로는 입출력스트림
-   소켓끼리 입출력스트림이 교차연결됨
### 4.4.2. 클래스
- Socket클래스
    - 프로세스간의 통신을 담당
    - InputStream, OutputStream 가짐 -> 프로레스간의 통신
- ServerSocket클래스
    - 포트와 연결되어 외부의 연결요청을 기다리다 연결요청이 들어오면, Socket을 생성해서 소켓과 소켓간의 통신이 이루어지도록 한다. 한 포트에 하나의 ServerSocket만 연결할 수 있다.(프로토콜 다르면 같은 포트 공유 가능)
### 예제
- 클라이언트 프로그램의 요청을 지속적으로 처리하기 위해 무한 반복문 사용
```
    while(true){
        try{
            ...
            Socket socket = serverSocket.accept();
            ...
        }
    }
```
    예제1 - 서버 소켓을 만들어 포트에 바인드 시켜놓고, accept()로 요청을 받아들여 getOutputStream, DataOutputStream 이용하여 데이터 전송  

    예제2 - 소켓을 생성하고(ip,포트) 해당 포트에 요청을 보내고 getInputStream, DataInputStream활용하여 데이터 출력
## 4.4. UDP소켓 프로그래밍
- DatagramSocket과 DatagramPacket사용
- DatagramPacket : 헤더와 데이터로 구성
    - 헤더 : DatagramPacket을 수신할 호스트의 정보(호스트의 주소와 포트) 저장
### 예제
- 클라이언트에서 DatagramPacket 생성해서 DatagramSocket으로 서버에 전송, DatagramPacket을 받아(receive) 정보 얻어서 서버시간을 DatagramPacket에 담아서 전송(send)