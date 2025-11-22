# 형변환(Type Casting)

## 형변환이란?

- 변수 또는 값의 데이터 타입을 다른 데이터 타입으로 변환하는 과정
- 형변환을 통해서 사용할 수 있는 멤버의 개수를 조절할 수 있다.
- 형제끼리는 형변환이 안된다. 부모가 같을 뿐 타입이 아예 다르기 때문이다.

## Upcasting vs Downcasting

### Upcasting

```java
Parent parent = new Child();
```

- 서브 클래스의 인스턴스 -> 슈퍼 클래스 타입
- 컴파일러에 의해 자동으로 일어난다. (묵시적 형변환)

### 활용

- 다형성, 상속에 활용한다.
- 공통적인 부분(ex.인터페이스 타입)으로 간단하게 코드를 다룬다. 코드의 유연성을 높이고, 다양한 객체를 동일한 방식으로 처리할 수 있게 한다.
- 자식 클래스의 고유한 메서드를 실행할 수 없다. 이때 다운캐스팅 필요하다.

### Downcasting

```java
Parent parent = new Child(); // 업캐스팅이 선행됨
Child child = (Child) parent;
```

- 슈퍼 클래스 타입의 참조 -> 서브 클래스 타입
- 명시적으로 수행해야 한다. (명시적 형변환)
- 타입 체크 없이 수행될 경우 런타임 오류(ClassCastException)를 일으킬 수 있다. instanceof를 사용하는 게 좋다.

#### 활용

- 업캐스팅된 참조를 통해 다형적으로 처리된 객체들 중 특정 객체의 상세 기능을 사용하고자 할 때 유용하다.

## 왜 Downcasting은 명시적으로 사용하는가?

### 이유
```java
Parent p = getSomething();  // 반환 타입: Parent
Child c = (Child) p;

// 컴파일러, 너가 뭘 알 수 있는데?
public Parent getSomething() {
    if (Math.random() > 0.5) return new Child();
    else return new Parent();
}
```

- 결국 컴파일 타임에 컴파일러는 계층도를 알고 있지만, <u>동적 정보</u>를 몰라서 인스턴스 타입을 구체적으로 알 수 없다.
- 즉, 결과에 따라 형제간 캐스팅이 된다면 런타임 에러가 발생한다.
- 즉, 다운캐스팅은 타입 안전성을 컴파일 시에 완전히 검증할 수 없어 위험하다.

### 근데 명시적 형변환을 한다고 뭐가 해결되나?
- 그래도 문제는 발생한다. 하지만, 개발자가 위험을 인지하고 의도적으로 수행하도록 한다. 어떻게 보면 경고, 혹은 책임의 전가라 볼 수 있다.

## 참고

- https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85-%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85-%ED%95%9C%EB%B0%A9-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0
- https://gyoogle.dev/blog/computer-language/Java/Casting.html
