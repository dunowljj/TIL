Generics
======
# 1. Why Use Generics?

# 2. Generic Types
- 지네릭 타입은 타입에 대해 매개변수화된 지네릭 클래스 또는 인터페이스이다.
## A Simple Box Class (예시)
- 논제네릭 모든 타입의 객체에서 작동하는 `Box`클래스를 예시로 설명하면서 시작한다. 해당 클래스는`set`메서드와 `get`메서드만 제공한다.
```
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```
- `Object`로 받고 반환하기때문에, 원하는 무엇이든 통과시킬 수 있고, 제공되는 것은 하나의 원시 타입이 아니다.
- 컴파일 할 때, 클래스가 어떻게 사용되는지 입증할 방법이 없다.
- 코드의 한 부분에서  `Integer`을 박스에 위치시키고 `Integer`를 받아낼 것으로 예상하는 반면에 실수로 코드의 다른 부분에서 runtime error를 야기하는 `String`형식을 통과시킬 수도 있다.
***

## A Generic Version of the Box Class
- 지네릭 클래스는 해당 형식으로 정의된다.
```
class name<T1, T2, ..., Tn> { /* ... */ }
```
- \<>로 구분되는 타입 매개변수 영역은 클래스 이름을 따라온다. 타입 파라미터를(또는 타입변수)T1,T2...,and Tn으로 명시한다.
- `Box`를 지네릭을 사용하도록 변경하려면, 너는 `public class Box` 에서 `public class box<T>`로 바꿈으로써 지네릭 타입을 선언해야 한다. 이것은 어느 클래스에서든 사용될 수 있는 타입 변수를 소개한다.
- 위와 같은 변화에 의해 `Box`클래스는 이렇게 된다:
```
/**
*Generic vesion of the Box class
*@param <T> the type of the value being boxed
*/
public class Box<T> {
    //T stands for "Type"
    private T t;

    public void set(T t) {this.t = t;}
    public T get() {return t;}
}
```
- 보이듯이 모든 나타나는 모든 `Object`는 `T`로 대체되었다. 타입 변수는 primitive만 아니라면 너가 지정할 수 있다. 어떤 클래스 타입, 어떤 인터페이스 타입, 어떤 배열 타입, 심지어 다른 변수 타입도 가능하다.
***

## Type Parameter Naming Conventions
- 관례에 의하면, 매개변수 이름들은 대문자로 한 글자이다. 이 관례는 너가 이미 알고 있는 naming convention과 현저한 대조를 보인다. 그리고 적절한 이유를 가지고 있다. 이 관례가 없다면 **타입 변수**와 **일반적인 클래스 혹은 인터페이스** 사이의 차이를 말하기 힘들 것이다.
- 주로 사용되는 이름들은 이러하다:  
    - E - Element (자바 컬렉션 프레임웍에서 광범위하게 사용)
    - K - Key
    - N - Number
    - T - Type
    - V - Value
    - S,U,V etc. -2nd, 3rd, 4th types
- 이 이름들이 JAVA SE API와 글의 나머지 내용에서 사용되는 것을 볼 수 있을 것이다.
***

## Invoking and Instantiating a Generic Type
- 지네릭 `Box`클래스를 참조하기 위해, T를 Integer와 같은 concrete 값으로 대체한 지네릭 타입을 사용해야한다.
```
Box<Integer> integerBox;
```
당신은 지네릭 타입의 동작을 일반적인 메서드의 동작으로 생각할 수 있다. 그러나 당신은 메서드에 값을 전달하는 대신, `Box`클래스 자신에게 타입 전달값을 전달한다.

>Type Parameter와 Type Argument 용어 : 많은 개발자들이 **"type parameter"**와 **"type argument"**라는 용어들을 혼재해서 사용한다. 그러나 이 용어들은 같지 않다. 코딩에서 매개변수화된 타입을 생성하기 위해 **type argument**를 사용한다.  
그러므로 `Foo<T>`에 있는 `T`는 **type parameter(매개변수)**이고 `Foo<String> f`에 있는 `String`은 **type argument(인수)**이다.

- 다른 어떤 정의들과 같이, 이 코드는 확실히 새 `Box`객체를 만들지 않는다. 심플하게 `integerBox`가 "Box of Integer"에 대한 참조를 가졌음을 뜻한다. "Box of Integer"는 `Box<Integer>`을 읽는 방법이다.
- 일반적으로 지네릭 타입의 사용은 매개변수화된 타입으로 여겨진다.
- 보통은 이 클래스의 인스턴스를 만들기 위해 `new`키워드를 사용한다 `<Integer>`을 클래스 이름과 괄호 사이에 위치시켜라.
```
Box<Integer> integerBox = new Box<Integer>();
```
***

## The Diamond
- java SE 7부터, 문맥으로부터 타입 인수를 컴파일러가 결정하고 추론할 수 있다면, 타입 인수를 비운(<>)채로 지네릭 클래스 생성자를 불러오는 방식으로 대체할 수 있다. 꺽쇠 괄호 한쌍(<>)은 다이아몬드로 불린다. 다음과 같이 `Box<Integer>`의 인스턴스를 생성 가능하다.
```
Box<Integer> integerBox = Box<>();
```
- 다이아몬드 표기법과 타입 추론에 대한 추가 정보 [Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)

