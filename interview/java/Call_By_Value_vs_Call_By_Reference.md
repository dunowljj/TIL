# Call By Value vs Call By Reference

## Call by Value

- 함수에 인자를 전달할 때, 인자의 값의 복사본이 생성되어 함수에 전달

## Call by Reference

- 함수에 인자를 전달할 때, 인자가 가리키는 객체의 참조(주소)가 전달

## 자바는 Call By Value

- 기본 데이터 타입 : 값 자체가 함수에 복사되어 전달
- 객체 : 객체의 참조(주소의 값)가 복사되어 전달

> 자바에서 메서드에 객체의 참조가 인자로 넘어가면, 메서드 내부에서 해당 객체의 값을 수정 가능하다. 그래서 Call By Reference와 착각할 수 있는데, 엄밀히 말하면 참조값이 복사된 것이라 Call By Value이다.

## 예시 - Call By Value

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }

    void setName(String name) {
        this.name = name;
    }
}

public class Test {
    static void modifyReference(Person p) {
        p.setName("Alice");
        p = new Person("Bob");
        p.setName("Charlie");
    }

    public static void main(String[] args) {
        Person person = new Person("Original");
        System.out.println("Before modification: " + person.name); // Original

        modifyReference(person);
        System.out.println("After modification: " + person.name); // Alice
    }
}
```

- `modifyReference 메서드`의 파라미터 p는 인수로 주어진 객체의 주소값을 p에 복사한 것이다.
- `p = new Person("Bob");`에서 내부적으로 다음과 같은 동작이 일어난다.
  - 힙 메모리에 `Bob이라는 이름을 가진 새로운 Person 객체`가 생성된다.
  - 생성된 객체의 주소가 참조변수 p에 저장된다. (스택 메모리에 저장된다.)

## 예시 - Call By Value 메서드 없이

```java
public class Test {
    public static void main(String[] args) {
        Person person = new Person("Original");
        System.out.println("Before modification: " + person.name); // Original

        // 여기서 modifyReference 메서드의 로직을 직접 적용
        person.setName("Alice");
        Person p = person;
        p = new Person("Bob");
        p.setName("Charlie");

        System.out.println("After modification: " + person.name); // Alice
    }
}
```

- 해당 내용은 바로 위의 메서드가 있을때 예시와 동일한 내용이다.
- 즉, `modifyReference(Person p) 함수`에
  `modifyReference(person)`과 같이 넣어서 사용하는 것은, p라는 참조변수 입장에서 `Person p = person;`와 동일하다!