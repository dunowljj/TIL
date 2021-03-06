Chapter 1
======================

# 1. 인텔리제이로 스프링 부트 시작하기
## 1.1. 인텔리제이 장단점
### 1.1.1. 인텔리제이 장점
    1. 강력한 추천 기능
    2. 다양한 리팩토링과 디버깅 기능
    3. 이클립스의 깃에 비해 높은 자유도
    4. 프로젝트 시작할 때 인덱싱을 하여 파일을 비롯한 자원들에 대한 빠른 검색 속도
    5. HTML, CSS, JS, XML 에 대한 강력한 기능 지원
    6. 자바, 스프링 부트 버전업에 맞춘 빠른 업데이트

- Community는 HTML&CSS&Javascript 지원 x
- 학생 라이센스 덕분에 Ultimate를 다운해서 사용했다.
>JetBrains 제품 비교 : IntelliJ IDEA Ultimate vs IntelliJ IDEA Community  
>https://www.jetbrains.com/ko-kr/products/compare/?product=idea&product=idea-ce

### 1.1.2. Quora의 다양한 의견들
책에서 추천 한 Quora의 의견들을 일부 읽어보았습니다. 내용의 부분을 요약하자면,
- 리팩토링, 디버깅, 구문 체크 및 검사, 추천(Suggestions), 콘텐츠 지원 등 여러 방면에서 인텔리제이의 성능이 압도적인건 사실이다.
- non-profit 측면에서 이클립스는 강력한 경제성과 통용성을 지니며 큰 생테계를 유지하고 있다 

>Which is better for Java development: Eclipse or IntelliJ IDEA?  
>https://www.quora.com/Which-is-better-for-Java-development-Eclipse-or-IntelliJ-IDEA
***

## 1.2. 프로젝트 생성
- gradle - java 로 생성
- ArtifactId : 프로젝트의 이름
- GroupId 설정
***

## 1.3. 그레이들 프로젝트를 스프링 부트 프로젝트로 변경하기
    1. build.gradle 코드 직접 편집 
        -> 스프링 이니셜라이저를 통해 진행하면 코드의 역할을 도외시하게 될 수 있기 때문에 직접 코드를 추가하라고 적어주셨다.
    2. 설정 변경 반영, Auto-import 설정해도 됨
    3. 라이브러리 반영 확인

- build script : 프로젝트의 플러그인 의존성 관리를 위한 설정
- ext : 전역변수 설정하겠다는 의미
- 의존성을 관리해주는 플러그인 필수적으로 추가
+ repositories : 각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을 지 정함. mavenCentral, jcenter(deprecated)
+ dependencies : 프로젝트 개발에 필요한 의존성들을 선언  
* 그레이들의 버전 변경으로 compile, testCompile 메서드 사용 불가, 아래 코드로 변경

<pre>
<code>implementation('org.springframework.boot:spring-boot-starter-web')
testImplementation('org.springframework.boot:spring-boot-starter-test')</code>
</pre>
***

## 1.4. 인텔리제이에서 깃과 깃허브 사용하기
    1. (맥) Command+Shift+A -> share project on github
    2. idea 폴더 제외하고 선택 커밋
    3. Action 검색창 - plugins - Marketplace 탭 - ignore 설치
    4. idea, gradle ignore
        
- Command + K : 커밋 
- Command + Shift + K : 푸쉬
- ignore 파일이 자동으로 생성되길래 그냥 사용했다. 마켓에 검색해도 나오지 않는다.
***
