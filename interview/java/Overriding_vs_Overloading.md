# 오버라이딩(Overriding) vs 오버로딩(Overloading)

## 오버라이딩(Overriding)

- 상위 클래스 혹은 인터페이스에 존재하는 메소드를 하위 클래스에서 필요에 맞게 재정의하는 것
- 자바의 경우는 오버라이딩 시 동적바인딩된다.
- @Override 어노테이션 사용해서 명시해주는게 좋다.

### 조건

- 슈퍼클래스의 메서드와 동일한 이름, 매개변수 리스트, 반환 타입을 가져야 한다.
- 반환타입
  - primitive 타입인 경우 반환타입이 동일해야 한다.
  - 공변(Covariant) 타입인 경우에는 오버라이딩 가능하다.
- 오버라이딩하는 메서드는 슈퍼클래스의 메서드보다 접근성이 더 제한적이어서는 안 된다. (public > protected > default > private)

참고로, 그 외에도,

- final, private, static 메서드는 오버라이딩 불가하다.
  - final은 불가
  - private은 접근 불가
  - static 메서드는 따라서 작성 가능하지만, 오버라이딩이 아닌 숨김이다.
- 메서드가 예외를 던지는 경우
  - 체크된 예외 : 슈퍼클래스의 메서드에서 선언된 체크된 예외들과 같거나 이들의 하위 타입만 던질 수 있다.
  - 언체크 예외: 오버라이딩하는 메서드는 슈퍼클래스의 메서드와 관계없이 언체크된 예외(RuntimeException과 그 하위 타입)를 던질 수 있다.

## 오버로딩(Overloading)

- 오버로딩은 같은 이름을 가진 메서드를 같은 클래스 내에서 여러 번 정의하는 것

### 조건

- 메서드 이름이 일치하는 경우를 말한다.
- 매개변수의 타입, 순서, 개수가 적어도 하나는 달라야 한다.
- 반환타입은 무관하다.
- 접근제어자와 예외도 무관하다.

### 예시

```java
public class Calculation {
    public int calculate(int a, int b) {
        return a + b;
    }

    // 오버로딩이 아니며, 컴파일 에러가 발생
    // public double calculate(int a, int b) {
    //     return a + b;
    // }

    // 참조변수 이름만 바뀌었지 같은 메서드
    // public int calculate(int b, int a) {
    //     return a + b;
    // }

    // 올바른 오버로딩 예시: 매개변수 목록이 다름
    public double calculate(double a, double b) {
        return a + b;
    }
}
```
