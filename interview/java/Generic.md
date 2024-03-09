# Generic

- 클래스, 인터페이스, 메서드를 정의할 때 타입(Type)을 파라미터로 사용할 수 있게 해준다.
- 컴파일 시점에 타입 체크를 수행한다.
- 참타입 소거(type erasure) 원리에 의해 구현된다. 컴파일 시점에 제네릭 타입 정보는 제거되므로 주의가 필요하다.

### 효과

- 타입 안전성(Type Safety)을 강화한다.
- 타입 캐스팅(Casting)을 줄여 코드의 재사용성을 높인다.
- 같은 코드를 다양한 타입에 대해 사용할 수 있게 함으로써, 코드의 재사용성이 향상된다.

### 사용

- 제네릭 클래스: 타입 파라미터를 사용하여 정의된 클래스
- 제네릭 인터페이스: 타입 파라미터를 사용하여 정의된 인터페이스입니다. ex) List<E> 인터페이스
- 제네릭 메서드: 메서드 선언에 타입 파라미터를 포함하는 메서드입니다. 제네릭 메서드는 그 메서드가 속한 클래스의 타입 파라미터와는 독립적일 수 있다.

### 특징

- 컴파일 타임에 타입 체크를 한다.

```java
// 클래스 및 인터페이스
public class MyClass <T> { ... }
public class HashMap <K, V> { ... }
public interface MyInterface1 <T> { ... }
public interface MyInterface2 <T, K> { ... }

// 제네릭 메소드
public <T> T doSomething1(T o) {	... }
public <T> void doSomething2(T o) {	... }
```
