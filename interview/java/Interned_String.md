# Interned String

- 문자열 리터럴은 프로그램 전체에서 단 하나의 String 객체로 관리한다.
- 동일한 리터럴에 대한 모든 참조는 상수 풀 내의 같은 String 객체를 가리킨다.

## String의 두 가지 생성 방식

    1. new 연산자를 이용한 방식
    2. 리터럴을 이용한 방식

```java
String s1 = new String("Hello"); // new 사용
String s2 = "Hello"; // 리터럴 사용
```

- new를 통해 String 객체를 생성하면 Heap 영역에 존재하게 된다. 사용시마다 매번 새로운 String 객체가 heap 메모리에 생겨난다.

* 리터럴을 이용할 경우, String Constant Pool에 저장한다.
  - 확인해서 이미 같은 값이 존재하면 해당 값을 불러온다.
  - 없으면 풀에 저장하고 주소를 반환한다. 이때 `intern()`이 사용된다.

## intern() 메서드

```java
String s1 = "Hello"; // 문자열 리터럴. 상수 풀에서 관리
String s2 = "Hello"; // 동일한 리터럴. 상수 풀에서 이미 존재하는 객체의 참조를 반환
String s3 = "Hello"; // new 사용. 사용마다 다른 주소 반환


/**
 * s1 == s2      -> true
 * s1.equals(s2) -> true
 *
 * s2 == s3      -> false
 * s2.equals(s3) -> true
 */
```

String을 **리터럴로 선언**할 경우, 내부적으로 String의 `intern()`이라는 메소드가 호출된다. 참고로 리터럴 방식이 아니어도 직접 호출해서 사용할 수도 있다. `intern()` 호출 시,

- 주어진 문자열이 String Constant Pool에 존재하는지 검색하고 있다면 그 주소값을 반환한다.
- 존재하지 않는다면 String Constant Pool에 넣고 새로운 주소값을 반환한다.

## String Constant Pool

### 설명

- 자바의 힙 메모리 영역 내에 위치한 특별한 저장 공간이다.
- 문자열 리터럴과 String.intern() 메소드를 통해 인턴된 문자열 객체를 유니크하게 관리한다.

### 위치 변경

- 자바 7이전에는 메소드 영역 내에 위치했다. OOM의 위험성이 있었다.
- 자바 8부터는 힙 영역으로 옮겨졌고, GC가 가능해졌다.상수 풀의 크기를 조정하는 옵션도 지원한다.

## 참고

- https://gyoogle.dev/blog/computer-language/Java/Interend%20String%20in%20Java.html
- https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Java/%5BJava%5D%20Java%EC%9D%98%20String.md
