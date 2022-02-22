Generics
======
# 1. Why Use Generics?
- 간단히 말해서 지네릭스는 클래스, 인터페이스 그리고 인터페이스를 정의할때, (클래스나 인터페이스의) 타입이 파라미터가 될 수 있게 해준다.
- 메서드 선언에 형식 매개변수(formal parameter)가 사용되듯이?, 타입 매개변수는 다른 입력값에 대해 같은 코드를 재사용하는 방법을 제공한다.
- 차이점은 형식 매개변수는 값들이 입력되고, 타입 매개변수에는 타입이 입력된다.
+ 지네릭을 사용한 코드는 지네릭을 사용하지 않은 코드보다 많은 이점을 가진다.
    + 컴파일 시점 강력한 타입 체크
        + 자바 컴파일러는 만약 코드가 타입 안정성을 위배한다면, 일반적인 코드와 에러 이슈에 대해 강한력한 타입 검사를 한다.
        + 컴파일 시점에 에러를 수정하는 것이 찾기 힘든 런타임 에러를 수정하는 것 보다 쉽다.
    + 지네릭없이 형변환을 요구하는 코드:  
    ```
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);
    ```
    + 지네릭으로 변경하면, 변환을 필요로하지 않는다.  
    ```
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast
    ```
    + 프로그래머들에게 지네릭 알고리즘 구현을 가능하게 한다.  
    지네릭을 사용함으로써, 프로그래머들은 지네릭 알고리즘을 구현할 수 있다. 지네릭 알고리즘은 다른 타입의 컬렉션에서 동작할 수 있고, 커스터마이징 될 수 있으며, 안전한 타입과 좋은 가독성을 가진다.



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

>Type Parameter와 Type Argument 용어 : 많은 개발자들이  **"type parameter"** 와 **"type argument"** 라는 용어들을 혼재해서 사용한다. 그러나 이 용어들은 같지 않다. 코딩에서 매개변수화된 타입을 생성하기 위해 **type argument** 를 사용한다.  
그러므로 `Foo<T>`에 있는 `T`는 **type parameter(매개변수)** 이고 `Foo<String> f`에 있는 `String`은 **type argument(인수)** 이다.

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
***

## Multiple Type Parameters
- 이전에 언급했듯이, 지네릭 클래스는 복수의 파라미터를 가질 수 있다. 
- 예시로 지네릭 `Pair` 인터페이스를 구현하는 지네릭`OrderedPair`클래스이다:
```
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
```
- `OrderedPair`클래스의 두 가지 인스턴스 생성 방법:
```
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```
- `new OrderedPair<String, Integer>`라는 코드는 `K`를 `String`으로, `v`를 `Integer`로 인스턴트화한다. 그러므로, `OrderedPair`의 생성자의 매개변수 타입은 각각 `String`과 `Integer`이다. 오토박싱 덕분에 `String`과 `int`도 제한을 통과할 수 있다.
- 다이아몬드에서 언급했듯이, 자바 컴파일러가 타입을 추론할 수 있기 때문에, 아래 문장처럼 코드를 줄일 수 있다.
```
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```
- 지네릭 인터페이스를 생성할 때 지네릭 클래스 생성의 관례를 따른다.
***

## Parameterized Types
- 타입 파라미터(`K` or `V`)를 매개변수화된 타입(`List<String>`)으로 대체할 수 있다. `OrderedPair<K,V>`를 사용해 예시를 들자면:
```
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```
***

## Raw Types
- raw type은 타입 인수가 없는 지네릭 클래스나 인터페이스를 지칭하는 말이다. 예시로 주어진 `Box` 클래스:
```
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
```
- `Box<T>`의 매개변수화된 타입을 생성하기 위해, 형식 타입 매개변수 `T`에 실제 타입 인수(actual type argument, 실제라고 적는게 맞는지 모르겠다.)를 제공해야 한다:
```
Box<Integer> intBox = new Box<>();
```
- 만약 실제 타입 인수가 생략되었다면, `Box<T>`의 raw type이 생성된다:
```
Box rawBox = new Box();
```
- 그러므로, `Box`는 지네릭 타입인 `Box<T>`의 raw type이다. 그러나 지네릭 타입이 아닌 클래스나 인터페이스는 raw type이 아니다.
- 많은 API클래스들(`Collections`같은 클래스들)에서 지네릭은 JDK5.0 이전에는 존재하지 않았기 때문에, raw type들은 많은 레거시 코드에서 나타난다. raw type을 사용할 때, 필수적으로 pre-generics 행동( `Box`가 `Object`들을 제공)을 해야한다. . 이전 버전과의 하위호환성을 위해 매개변수화된 타입을 그것의 raw type에 할당하는 것은 허용된다.
```
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
```
- 그러나 raw type을 매개변수화된 타입에 할당하면 경고를 받는다:
```
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
```
- 만약 지네릭 대응하는 타입에 정의된 메서드를 호출하기 위해 raw type을 사용한다면 그 또한 경고를 받는다:
```
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
```
- 해당 경고는 raw type 이 안전하지 않은 코드를 런타임에 잡아내는 것을 지연하면서, 일반적인 타입 검사를 우회한다는 것을 보여준다. 그러므로 raw type들을 사용하는 것을 피해야 한다.
- [Type Erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) 섹션에 자바컴파일러가 어떻게 raw type을 사용하는지에 대한 정보가 더 있다.

## Unchecked Error Message
- 이전에 언급했듯이,지네릭 코드와 레거시 코드를 섞을 경우, 아래와 같은 경고 메세지를 마주할 것이다:
```
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```
- raw type을 사용하는 오래된 API를 사용할 때 이런 오류가 발생할 수 있다. 아래의 예시에 보이듯이:
```
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}
```
  
- "unchecked"라는 용어는 컴파일러가 타입 안전성을 보장하기 위해서 하는 모든 타입 검증을 위한 타입 정보가 불충분함을 의미한다. "unchecked" 경고는 컴파일러가 힌트를 주더라도 기본적으로 보이지 않는다. 모든 "unchecked" 경고를 보이 위해서는 `Xlint:unchecked.`와 함께 다시 컴파일해야한다.
  
- `Xlint:unchecked`와 함께 이전 예제를 다시 컴파일하는 것은 아래와 같은 추가적인 정보를 제공한다.:
```
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning
```
- 완벽하게 unchecked 경고를 비활성하기 위해서는 `Xlint:unchecked` flag를 사용하면 된다. `SuppressWarning("unchecked")` 어노테이션은 unchecked 경고를 막아준다.
만약 **@SuppressionWarning** 구문에 익숙하지 않다면, [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)를 봐라.
***

# 3. Generic Methods
- 지네릭 메서드는 고유의 타입 매개변수를 가진 메서드이다. 이것은 지네릭 타입을 선언하는 것과 유사하지만, 타입 매개변수의 범위가 그것이 선언된 메서드로 제한되어 있다. 지네릭 클래스 생성자 뿐만 아니라 정적 그리고 정적이 아닌 지네릭 메서드들이 허용된다.
- `Util`클래스는 두 쌍의 객체를 비교하는 지네릭 메서드인 `compare`을 포함한다:
```
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```
- 이 메서드를 호출하기 위한 완전한 문장은 이러하다:
```
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2); //여기 3줄 타입 부분
```
- 3줄 타입 부분과 같이 타입이 명시적으로 제공된다. 일반적으로 이런 타입은 생략될 수 있으며, 컴파일러가 필요한 타입을 추론할 것이다:
```
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
``` 
- 타입 추론으로 알려진 이 특성은 꺽쇠 괄호에 타입을 지정하지 않고도 지네릭 메서드를 일반 메서드처럼 호출하는 것을 허용한다. 이 주제는 [Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)에서 깊게 다뤄진다.
***

# 4. Bounded Type Parameters
- 아마 매개변수화된 타입에 들어갈 타입 인수를 제한하고 싶었던 순간들이 있었을 것이다.
