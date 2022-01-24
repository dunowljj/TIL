챕터 8 예외처리
=====
# 1. 예외클래스
- 에러와 예외 차이
- 모든 예외 클래스는 Exception 클래스 자손
- RuntimeException 클래스들: RuntimeExcpetion 클래스와 그 자손 클래스들
    - 사용자 실수 등 외적인 요인에 의한 예외(외부의 영향)
    - checked 예외 : 해당 클래스와 자손들이 발생할 가능성 있음
- Exception 클래스들: RuntimeExcpetion클래스들을 제외한 나머지
    - 프로그래머의 실수로 발생하는 예외 -> 걸리면 컴파일도 안됨
    - unchecked예외
***

# 2. try-catch문
- catch 여러 개 있어도 일치하는 하나만 수행
- catch 여러 개로 예외별 처리도 가능
## 2.1. 처리 실패?
- 발생한 예외 처리 못할 시 -> 비정상적 종료, 처리 못한 예외는 JVM의 예외처리기가 받아서 원인 출력
## 2.2. 흐름
    1. 예외 발생
    2. 해당하는 예외 클래스의 인스턴스 생성
    3. 처리할 수 있는 catch블럭 탐색  
    -> instanceof 연산자를 통해 검사해서 true 만날때까지 검사(Exception 에는 다 걸림)
    4. 찾으면 catch문 못찾으면 예외처리x
## 2.3. 정보 얻기
- printStackTrace(): 예외발생 당시에 호출스택에 있었던 메서드의 정보와 예외 메세지 화면에 출력
- getMessage(): 발생한 예외클래스의 인스턴스에 저장된 메세지 얻을 수 있음
## 2.4. 멀티 catch 블럭
- 하나의 catch 안에 | 사용해서 연결
- 중복 코드 줄일 수 있음
- **상속관계에 있는 예외클래스를 같이 쓰면 컴파일 에러** -> 조상하나 쓰는거랑 같음; 이미 대안적인 예외에 잡혔다고 나온다.
- 연결 개수 제한 없음
## 2.5. try-catch 리턴처리
- try -> return 값, 객체, String ; 값 복사
>[코데방님 try-catch문에서 리턴처리](https://codevang.tistory.com/211)
***
## 2.6. finally
- 발생여부 무관 실행
***

# 3. 예외 발생시키기
```
Exception e = new Exception("고의 발생");
throw e;
// = throw new Exception("고의 발생");
```
- 생성자로 메세지 넣을 수 있음
***

# 4. 메서드 예외 선언
- 예외를 인가
- throws Exception, ...
- 여러개 일때 쉼표
- 선언부에 예외를 선언함으로써 사용하려는 사람들이 무슨 예외를 처리해야하는지 쉽게 알 수 있음
- 상속 관계까지 고려해야 한다.
- checked 예외는 try-catch 안해도 컴파일 오류 안남
***
# 5. 사용자 정의 예외(307p)
- Exception 또는 RuntimeException 클래스 상속
- 기존에는 전자, 요즘엔 후자 많이 씀
- 생성자에 String 매개변수로 메시지 저장 가능
***

# 6. 예외 되던지기(exception re-throwing)
- 한 메서드의 여러 예외를 나눠서 처리하기도 함
- 되던지기는 한 메서드의 한 예외를 발생 메서드, 호출한 메서드가 나눠서 처리하도록 하는 것
- 예외 처리 후 인위적으로 발생시킴
- 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용
***
# 7. 연결된 예외(chained exception)
# 7.1. 의미
- 예외가 예외를 발생 시킬 수 있음 -> 발생 원인을 원인 예외(cause exception)이라함
1. 여러 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해 사용
2. checked를 unchecked예외로 바꿀 수 있도록 함  
# 7.2. 사용
## 7.2.1. 원인예외 만들기
```
try {
    startInstall();
    copyFiles();
} catch (SpaceException e){
    InstallException ie = new InstallException("설치중 예외발생");
    ie.initCause(e);
    throw ie;
} catch (MemoryException me){
...
```
- initCause()로 SpaceException을 원인 예외로 설정
# 7.2.2. checked -> unchecked
```
if(!enoughMemory())
throw new RuntimeException(new MemoryException("error"));
```
- Exception 자손인 MemoryException 감싸면서 unchecked로 변경
## 7.2.3. initCause()
- initCause()는 Exception의 조상인 Throwable클래스에 정의되어 있음
- Throwable initCause() 원인 예외로 설정
- Throwable getCause() 원인 예외를 반환
***

# 8. 오버라이딩과 throws
- 상위 클래스에 throws 선언 시 하위클래스는
    - 상위에서 예외 여러개 던졌으면 선택적으로 던지거나 안던져도 됨
    - 상위에서 던진 예외를 상속하는 클래스들 던질 수 있음
    - RuntimeException류는 그냥 던질수 있음
- 요즘 api에서 처리 많이함
***
# 9. try-with-resources
>dololak님 블로그  
https://dololak.tistory.com/67?category=636500

추가
====
- 8_8다시 보기