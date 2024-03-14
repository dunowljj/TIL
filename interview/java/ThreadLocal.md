# ThreadLocal

## ThreadLocal이란?

- 각 스레드가 변수의 자신만의 독립적인 인스턴스를 가질 수 있게 해준다.
- 각 스레드가 자신만의 데이터 복사본을 가지므로 각 스레드 사이에 간섭이 없다. 멀티스레드 환경에서 유용하다.

### 특징

- 스레드 풀 환경에서 ThreadLocal 을 사용하는 경우 ThreadLocal 변수에 보관된 데이터의 사용이 끝나면 반드시 해당 데이터를 삭제해 주어야 한다.
  - 쓰레드가 재사용될때 올바르지 않은 데이터를 참조할 수 있다.
  - 스레드 풀의 특성상 ThreadLocal의 객체가 계속 메모리에 남아 있어 메모리 누수가 발생할 수 있다.

### 사용

```java
private static final ThreadLocal<SimpleDateFormat> dateFormatThreadLocal = new ThreadLocal<>();

dateFormatThreadLocal.set(new SimpleDateFormat("yyyy-MM-dd"));
SimpleDateFormat dateFormat = dateFormatThreadLocal.get();
dateFormatThreadLocal.remove();
```

1. ThreadLocal 객체를 생성한다. 보통 static으로 선언하여 모든 인스턴스가 공유한다.
2. ThreadLocal.set() 메서드를 이용해서 변수에 값을 저장한다.
3. ThreadLocal.get() 메서드를 이용해서 변수 값을 읽어온다.
4. ThreadLocal.remove() 메서드를 이용해서 변수 값을 제거한다.

### 활용

- 쓰레드 안전해야하는 데이터 보관
- Spring security에서 사용자 정보 전파
- 엔티티 매니저의 트랜잭션 컨텍스트 전파

## 참고

- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Java#threadlocal
- https://javacan.tistory.com/entry/ThreadLocalUsage
