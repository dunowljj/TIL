# 불변 객체(Immutable Object) vs 가변 객체(Mutable Object)

## 불변 객체(Immutable Object)

### 의미

- 생성 후 그 상태를 변경할 수 없는 객체
- ex) String, Integer,
  LocalDate등이 이에 해당한다.

### 특징

- 불변 객체의 상태를 변경하려면 새로운 객체를 생성해야 한다.
- 불변 객체 자체는 스레드 안전하다. 여러 스레드가 동시에 불변 객체에 접근해도 객체의 상태가 변경되지 않기 때문에 동기화 문제가 발생하지 않는다.
- 오류 가능성을 줄이고 코드를 더 쉽게 이해하고 유지보수할 수 있다.

### 조건

- 모든 필드는 final로 선언한다.
- 자신의 내부 상태를 변경할 수 있는 메서드를 제공하지 않는다.
- 객체가 참조하는 다른 객체도 모두 불변이어야 한다.

## 가변 객체(Mutable Object)

### 의미

- 객체가 생성된 후에도 그 상태를 변경할 수 있다.

### 특징

- 객체의 상태를 직접 변경할 수 있어, 특정 상황에서 성능에 유리하다.
- 객체의 상태를 유연하게 조정할 수 있으나 버그 발생의 원인이 되기도 한다.
- 동기화를 적절히 관리하지 않으면 데이터 무결성에 문제가 생길 수 있다. 데이터 일관성을 보장하지 못한다.
- ex) StringBuilder, ArrayList, HashMap

## 방어적 복사

객체의 데이터를 외부에서 임의로 변경할 수 없도록 보호하기 위해, 객체의 복사본을 만드는 기술

<details>
<summary>예시</summary>
<div markdown="1">

````java
public class Example {
    private Date date;

    public Example(Date date) {
        this.date = new Date(date.getTime()); // 생성자에서 방어적 복사
    }

    public Date getDate() {
        return new Date(date.getTime()); // 반환 시 복사본 제공
    }
}
``
- 이런식으로 반환 시 복사본을 생성해서 반환하면 불변성이 유지된다.
```java
public void setDate(Date date) {
    this.date = new Date(date.getTime()); // 설정 시 복사본 사용
}
````

</div>
</details>

- setter에 새 객체를 초기화하도록 구현할 수도 있다.

## 깊은 복사 vs 얕은 복사

### 얕은 복사

- 객체의 최상위 수준의 필드만 복사하는 것

<details>
<summary>예시</summary>
<div markdown="1">

```java
class Example {
    int value;

    public Example(int value) {
        this.value = value;
    }
}

public class Main {
    public static void main(String[] args) {
        Example original = new Example(100);
        Example shallowCopy = original; // 얕은 복사: original 참조를 shallowCopy에 할당

        shallowCopy.value = 200; // shallowCopy를 통해 값 변경

        System.out.println(original.value); // 출력: 200, original을 통해서도 변경된 값이 보임
    }
}
```

- 사용하던 참조를 새 참조를 초기화하면서 할당하는 것
- 그 외에 `Object.clone()`사용 시에 복사되는 요소들이 참조타입인 경우, 참조 자체가 복사되어 얕은 복사가 된다. (clone은 이펙티브 자바에서 비권장한다.)
</div>
</details>

### 깊은 복사

- 객체에 포함된 모든 수준의 필드를 재귀적으로 복사하는 것. 즉, 해당 객체가 참조하는 모든 객체들까지 새롭게 복사한다.

<details>
<summary>예시</summary>
<div markdown="1">

```java
class Example {
    int value;

    // 기본 생성자
    public Example(int value) {
        this.value = value;
    }

    // 복사 생성자: 깊은 복사를 위해 사용
    public Example(Example another) {
        this.value = another.value;
    }
}

public class Main {
    public static void main(String[] args) {
        Example original = new Example(100);

        // 깊은 복사: original 객체를 새로운 Example 객체로 복사
        Example deepCopy = new Example(original);

        deepCopy.value = 200; // deepCopy를 통해 값 변경

        System.out.println(original.value); // 출력: 100, original 객체는 변경되지 않음
        System.out.println(deepCopy.value);  // 출력: 200, deepCopy 객체의 변경된 값
    }
}
```

- 새로 객체를 할당하고, 필드의 값도 직접 초기화 해준 경우
</div>
</details>

#### 참고

- 불변성을 유지하기 위해 깊은 복사를 하려고 할 때,
  - 복사하려는 객체의 필드가 객체라면 필드에 존재하는 객체의 내부도 재귀적으로 모두 복사해야 불변성을 지킬 수 있다.
  - 컬렉션을 복사하는 경우, 각 요소가 객체라면 마찬가지로 재귀적으로 복사해야 한다.

## 참고

- https://steady-coding.tistory.com/m/559
- https://sup2is.github.io/2020/01/29/java-immutable-object-with-string.html
- https://mangkyu.tistory.com/131
- G선생
