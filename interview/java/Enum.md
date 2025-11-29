# Enum

- Enumeration의 약자로, 열거, 목록 등의 의미를 지닌다. 쉽게 말해 상수 데이터의 집합이다.
- 서로 관련된 상수들의 집합을 타입 안전하게 사용할 수 있도록 해주는 매우 유용한 기능이다.

### 특징

- enum의 각 상수들은 해당 enum 타입의 인스턴스이다. 싱글톤으로 각 인스턴스를 유지한다. 때문에 상수간의 비교를 `==`로 할 수 있다. (주소 비교)
- java.lang.Enum 클래스를 이미 상속하고 있다.
  - 몇 가지 기본적인 메서드를 제공한다.
  - Enum 자체가 Serializable을 이미 구현 중이기 때문에, enum은 Serializable 구현없이 직렬화가 가능하다.
- 타입 안전성 보장, 최적화된 switch문 사용 가능, 가독성 증가, 변경 범위 최소화 등의 장점이 있다.
- private 생성자 뿐 아니라 리플렉션 우회 방지까지 JVM 차원에서 잠궈놓았다. (유일한 인스턴스 집합 보장)
- ordinal()은 선언 순서 기반이라 사용에 주의해야 한다.

### 직접 구현 예시

```java
public enum Color {
    RED, GREEN, BLUE;
}
```

```java
public class Color {

    // 인스턴스를 미리 만들어둠
    public static final Color RED = new Color("RED");
    public static final Color GREEN = new Color("GREEN");
    public static final Color BLUE = new Color("BLUE");

    private final String name;

    // 외부에서 인스턴스를 만들지 못하도록 private 생성자
    private Color(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}
```

위 두 코드 뭉치는 비슷한 역할을 한다.

### 직렬화, 역직렬화

- 언급했듯이, Enum 자체가 Serializable을 이미 구현 중이기 때문에, Serializable 구현없이 직렬화가 가능하다.
- name() 기반 직렬화는 역직렬화시 주의해야한다. 코드상 변수명인 name 변경 시 이미 서비스에 사용중인 name과 차이가 생길 수 있다.

#### name()기반 대신에 사용할, 직렬화/역직렬화 방식 좋은 예시

```java
public enum Color {
    RED("R"),
    BLUE("B");

    private final String code;

    Color(String code) { this.code = code; }

    @JsonValue
    public String getCode() {
        return code;
    }

    @JsonCreator
    public static Color fromCode(String code) {
        for (Color c : values()) {
            if (c.code.equals(code)) return c;
        }
        throw new IllegalArgumentException();
    }
}
```

#### code가 바뀌면 어짜피 똑같은 문제가 발생하지 않나?

- code는 외부와 약속한 식별자로 잘 안바뀐다.
- name은 개발자가 마음대로 리팩터링할 수 있는 값이다. 그러므로 code 기준으로 하는게 더 낫다.
- 그래서 발생할 문제를 해결하기 위해 DB/메시지 큐 데이터 마이그레이션하거나 아래처럼 여러 코드 지원을 하기도 한다.

```java
Color(String... codes) { this.codes = List.of(codes); }

RED("R", "RED_OLD1", "RED_OLD2");
```

### 응용

- 추상 메서드를 정의해서 상수 메서드로도 사용 가능하다. 람다식도 가능하다.
- 상태와 행위를 묶어줄 수 있다.
- 이펙티브자바 - 전략 열거 패턴 사용, 인터페이스로 enum 확장
- switch문 사용하기

## 참고

- https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%B4%EA%B1%B0%ED%98%95Enum-%ED%83%80%EC%9E%85-%EB%AC%B8%EB%B2%95-%ED%99%9C%EC%9A%A9-%EC%A0%95%EB%A6%AC
