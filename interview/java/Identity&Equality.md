# 동일성과 동등성(Identity and Equality)

## 동일성(Identity)

- 자바에서는 == 연산자를 사용하여 검사한다.
- 동일성은 두 객체 참조가 메모리상에서 같은 위치(주소), 즉 동일한 객체를 가리키는지 여부를 확인한다.

## 동등성(Equality)

- equals()를 사용해 검사한다.
- 동등성은 두 객체의 상태나 값이 같은지를 확인한다.

### Object.equals()

- Object 클래스에서는 기본적으로 == 연산자와 같은 동작을 한다.
- `Object.equals()` 메소드를 적절하게 오버라이드하여 객체의 값 비교를 구현할 수 있다.

### Wrapper Class의 비교

```java
String str3 = new String("hello");
String str4 = new String("hello");
boolean result2 = str3.equals(str4); // true, 두 문자열의 내용이 같습니다.
```

String 클래스를 예시로 들면,

- new로 생성하여 서로 다른 주소를 가르키고 있지만, equals를 사용하면 값이 같기때문에 true가 반환된다.
- 이것은 String이 값을 비교하도록 `eqauls()`를 오버라이딩했기 때문이다.
- 자바의 wrapper class들은 Object 클래스의 equals() 메소드를 오버라이드하여 값 비교를 수행하도록 구현되어 있다.
