머스테치로 화면 구성하기
===========
# 4.1 서버 템플릿 엔진과 머스테치 소개
## 4.1.1. 템플릿 엔진
- 서버 템플릿 엔진 : 서버
    - JSP,Velocity,Freemarker,Thymeleaf
- 클라이언트 템플릿 엔진 : 브라우저에서 작동
    - 흔히 말하는 Vue.js,React.js 이용한 SPA
    - cf)서버 사이드 랜더링
- JS에서 자바 코드사용(126p)
## 4.1.2. 머스테치란
- 루비, 자바스크립트, 파이썬, PHP, 자바, 펄, Go, ASP 등 대부분의 언어 지원
- 문법이 심플
- 로직 코드 사용할 수 없어 View와 서버 역할 명확하게 분리
- Mustache.js, Mustache.java 2가지 모두 있음. 하나의 문법으로 클라이언트/서버 템플릿 모두 사용 가능
## 4.1.3. 머스테치 설치
- mustache 플러그인 설치
- 의존성 추가 complie('org.springframework.boot:spring-boot starter-mustache')
***

# 4.2. 기본페이지 만들기
## 4.2.1. index(131p)
- src/main/resources/templates/index.mustache
## 4.2.2. IndexController
- 머스테치 스타터가 경로와 확장자 자동 지정 후 View Resolver가 처리
## 4.2.2. IndexControllerTest
- TestRestTemplate
- getForObject
- assertThat(body).contains("")
***
# 4.3. 게시글 등록 화면 만들기
## 4.3.1. 프론트엔드 라이브러리 사용
- 직접 라이브러리 받기
- 외부 CDN
    - 편하게 한 줄 추가
    - 실무에서는 외부 서비스 의존 방지를 위해 잘 사용x
-해당 책은 CDN 사용
## 4.3.2. 레이아웃
- HTML은 위에서부터 실행(head완료 -> body)
- js는 body, css는 head(깨짐방지)
- bootstrap.js는 제이쿼리 꼭 있어야(먼저호출)
## 4.3.2 js 추가
- static/js/app/index.js
    - 브라우저의 스코프는 공용 공간으로 쓰이기 때문에 나중에 로딩된 js가 먼저 js를 덮어씀
    - 중복 함수를 피하기 위해 유효범위(scope)생성
- footer에 추가
***

# 4.4. 전체 조회 화면 만들기
- index.mastache 수정
    - {{#posts}}
        - List 순회
    - {{변수명}}
        - List에서 뽑아낸 객체의 필드 사용
- PostsRepository
    - @Query("")
        - 가독성 좋음
- PostsService
    - (readOnly = true)
        - 트랜잭션 범위 유지, 조회 기능만 남겨서 속도 개선
- 규모가 큰 프로젝트에서는 조회용 프레임워크 사용(147p)
- 148p map,list,람다
***

# 4.5. 게시글 수정, 삭제 화면 만들기
- posts-update 추가
- index.js에 수정/삭제 추가
- ajax PostsApiController 거침
- IndexController에 GET 추가
***

#### 여태 해온게 이런 spring CRUD라 힘들진 않았으나, 생소한 개념들이 많아 추가적인 공부가 필요하며, 테스트 코드 부분을 강하게 복습해야함.
- 람다식, 자료구조, 서블릿 등 실행순서, TestRestTemplate
