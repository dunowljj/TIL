# Annotation

## 의미

- 어노테이션이란 본래 주석이란 뜻으로, 인터페이스를 기반으로 한 문법이다.
- 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다.
- 컴파일러 체크, 코드 분석, 런타임 처리 등에 활용된다.

## 특징

- 인터페이스 기반이다.
  - 에노테이션의 속성 값은 사실 추상메서드의 반환 값이다. 요소는 메서드처럼 동작한다.
  - 상속 불가
  - 런타임에 프록시로 읽힘
  - 리플렉션 처리
- 런타임에 프록시로 생성된다.
  - JVM이 “애너테이션 요소 풀(JSON 같은 metadata)”을 읽어서
    InvocationHandler + Proxy 클래스로 구현체를 즉석에서 만든다.
  - 인터페이스는 인스턴스 생성이 불가하다. 메서드 호출을 해야 읽을 수 있기 때문에 프록시가 필요하다.

## 종류

어노테이션에는 크게 세 가지 종류가 존재한다.

### built-in annotation

- JDK 에 내장되어 있다.
- ex) @Override, @Deprecated, @SupressWarning, @FunctionalInterface 등

### Meta annotation

- 어노테이션에 대한 정보를 나타내기 위한 어노테이션
- ex) @Target, @Retention, @Documented, @Inherited, @Naitve

### Custom Annotation

- 개발자가 직접 만드는 어노테이션
- `public @interface 이름 {}`형식으로 만들 수 있다.

## 주요 기능

- 컴파일러 체크 (@Override, @Deprecated)
- 런타임 동작 제어 (Spring DI, AOP, JPA 등)
- 코드 생성 및 자동화 (Lombok, MapStruct)
- 문서·테스트 도구용 메타데이터 제공 (Swagger, JUnit)
- 설정 파일(XML 등)을 대체하는 역할

## 참고

- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Java#annotation
- https://asfirstalways.tistory.com/309
