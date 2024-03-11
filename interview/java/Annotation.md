# Annotation

## 의미

- 어노테이션이란 본래 주석이란 뜻으로, 인터페이스를 기반으로 한 문법이다.
- 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다.
- 컴파일러 체크, 코드 분석, 런타임 처리 등에 활용된다.

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

## 참고

- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Java#annotation
- https://asfirstalways.tistory.com/309