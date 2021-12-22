# 1. URI(Uniform Resource Identifier)
## 1.1. URI, URN, URL 구분
- URI에 나머지 둘이 포함된다.
- identifier, name, locator
    - identifier 는 다른 항목과 구분하는데 필요한 정보 (ex. 주민번호)
- URN은 변하지 않음. 이름으로 찾는 방법이 실제로 보편화 x
    - 보통 URI, URL이 큰 차이가 없음
## 1.2. scheme
- scheme: //[userInfo@]host[:port][/path][?query][#fragment]
    - userinfo : url에 사용자 정보 포함, 거의 사용
    - host : 호스트명 ; 도메인명|IP주소 직접 입력
    - 포트 번호 : 생략 가능
    - path : 리소스 경로, 계층적 구조
    - query : key=value, query string 혹은 query parameter라고 불림 모두 문자로 넘어간다.
        - ex)구글에서 q : 검색, hl : 언어
    - fragment : html 내부 북마크 등에 사용, 서버 전송되는 정보x  
- 주로 프로토콜 사용(ex. http, https, ftp 등)

>https에 대해 찾아보기, 포트 번호 생략?
***

# 2. 웹 브라우저의 요청 흐름
## 2.1. 웹 브라우저에 요청
    1. url 요청
    2. DNS 조회
    3. IP와 PORT정보 찾아냄
    4. HTTP 요청 메세지 생성
        • method, path, query, http version, host:www.google...
## 2.2. HTTP 메세지 전송
    1. 웹 브라우저가 HTTP 메세지 생성
    2. SOCKET 라이브러리를 통해 전달
        - TCP/IP 연결(3way) 먼저 진행, 데이터 전달
    3. TCP/IP 패킷 생성, HTTP 메세지 포함
    4. 인터넷을 통해 서버로 전달
## 2.3. 서버의 응답
    1. 서버에서 TCP/IP 포장지 버리고, http 메세지 해석
    2. http 응답 메세지 생성
        • http version, status, Content-Type : 응답하는 데이터 형식, Content-Length : html데이터 길이
    3. 웹브라우저에게 응답 메세지 전달, 웹 브라우저 : html 데이터 랜더링 