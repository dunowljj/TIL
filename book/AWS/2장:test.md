# 2. 스프링 부트에서 테스트 코드 작성하기
- 항상 갈망하던 TDD 관련 내용이라 기뻐하며 읽었다. 
- 국비 프로젝트를 진행하면서, 간단한 내용이기도 하고(핑계), 애초에 테스트에 대해 자세히 배우지 못해서 따로 공부할 시간도 없었다(핑계2). 
- 한창 기능을 구현하다가 막히면 테스트를 진행했던 기억이 있다. 이참에 기초를 잘 다질 생각이다.
+ 서비스 회사에서 단위 테스트는 필수조건

>lombok 기능을 구현하면서 참조 교재와 그레이들 버전이 달라 계속해서 코드를 찾아서 변경해야하는 불상사가 발생했다.
>찾아보는 것도 공부라는 생각은 들었지만, 흐름과 개념을 먼저 잡는게 더 효율적이란 생각도 들었고, 이동욱님의 오류수정 답변에도 그레이들 4로 먼저 연습하는 것을 권장한다는 메세지를 보았다.
>   >그레이들 설정법 도움을 받은 블로그 (https://namsick96.github.io/build%20tool/Gradle_version_change_at_Intellij/)

## 2.1. 테스트 코드 소개
### 2.1.1. 레드 그린 사이클
- 통과한 코드를 작성, 리팩토링

### 2.1.2. 단위 테스트 코드 이점(위키피디아)
- 개발단계 초기에 문제 발견
- 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인 가능(ex. 회귀테스트)
- 기능의 불확실성 감소
- 시스템에 대한 실제 문서 제공, 자체가 문서로 사용 가능
>'TDD 실천법과 도구' 공개 PDF
>(http://repo.yona.io/doortts/blog/issue/1)

### 2.1.3. 단위 테스트 코드의 이점
- 톰캣이랑 씨름 안해도 됨
- sysout, log? -> 눈 대신 자동검증
- 특히 규모가 클수록 기존 기능 검증 및 보호
    - 하나의 기능을 추가할 때마다 너무 많은 자원 소모, 서비스의 모든 기능을 테스트 불가능

### 2.1.4. xUnit?
- 대표적 테스트 프레임워크
- 개발환경(x)에 따라 Unit테스트를 도움
- JUnit -Java, DBUnit - DB, CppUnit - C++, NUnit - .net
- JUnit5 -> 책에서는 4 사용, 그 당시 주로 4를 사용함
***

## 2.2. HelloController 테스트 코드 작성
-  No tests found for given includes 오류
    - Setting - Build Tools - Gradle - Run tests using : InteliJ IDEA로 변경
- MockMVC 사용 
    - 웹 api 테스트
    - 스프링 mvc 테스트의 시작점
    - mvc.perform(get("/hello"))  
    .andExpect(status().isOK  
    .andExpect(content().string(hello))
***

## 2.3. 롬복 소개 및 설치하기
- 자바 개발자들의 필수 라이브러리
- compile('org.projectlombok:lombok')
- 어노테이션만으로 getter, setter, toString, 기본생성자 생성
- Enable annotation processing 체크
***

## 2.4. Hello Controller 코드를 롬복으로 전환하기
### 2.4.1. Dto 생성
- 테스트 코드가 우리를 지켜주기 때문에 맘놓고 전환가능
- @Getter, 
- @RequiredArgsConstructor 
    - 선언한 모든 final 필드가 포함된 생성자만 생성
### 2.4.2. HelloControllerTest 수정1 : lombok, assertj
- assertThat
    - 테스트 검증 라이브러리의 검증 메소드
    - 검증하고 싶은 대상을 메소드 인자로
    - 메소드 체이닝 지원
- isEquslTo
    - 값이 같을 때만 성공
>백기선님 유투브 assertJ가 JUnit의 assertThat보다 편리한 이유
>(http://bit.ly/30vm9Lg)

### 2.4.3. HelloController 수정
- HelloController 매핑 수정
- @RequestParam

### 2.4.4. HelloControllerTest 수정2
- @param
    - 테스트할 요청 파라미터 설정 =
    - String만 허용, 숫자/날짜 등의 데이터 문자열로 변경해야함(ex.Sring.valueOf(amount))
- jsonPath
    - JSON 응답값을 필드별로 검증
    - $ 기준으로 필드명 명시 $.변수명
***

#### perform, get, param, andExpect / status,jsonPath