# Record

### Record란?

- Java 14버전부터 도입되고 16부터 정식 스펙에 포함
- 데이터 지향 메소드를 자동으로 구현해준다.
- Entity나 DTO 구현에 편리하다.

### 구조

```java
public record Car(String name, String company) {}
```

- `레코드명(헤더), {바디}` 형태이다. 헤더에 나열되는 필드를 컴포넌트라고 한다.
- 컴파일러는 헤더를 통해 내부 필드를 추론한다.
- record 내 각 필드(헤더에 나열)는 private final로 선언된다.
- 생성자, getter, equals, hashCode, toString도 자동으로 생성된다.
  - getter 메소드의 경우 구현시 getXXX()로 명칭을 짓지만, 자동으로 만들어주는 메소드는 name(), age()와 같이 필드명으로 생성된다.

### 특징

- record는 불변 객체로 abstract로 선언할 수 없으며 암시적으로 final로 선언된다. 값이 정해지면 setter를 통해 값을 변경할 수 없다.
- 헤더에서 정의한 멤버만을 record에서 관리한다.
  - 레코드 내부에 멤버 변수를 선언할 수 없다.
  - static 변수는 생성이 가능하다.
- 다른 클래스를 상속 받을 수 없고, 인터페이스 구현은 가능하다.
- 매개변수가 없는 생성자는 제공하지 않는다.
- 메서드 추가가 가능하며, 자동 생성된 메서드는 재정의 가능하다.
- static 메서드, static 필드, 중첩 클래스, 제네릭 타입 지정으로 모두 가능하다.

### 컴팩트 생성자

```java
public record Car(String name, String company) {

    public Car {
        Objects.requireNonNull(name);
        Objects.requireNonNull(company);
    }
}
```

- 생성자 매개 변수를 받는 부분이 사라진 형태
- 개발자가 명시하지 않아도 컴팩트 생성자의 마지막 부분에 초기화 구문이 자동으로 삽입된다.
- 컴팩트 생성자 내부에서는 인스턴스 필드에 접근할 수 없다. 위의 코드에서 name과 company는 멤버 변수가 아닌 파라미터 변수이다.
- 컴팩트 생성자의 선언 의도는 생성자 본문에 검증 및 정규화용 코드만 넣는 것이다.
- 컴팩트 생성자의 사용은 일반 생성자와 같다.

### Entity로 활용?

- 프록시를 생성하기 위해서는 엔티티는 불변이어서는 안된다. 불변의 특성을 가지는 Record는 Entity로 활용하기 부적합하다.
- Record는 매개변수 없는 생성자를 지원하지 않는다. 리플렉션에 제약을 가한다.
- 데이터 변질 우려가 적어 DTO로 사용하기 적합하다.

## 참고

- https://velog.io/@power0080/java%EC%9E%90%EB%B0%94-record%EB%A5%BC-entity%EB%A1%9C
- https://gyoogle.dev/blog/computer-language/Java/Record.html
