# try-with-resources

### 설명

- 자바 버전7에 도입
- AutoCloseable 인터페이스를 구현하는 리소스(파일, 소켓, 데이터베이스 연결 등)를 사용한 후에 자동으로 닫아주는 기능을 제공한다.
- try() 안에 사용할 리소스 객체를 명시적으로 선언하여 사용하면, try 블록 안에서 로직이 정상 완료 여부와 관계 없이 JVM에서 자동으로 자원을 반납한다.
- 더 간결한 코드를 제공하며, 리소스 관리가 안전해진다.

### 등장 배경, 자바 7이전 사용

```java
FileInputStream in = null;
try {
    in = new FileInputStream("somefile.txt");
    // 파일에서 데이터를 읽는 작업 수행
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (in != null) {
        try {
            in.close(); // finally 블록에서 리소스를 명시적으로 닫음
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

- 자바 7 이전에서 AutoCloseable을 구현한 객체를 사용할 경우 개발자가 임의로 `finally`와 `.close()`를 사용하여 자원 해제를 해야했다.
- 이떄, 실수로 자원 해체를 누락할 가능성이 있으며 누수로 이어진다.
- 자원 해체 중복코드가 생긴다.

### 사용 - Java 7,8

```java
try (FileInputStream in = new FileInputStream("path/to/file")) {
    // 파일 처리
} catch (IOException e) {
    // 예외 처리
}
```

- try() 안에 사용할 리소스 객체를 명시적으로 선언하여 사용한다.
- 자원 해체 누락 가능성이 없어지며, finally와 close 호출 없이 간결함을 유지한다.

### 사용 - Java 9

```java
FileInputStream in = new FileInputStream("path/to/file");
try (in) {
    // 파일 처리
} catch (IOException e) {
    // 예외 처리
}
```
- 자바 9 버전에서는 이미 선언된 이미 선언된 리소스를 try 블록에 포함시킬 수 있게 되었다.

### 등장 배경

## 참고

- https://github.com/ksundong/backend-interview-question
