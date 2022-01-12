Generics
======
# 1. Why Use Generics?

# 2. Generic Types
- 지네릭 타입은 타입에 대해 매개변수화된 지네릭 클래스 또는 인터페이스이다.
## A Simple Box Class (예시)
- 논제네릭 모든 타입의 객체에서 작동하는 `Box`클래스를 설명하면서 시작한다. 해당 클래스는`set`메서드와 `get`메서드만 제공한다.
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
