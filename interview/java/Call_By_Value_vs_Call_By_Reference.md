# Call By Value vs Call By Reference

- Call By 는 함수 호출 시에 어떻게 인자를 전달하는지에 대한 문제이다.

## Call By Value

- 함수에 인자를 전달할 때, 인자의 값의 복사본이 생성되어 함수에 전달

## Call By Reference

- 함수에 인자를 전달할 때, 인자가 가리키는 객체의 참조(주소)가 전달

# 자바는 Call By Value

- 기본 데이터 타입 : 값 자체가 함수에 복사되어 전달
- 객체 : 객체의 참조(주소의 값)가 복사되어 전달

#### "자바는 객체 참조를 사용하는데, Call By Refernce가 아닌가?"

> 자바에서 메서드에 객체의 참조가 인자로 넘어가면, 메서드 내부에서 해당 객체의 값을 수정 가능하다. 그래서 Call By Reference와 착각할 수 있는데, 엄밀히 말하면 참조값이 복사된 것이라 Call By Value이다.

## 기본 데이터 타입의 Call By Value

```java
public class Main {
    public static void change(int x) {
        x = 100;
        System.out.println(x); //100
    }

    public static void main(String[] args) {
        int a = 10;
        System.out.println(a); // 10
        change(a);
        System.out.println(a); // 10
    }
}
```

- change 메서드에서 넘겨받은 primitive type 인자를 수정하지만, 복사본을 수정한 것이라서 원본 값은 그대로이다.

## 객체 참조의 Call By Value

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
  - 힙 메모리에 `이름이 Bob인 Person`이 새로 생성된다.
  - 힙에 있는 생성된 객체를 가르키는 일종의 주소가 참조변수 p에 저장된다. (p는 스택에 존재)
  - 즉, p는 생성한 `이름이 Bob인 Person`을 가르킨다.

## 객체 참조의 Call By Value \(메서드 없이, 위와 동일)

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

# 추가

## 장단점

### Call By Value

"안정성과 예측 가능성"

- 장점
  - 안정성이 높고, side effect가 적다.
  - 함수간 독립성 -> 병렬 처리에 유리
- 단점
  - 복사 비용이 발생한다. 값이 크면 비효율적일 수도 있다.
  - 공유 객체가 필요할 경우, 복잡한 구조가 필요하다. (primitive만 쓰면 공유 불가)

### Call By Reference

"성능과 유연성"

- 장점
  - 함수에서 직접 외부 상태를 변경 가능하여 효율적
  - 여러 리턴값처럼 사용 가능
- 단점
  - side effect 가능성
  - 데이터 무결성 유지가 어려움
  - 코드 추적 어려움, 디버깅 난이도 상승
