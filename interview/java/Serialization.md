# Serialization vs Deserialization

## 직렬화(Serialization)

- 직렬화란 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터 변환하는 기술 (객체 -> 바이트스트림)
- 자바에서는 간단히 `java.io.Serializable` 인터페이스 구현으로 직렬화/역직렬화가 가능하다.
  - Primitive 타입이 아닌 Reference 타입처럼 주소값을 지닌 객체들은 바이트로 변환하기 위해 Serializable 인터페이스를 구현해야 한다.
  - cf) `Serializable`은 마커 인터페이스이며, `writeObject()` 메서드에 보면 `instanceof`로 타입을 검사하는 로직이 있다.

### 활용

- JVM에 상주하는 객체 데이터를 영속화할 때
- Servlet Session
- Cache
- Java RMI(Remote Method Invocation)

### 규칙

#### 대상 제외

- `transient` 키워드로 선언된 필드들 직렬화 제외
- `static` 필드는 클래스의 상태이므로 직렬화 제외

#### 상속

- 부모 클래스가 Serializable을 구현하면, 하위 클래스도 자동으로 직렬화 가능
- 부모 클래스가 Serializable을 구현하지 않았지만 하위 클래스가 구현한 경우, 하위 클래스의 직렬화 과정에서는 부모 클래스 필드는 직렬화 제외

#### 클래스

- 내부 클래스, 로컬 클래스, 익명 클래스는 자동으로 바깥 클래스의 참조를 갖기 때문에, 바깥 클래스가 직렬화되지 않았다면 직렬화 과정에서 문제가 발생할 수 있다.
- 정적 중첩 클래스(static nested class)는 바깥 클래스의 참조를 갖지 않기 때문에 직렬화 해도 안전하다.

#### 컬렉션, 맵

- Serializable을 이미 구현했다. 직렬화하려면 내부 객체도 직렬화 가능해야 한다.

## 역직렬화(Deserialization)

- 바이트로 변환된 데이터를 다시 객체로 변환하는 기술
- 신뢰할 수 있는 데이터만 역직렬화 해야한다.

## 구현

```java
import java.io.*;

// 직렬화 가능하게 하려면 Serializable 인터페이스를 구현해야 함
class Person implements Serializable {
    private static final long serialVersionUID = 1L; // 클래스 버전 관리
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        Person person = new Person("John Doe", 30);

        // 객체 직렬화
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
            oos.writeObject(person);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 객체 역직렬화
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
            Person deserializedPerson = (Person) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```

## serialVersionUID

- 객체가 직렬화되어 저장되거나 전송된 후, 해당 객체를 정의하는 클래스가 변경되었을 때, 역직렬화 과정에서 클래스의 호환성을 검사할 수 있게 해준다.
- 직렬화된 객체의 클래스가 업데이트되어 필드가 추가되거나 제거되었을 경우, serialVersionUID가 일치하지 않으면 역직렬화 과정에서 InvalidClassException이 발생한다.

### serialVersionUID를 지정하지 않는 경우

- Java 직렬화 메커니즘은 클래스의 구조를 기반으로 해시 같은 방법을 사용하여 자동으로 serialVersionUID 값을 생성한다.
- 클래스의 구조가 변경될 때마다(필드 추가/삭제, 메소드 변경 등) 자동 생성된 serialVersionUID 값도 변할 수 있다.
- 이를 바탕으로 원하는 클래스 구조를 유지하는지 검사할 수 있다.

### 개발자가 serialVersionUID를 명시적으로 지정해준 경우

- serialVersionUID가 final로 명시적으로 지정되어 있다면, 그 값은 클래스 버전이 변경되어도 변하지 않는다. 개발자가 의도적으로 호환성 관리를 하게 도와준다.
- 역직렬화 과정에서 직렬화된 객체의 구조가 실제 클래스의 구조와 달라졌더라도, serialVersionUID 값이 같다면 Java는 이를 호환 가능한 것으로 간주하고 InvalidClassException 오류를 발생시키지 않는다.

## 참고

- https://gyoogle.dev/blog/computer-language/Java/Serialization.html
- https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Java/%5BJava%5D%20%EC%A7%81%EB%A0%AC%ED%99%94.md
