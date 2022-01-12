지네릭스, 열거형, 애너테이션
========
# 1. 지네릭스(Generics)
## 1.1. 설명
- 다양한 타입의 객체들을 다루는 **메서드**나 **컬렉션 클래스**에 컴파일 시의 타입 체크를 해주는 기능
- **타입의 안정성을 제공**
- **타입체크**와 **형변환**을 생략 가능 -> 코드가 간결해짐
- 형변환 생략 ? : 원래는 Object형으로 반환되던거 바꿔주니 안해도 됨
## 1.2. 타입변수
- ArrayList클래스의 선언에서 클래스 이름 옆의 '<>'안에 있는 E를  '타입 변수'라고 하며, 일반적으로는 'Type'의 첫 글자를 따서 T를 사용
- ArrayList\<E>의 경우 Element 첫 글자
- 타입 변수가 여러 개인 경우 Map\<K,V>와 같이 ','를 구분자로 나열
```
//제네릭스 도입 전
public class ArrayList extends AbstractList{
    private transient Object[] elementData{~};
    public boolean add(Object o){~};
    public Object get(int index){~};
    ...
}
// 도입 후
public class ArrayList<E> extends AbstractList<E>{
    private transient E[] elementData{~};
    public boolean and(E o){~};
    public E get(int index){~};
    ...
}
```
- E 자리에 자료형을 넣으면 모두 대입되어 들어간다고 생각하면 편함. 형변환 불필요
## 1.3. 용어
- 지네릭 타입 호출 : 타입 매개변수를 지정하는 것
- 매개변수화된 타입 : 지정된 타입
- 주의! : Box\<String>과 Box\<Integer> 는 지네릭 클래스 Box\<T>에 서로 다른 타입을 대입하여 호출한 것일 뿐, 이 둘이 별개의 클래스를 의미하는 것은 아니다. **컴파일 후에** Box\<String>과 Box\<Integer>는 이들의 '원시 타입'인 Box로 바뀐다. 즉 지네릭 타입이 제거된다.
## 1.4. 다형성
- **참조변수에 지정해둔 지네릭 타입**과 **생성자에 지정해 준 지네릭 타입**은 **일치해야 한다.** 두 클래스가 서로 상속관계에 있어도 일치해야한다.
- 지네릭 타입이 **아닌 클래스간 다형성**은 괜찮다.
- 지네릭 타입이 상위 클래스인 ArrayList를 생성하고, 이 ArrayList에 해당 클래스의 자손을 저장하는건 가능하다. 단, **꺼낼때 형변환**이 필요하다.

## 1.5. Iterator\<E>
- 제네릭스가 도입되면서 기존의 소스에 Object가 들어간 클래스는 거의 바뀌었다고 보면 됨
## 1.6. HashMap\<K,V>
- 지정해야 할 타입이 두개, 콤마로 타입을 구분
- K, V는 임의의 참조형 타입을 의미
- 마찬가지로 get(Object key), keySet()/values() 사용 시 형변환 안해도 됨

## 1.7. 제한된 지네릭 클래스
- 지네릭 타입에 'extends' 사용 시 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.
- 주의할 점은 여전히 **한 종류**만 담을 수 있다. 하지만 자손들만 담을 수 있다는 제약이 하나 더 추가된 것이다.
- 클래스가 아닌 **인터페이스를 구현**해야 한다면 이때도 extends사용(implement(x))
```
//Fruit의 자손이면서 Eatable인터페이스도 구현해야 한다면?
class FruitBox<T extends Fruit & Eatable>{...}
```
## 1.8. 지네릭스의 제약
    1. static멤버에 타입 변수 T를 사용 불가 : T는 인스턴스 변수로 간주, 때문에 모든 객체에 동일하게 동작해야하는 static멤버는 인스턴스 변수 참조 불가 
    -> 생성할때 생각해보면 맞는 얘기
    ‣ 객체별로 다른 타입을 지정하는 것은 적절하다. 제네릭스는 인스턴스별로 다르게 동작하도록 만든 기능이다.
    2. 제네릭 타입의 배열 생성 x 참조 변수 선언은 가능: new, instanceof에 피연산자로 T 사용 불가
    -> new 연산자는 컴파일 시점에 T가 뭔지 정확히 알아야 하는데 그것을 알 방법이 없음(468)
## 1.9. 와일드 카드
- 제네릭 타입에 다형성을 적용할 방법은 없을까? -> 와일드 카드
<? extends T> 와일드 카드 상한 제한. T와 그 자손들만 가능 
<? super T> 와일드 카드 하한 제한, T와 그 조상들만 가능
<?> : 제한 없음. 모든 타입이 가능. <? extends Object>와 동일
- 다형성이용 생성할때, 매개변수로 받을 때
## 1.10. 지네릭 메서드
- 선언부에 지네릭 타입이 선언된 메서드
- ex) Collection.sort()
```
static <T> void sort(List<T> list, Comparator<? super T> c)
```
- 지네릭 클래스가 아닌 클래스에도 정의될 수 있다.
- 주의! 지네릭 **클래스에 정의된** 타입 매개변수 **T**와 **매개변수에 정의된** 타입 매개변수 **T**는 다르다.
- **static멤버**에는 타입 매개변수를 사용할 수 없지만, **메서드**에 지네릭 타입을 선언하고 사용하는 것은 가능하다.
- 메서드에 선언된 지네릭 타입은 **메서드 내에서만 지역적으로 사용**될 것이므로 메서드가 static이건 아니건 상관 없음
+ 호출 시 Juicer.\<Fruit>makeJuice(fruitBox)와 같이 타입 변수에 타입 대입해서 호출
+ 생략해도 컴파일러가 선언부를 통해 추정해준다.
+ 대입 타입 생략 불가 시 참조변수나 클래스 이름을 생략할 수 없다.
## 1.11. 지네릭 타입 형변환
- (지네릭 - 넌지네릭)지네릭 타입과 넌지네릭 타입은 서로 형변환이 가능하나 경고가 뜸
- (지네릭 - 지네릭)Object, String 등은 서로 형변환 불가
```
//error
Box<Object> objBox = (Box<Object>)new Box<String>();

//ok
Box<? extends Object> wBox = new Box<String>();
```
- 형변환, 다형성을 위해 와일드카드를 써야함.
## 1.12. 지네릭 타입의 제거
- 컴파일러는 지네릭 타입을 이용해 소스파일을 체크, 필요한 곳에 형변환을 넣어준다. 그리고 지네릭 타입을 제거한다. -> 클래스파일에 지네릭 타입 정보 없음(이전 버전 호환)  

      1. 지네릭 타입의 경계(bound)를 제거
      2. 지네릭 타입 제거 후에 타입 일치하지 않으면 형변환을 추가
        - List의 get()은 Object타입을 반환하므로 형변환 필요
        - 와일드 카드가 포함되어 있는 경우 적절한 타입으로의 형변환 추가
***
# 2. 열거형(enum)
## 2.1. 열거형이란?
- 여러 상수를 선언해야 할 때, 편리하게 선언할 수 있는 방법
- 일반적으로 상수를 선언할 때 상수가 많으면 코드가 불필요하게 길어진다.
```
class Card{
    enum Kind { CLOVER, HEART, DIAMOND, SPADE} //열거형 Kind를 정의
    enum Value {TWO, THREE, FOUR}              //열거형 Value를 정의

    final Kind kind;
    final Value value;
}
```
- 위처럼 따로 값을 지정 안해도 자동적으로 0부터 할당됨
- 카드 무늬와 숫자는 비교 대상이 아니어서 값이 같아도 false
- 열거형을 이용해서 상수를 정의한 경우 값을 비교하기 전에 타입을 먼저 비교하므로 값이 같더라도 타입이 다르면 컴파일 에러가 발생한다.
## 2.2. 정의 및 사용
```
enum 열거형이름 {상수명1, 상수명2, ... }

//예시
enum Direction{EAST, SOUTH, WEST, NORTH}
```
- '열거형이름.상수명' 과 같이 사용. 클래스의 static변수 참조와 사용 방법이 같음
## 2.3. 비교
- equals()가 아닌 '=='로 비교 가능하여 빠른 성능
- 그러나 '<','>'와 같은 비교연산자 사용 불가, compareTo()사용해야 한다.
## 2.4. java.lang.Enum
- 모든 열거형의 조상
### 2.4.1. 메서드
- Class\<E> getDeclaringClass() : 열거형의 Class객체를 반환
- String name() : 열거형 상수의 이름을 문자열로 반환
- int ordinal() : 열거형 상수가 정의된 순서를 반환(0부터 시작)
- T valueOf(Class\<T> enumType, String name) : 지정된 열거형에서 name과 일치하는 열거형 상수를 반환
+ 컴파일러가 모든 열거형에 자동적으로 추가해주는 메서드
    + static E[] values() : 열거형 Direction에 정의된 모든 상수를 출력하는데 사용
    + static E valueOf(String name) : 열거형 상수의 이름으로 문자열 상수에 대한 참조를 얻을 수 있게 함
```
Direction[] dArr = Direction.values();

Direction d = Direction.valueOf("WEST");
sout(d); //WEST
sout(Direction.WEST == Direction.valueOf("WEST")); //true
```
## 2.5. 열거형에 멤버 추가하기
- Enum클래스에 정의된 oridnal()이 열거형 상수가 정의된 순서를 반환하지만, 이 값을 열거형 상수의 값으로 사용하지 않는 것이 좋다. -> 내부적인 용도임
- 열거형 상수의 값이 불규칙적인 경우에는 이름 옆에 원하는 값을 괄호()와 함께 적기
```
enum Direction {EAST(1), SOUTH(5), WEST(-1), NORTH(10)}
```
- 다른거 추가할 때 ';' 빠트리면 안됨.
- 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해 주어야 한다. 먼저 열거형 상수를 모두 정의한 다음 다른 멤버들을 추가해야 한다.(479p)
- 열거형 객체의 생성자는 묵시적으로 private이다.
***
# 3. 에너테이션
## 3.1. 에너테이션?
- 소스코드에 대한 문서를 따로 만들기보다 소스코드와 문서를 하나의 파일로 관리하는 것이 낫지 않을까?
    - 주석에 소스코드 정보 저장
    - 소스코드의 주석으로부터 HTML문서를 생성하는 프로그램(javadoc.exe) 만들어서 사용
- 481p 모든 애너테이션의 조상인 Annotation 인터페이스 소스
    - '/**'로 시작하는 소스코드, '@'이 붙은 태그들
    - 미리 정의된 태그들을 이용해 주석 안에 정보 저장
    - javadoc.exe가 이 정보를 읽어서 문서를 작성하는데 사용
+ 이 기능을 응용!
    + 프로그램의 소스코드 안에 **다른 프로그램을 위한 정보**를 **미리 약속된 형식으로 포함**시킨 것이 애너테이션이다.
    + 주석처럼 **프로그래밍 언어에 영향x**, 다른 프로그램에게 **유용한 정보를 제공**
        + 프로그램에게 알리는 역할을 할 뿐, 메서드가 포함된 프로그램 자체에는 아무런 영향을 끼치지 않는다.
+ JDK에서 제공하는 표준 애너테이션은 주로 컴파일러를 위한 것(컴파일러에게 유용한 정보 제공)
+ 메타 애너테이션도 제공
+ 약속된 정보를 제공한다.
## 3.2. 표준 애너테이션
- 기본 제공 애너테이션은 몇 개 없음
- 메타 애너테이션 : 애너테이션을 정의하는데 사용되는 애너테이션
    - *가 붙은 것은 메타 애너테이션이다.
### 3.2.1. @Override
- 컴파일러가 오버라이딩 할때 일치하는 메서드가 조상에 있는지 점검해줌
- 없어도 되지만, 알아내기 어려운 실수를 미연에 방지해주므로 반드시 붙이기
### 3.2.2. @Deprecated
- 새 버전이 나올때 기존 기능을 대체할 것이 생겨도 함부로 삭제하기는 어려움
- 다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미
- 예시로 java.util.Date 클래스의 대부분의 메서드에 붙음
### 3.2.3. @FunctionalInterface
- 함수형 인터페이스 선언 시 붙이면 컴파일러가 올바르게 선언했는지 확인.
- 실수 미연에 방지하도록 반드시 붙이기
- (함수형 인터페이스는 추상 메서드가 하나여야함)
### 3.2.4. @SuppressWarnings
- 컴파일러가 보여주는 경고메세지가 나타나지 않게 억제
- 발생할 것을 알면서도 묵인해야할 경고들이 있을때 사용. 다른 경고들을 놓치기 쉽기 때문
- 주요 경고들 
    - deprecation
    - unchecked : 지네릭스 타입을 지정하지 않았을 때
    - rawtypes : 지네릭스 사용하지 않아서
    - varargs : 가변인자의 타입이 지네릭 타입일 때 발생하는 경고를 억제할 때
- 사용 : 애너테이션 뒤에 괄호 안에 문자열 지정
```
@SuppressWarnings("unchecked")      //지네릭스와 관련된 경고를 억제
ArrayList list = new ArrayList();   //지네릭 타입 미지정
list.add(obj);                      //여기서 경고 발생하지만 억제됨
```
## 3.3. 메타 에너테이션
- java.lang.annotation 패키지에 포함
- 애너테이션에 붙이는 애너테이션
- 적용대상이나 유지기간등을 지정하는데 사용
- @Target, @Documented, @Inherited, @Retention, @Repeatable

### 3.3.1. @Target (489p)
- 애너테이션이 **적용가능한 대상을 지정**하는 역할
- 여러 개의 값을 지정할 때는 배열에서처럼 괄호{} 사용
- 지정 가능한 애너테이션 적용대상의 종류는 java.lang.annotation.ElementType이라는 열거형에 정의되어 있음
    - static import문을 사용하면 편리하게 사용 가능
### 3.3.2. @Retention
- **유지 기간을 지정**하는데 사용
- 유지 정책 종류
    - SOURCE : 소스 파일에만 존재. 클래스 파일에는 존재하지 않음
        - 컴파일러 직접 작성아니면 필요없다.
    - CLASS : 클래스 파일에 존재. 실행시에 사용 불가. 기본값
        - 디폴트지만 실행 시 정보를 얻을 수 없어 잘 안씀
    - RUNTIME : 클래스 파일에 존재. 실행시에 사용가능
        - 'RUNTIME' 사용 시 실행 시에 리플렉션을 통해 클래스파일에 저장된 애너테이션의 정보를 읽어서 처리할 수 있다.
- @Override나 @SuppressWarnings처럼 컴파일러가 사용하는 애너테이션은 유지 정책이 'SOURCE'이다.
- @FunctionalInterface는 컴파일러가 체크해주지만 실행 시에도 사용되므로 'RUNTIME'사용. 
### 3.3.3. @Documented
- 애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함
- 자바에서 제공하는 기본 애너테이션 중 @Override 와 @SuppressWarnings를 제외하고 모두 이거 붙음
### 3.3.4. @Inherited
- 애너테이션이 자손 클래스에 상속되도록 함
    -> @Inherited가 붙은 애너테이션을 조상 클래스에 붙이면, 자손 클래스도 붙은 것으로 인식
### 3.3.5. @Repeatable
- 보통 하나의 대상에 한 종류의 애너테이션을 붙이는데, @Repaeatable이 붙은 애너테이션은 여러 번 붙일 수 있다.
```
@Repeatable(ToDos.class)
@interface ToDo{
    String value();
}
```
- 위와 같이 @ToDo라는 애너테이션이 정의되어 있을 때, 클래스에 @ToDo를 여러번 붙이는 것이 가능하다.
- 일반적인 애너테이션과 달리 같으 이름의 애너테이션이 하나의 대상에 여러 번 적용될 수 있기 때문에, 이 애너테이션들을 하나로 묶어서 다룰 수 있는 애너테이션도 추가로 정의해야 한다.
```
@interface ToDos{ // 여러 개의 ToDo애너테이션을 담을 컨테이너 애너테이션 ToDos
    ToDo[] value(); //ToDo애너테이션 배열타입의 요소를 선언. 이름이 반드시 value이어야 함
}

@Repeatable(ToDos.class)
@Interface ToDo{
    String value();
}
```
## 3.4. 애너테이션 타입 정의하기
- @ 기호를 붙이는 것을 제외하면 인터페이스 정의하는 것과 동일
```
@interface 애너테이션이름{
    타입 요소이름();
    ...
}
```
- 엄밀히 @Override는 애너테이션, 'Override'는 애너테이션의 타입이다.
- 애너테이션도 상수를 정의할 수 있지만, 디폴트 메서드는 정의할 수 없음

## 3.5. 애너테이션 요소
- 애너테이션 내에 선언된 메서드를 **애너테이션의 요소**라고 함
- 애너테이션의 요서는 **반환값이 있고** **매개변수는 없는** 추상 메서드의 형태에 상속을 통해 구현하지 않아도 됨
- 적용할 때 이 요소들의 값을 **빠짐없이 지정**해주어야 한다.(이름 같이 적어서 순서는 무관) - 494p!!
- 기본값이 있는 요소는 애너테이션을 적용할 때 값을 지정하지 않으면 기본값이 사용된다.
```
@interface TestInfo(){
    int count default 1;
}
@TestInfo //@TestInfo(count=1)과 동일
public class NewClass{...}
```
- 애너테이션 요소가 **오직 하나**뿐이고 이름이 `value`인 경우, 애너테이션을 적용할 때 요소의 이름을 생략하고 **값만 적어도** 된다.
```
@interface TestInfo(){
    String value();
}

@TestInfo("passed") //@TestInfo(vlaue="passed")와 동일
class NewClass{...}
```

- 요소 타입이 **배열**인 경우, 괄호{}를 사용해서 여러 개의 값을 지정할 수 있다.
```
@inteface TestInfo(){
    String[] testTools();
}

@Test(testTools={"JUnit", "AutoTester"})
@Test(testTools="JUnit")                //하나일때 괄호 생략 가능
@Test(testTools={})                     //값이 없을때는 괄호 반드시 필요
```

-기본값 지정할 때 마찬가지로 괄호{} 사용 가능
```
@inteface TestInfo(){
    String[] info() default {"aaa","bbb"};
    String[] info2() default "ccc";
}

@TestInfo           //TestInfo(info={"aaa","bbb"}, info2="ccc")
@TestInfo(info2={}) //TestInfo(info={"aaa","bbb"}, info2={})
class NewClass {...}
```

- 요소의 타입이 배열일 때도 요소의 이름이 `value`이면 요소 이름 생략 가능
```
@interface SuppressWarning{
    String[] value();
}

@SuppressWarnings({"deprecation", "unchecked"})
class NewClass{...}
```

## 3.6. 모든 애너테이션의 조상
- Annotation
- 상속이 허용되지 않아 명시적으로 지정 불가(extends)
- 일반적인 인터페이스로 정의되어 있음
```
package java.lang.annotation;

public interface Annotation {
    boolean equals(Object obj);
    int hashCode();
    String toString();

    Class<? extends Annotation> annotationType();
}
```
- 위처럼 정의되어 있기 때문에 equals(), hashCode(), toString()과 같은 메서드 호출 가능

## 3.7. 마커 애너테이션
- 값을 지정할 필요가 없는 경우, 애너테이션의 요소를 하나도 정의하지 않을 수 있다.
- Serializable, Cloneable처럼 요소가 하나도 정의되지 않은 애너테이션

## 3.8. 애너테이션 요소의 규칙
    1. 요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용된다.
    2. () 안에 매개변수를 선언할 수 없다.
    3. 예외를 선언할 수 없다.
    4. 요소를 타입 매개변수로 정의할 수 없다.
- 예시 499p
### 활용 예제
- 모든 클래스 파일은 클래스 로더에 의해 메모리에 올라갈 때, 클래스에 대한 정보가 담긴 객체를 생성하는데, 이 객체를 **클래스 객체**라고 한다.
- **클래스 객체**에는 해당 클래스에 대한 모든 정보가 있고, 애너테이션의 정보도 포함되어 잇다.
- getAnnotation() :  모든 애너테이션을 배열로 받아올 수 있고, 매개변수로 정보를 얻고자 하는 애너테이션을 지정할 수 있다.

### 연습문제
- 12-3 다시보기!!
- 12-4도 다시보기 : 지네릭 메서드와 컬렉션 클래스에 붙이는 지네릭스를 구분하자
    - **static멤버**에는 타입 매개변수를 사용할 수 없지만, **메서드**에 지네릭 타입을 선언하고 사용하는 것은 가능하다.


### 의문
리스트에 다양한 자식 클래스들 처리도 컴파일러가 각각 해주는건지?