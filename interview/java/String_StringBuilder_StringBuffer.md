# String vs StringBuilder vs StringBuffer

- 문자열을 다루는 데 사용되는 세 가지 주요 클래스이다.

## 비교

| 분류   | String       | StringBuilder                      | StringBuffer           |
| ------ | ------------ | ---------------------------------- | ---------------------- |
| 불변성 | Immutable    | Mutable                            | Mutable                |
| 동기화 |              | Synchronized 가능 (Thread-safe)    | Synchronized 불가능    |
| 활용   | 연산↓, 조회↑ | 연산↑, 단일스레드 혹은 스레드 무관 | 연산↑, 멀티스레드 환경 |

### 활용

- String : 문자열 연산이 적고 조회가 많은 경우
- StringBuffer : 문자열 연산이 많은 Multi-Thread 환경
- StringBuilder : 문자열 연산이 많은 Single-Thread 또는 Thread 신경 안쓰는 환경

## String

### 특징

- Immutable하다.
- new 연산을 통해 생성된 인스턴스의 메모리 공간도 변하지 않는다.
- `+` 같은 concat 연산 시 원본을 변경하지 않고 새로운 String 객체를 생성한다.
- 문자열의 빈번한 수정이 필요한 상황에서는 메모리 공간의 낭비가 발생하고 성능이 떨어진다.
- 그래서 문자열 연산이 적고, 조회가 많은 상황에서 쓰기 좋다.
- JDK 1.5 이후부터는 `+연산` 사용시 컴파일러가 StringBuilder로 자동 변경한다.

### 불변성의 장점

- String 객체가 공유되거나 재사용될 때 안전성을 보장하고, 캐싱을 유리하게 한다.
- 해시코드의 캐싱에도 이용된다.
- 멀티 쓰레드 환경에서 동기화를 신경쓰지 않아도 된다.

## StringBuffer, StringBuilder

### 공통점

- String과 다르게 Mutable하다.
- new 연산으로 클래스를 한 번만 만들고, 만든 객체를 이용해 연산하고 크기를 변경시켜 문자열을 변경한다.
- 문자열 연산이 자주 발생하는 상황에서 성능적으로 유리하다.

### 차이점

- 동기화 측면에서 차이가 난다.
  - StringBuffer는 Thread-Safe
  - StringBuilder는 Thread-safe하지 않다.
- 구조가 매우 유사하나 스레드 안전성과 성능 사이의 트레이드오프를 기반으로 차이가 생긴다.
  - 성능 StringBuilder > StringBuffer
  - 스레드 안전성 StringBuilder < StringBuffer

## 참고

- https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Java/%5BJava%5D%20String%2CStringBuilder%2CStringBuffer%20%EC%B0%A8%EC%9D%B4.md
- https://gyoogle.dev/blog/computer-language/Java/String%20&%20StringBuilder%20&%20StringBuffer.html