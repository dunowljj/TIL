람다와 스트림
=====
# 1.1. 람다식(Lamda expression)
- 메서드를 하나의 식으로 표현한 것
- 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, **익명 함수**라고도 한다.
```
int[] arr = new int[5]
Arrays.setAll(arr, (i) -> (int)(Math.random()*5+1);
```
- 간결하고 이해하기 쉬움
- 클래스 생성, 객체 생성 과정 없이 호출
- 매개변수로 전달, 메서드 결과로 반환 모두 가능
## 1.2. 람다식 작성
- 메서드에서 **이름과 반환타입을 제거**하고 매개변수 선언부와 몸통{} 사이에 `->` 추가
- 반환값이 있는 경우 return문 대신 식으로 대체 가능
- 문장이 아닌 '식'이므로 `;` 붙이지 않는다
- **매개변수의 타입**이 추론 가능한 경우 생략 가능. 대부분의 경우 생략 가능
- 매개변수가 하나뿐인 경우 괄호`()` 생략 가능, 매개변수의 타입이 있으면 괄호`()`를 생략할 수 없다.
- 괄호`{}`안의 문장이 하나일 때 괄호`{}`생략 가능 문장의 끝에 `;`붙어서는 안됨.
```
(String name, int i) -> System.out.println(name+"="+i)
```
- 괄호 안의 문장이 **return문**일 경우 괄호`{}` 생략 불가
- 예시는 554p
## 1.3. 익명 함수? 익명 객체
- 람다식은 익명 클래스의 객체와 동등
- 익명 객체를 어떻게 호출할 것인가?
```
타입 f = (int a, int b) -> a>b ? a:b;
```
- f의 타입은 클래스 또는 인터페이스가 가능하며 람다식과 동등한 메서드가 정의되어 있어야 한다.
## 1.4. 함수형 인터페이스
### 1.4.1. 설명
- 오직 하나의 추상메서드만 정의되어 있어야 한다.
- 하나의 메서드가 선언된 인터페이스를 정의하여 람다식을 다루는 것이 자연스러움
```
interface MyFunction {
    public abstract int max(int a, int b);
}

MyFunction f = (int a, int b) -> a>b ? a:b;
int big = f.max(5,3); //익명 객체의 메서드를 호출
```
### 1.4.2. 함수형 인터페이스 타입의 매개변수, 반환 타입
- (557p)메서드의 매개변수가 MyFunctiona 타입이면, 이 메서드를 호출할 때 람다식을 참조하는 함조 변수를 매개변수로 지정해야 한다는 의미
- 참조변수 없이 직접 람다식을 매개변수로 지정하는 것도 가능
- 메서드 반환 타입이 함수형 인터페이스타입이라면, 함수형 인터페이스의 추상메서드와 동등한 람다식을 가리키는 참조변수를 반환하거나 람다식을 직접 반환할 수 있음
+ 메서드를 통해 람다식을 주고받을 수 있다.

## 1.5. java.util.function패키지
- 일반적으로 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의
- 매번 함수형 인터페이스 정의하지말고 가능한 활용 -> 통일, 재사용성, 유지보수
- 매개변수와 반환값의 유무에 따라 4개의 함수형 인터페이스가 정의되어 있음
- java.lang.Runnable, Supplier\<T>, Consumer\<T>, Function\<T,R>, Predicate\<T>
- run / get / accept / apply / test
- `Predicate`는 `Function`의 변형으로 반환타입이 boolean이라는 것만 다르다.
### 1.5.+ 매개변수가 두 개인 함수형 인터페이스
- 이름 앞에 접두사 'Bi'가 붙는다
- BiConsumer\<T,U>, BiPredicate\<T,U>, BiFunction\<T,U,R>
- 두 개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하다면 **직접** 만들어서 써야한다.
### UnaryOperator, BinaryOperator
- Function의 또 다른 변형
- **매개변수의 타입과 반환타입의 타입이 모두 일치**한다는 점만 제외하고는 Function과 같다.
- UnaryOperation\<T>, BinaryOperator\<T>
- apply / apply
## 1.6. Predicate의 결합
- &&(and), ||(or), !(not)으로 여러 조건식을 연결하여 식을 만들듯이, 여러 Predicate를 and(), or(), negate()로 연결해서 하나의 새로운 Predicate로 만들 수 있다.
```
Predicate<Integer> p = i -> i < 100;
Predicate<Integer> q = i -> i < 200;
Predicate<Integer> r = i -> i%2 ==0;
Predicate<Integer> notP = p.negate();       // i <=100

// 100 <= i && (i<200 || i%2==0)
Predicate<Integer> all = notP.and(q.or(r));
System.out.println(all.test(150));          //true
```
### isEqual()
- static메서드인 isEqual()은 두 대상을 비교하는 Predicate를 만들때 사용
- isEqual()은 매개변수로 비교대상을 하나 지정, 또 다른 비교대상은 test()의 매개변수로 지정
- 납득 잘 안감 562 다시보기
## 1.7. 컬렌션 프레임웍과 함수형 인터페이스
- 564p메서드
- forEach,removeAll,replaceAll, compute, merge, removeIf ...
## 1.8. 메서드 참조
- 람다식이 하나의 메서드만 호출하는 경우 `메서드 참조(method reference)`로 간략히 할 수 있음
```
//보통
Function<String, Integer> f = (String s) -> Integer.parseInt(s);

//메서드 참조
Function<String, Integer> f = Integer::parseInt;
```
- 컴파일러는 생략된 부분의 우변의 parseInt메서드의 선언부로부터, 또는 좌변의 Function인터페이스에 지정된 지네릭 타입으로부터 생략된 부분을 쉽게 알아낼 수 있다.
- 하나의 메서드만 호출하는 람다식은 `클래스이름::메서드이름`,`참조변수::메서드이름`로 표현 가능
## 1.9. 생성자의 메서드 참조
- 생성자를 호출하는 람다식도 메서드 참조로 변환할 수 있다.
```
Supplier<MyClass> s = () -> new MyClass();
Supplier<MyClass> s = MyClass::new;
```
- 매개변수가 있는 생성자라면 매개변수 개수에 맞춰 함수형 인터페이스를 사용하거나 새로 정의하면 된다.
```
Bifunction<Integer, String, MyClass> bf = (i, s) -> new MyClass(i,s);
Bifunction<Integer, String, MyClass> bf = MyClass::new;
```
- 배열의 경우
```
- Function<Integer, int[]> f = x -> new int[x];
- Function<Integer, int[]> f2 = int[]::new;
```
***

# 2. 스트림
## 2.1. 필요성
- for, iterator -> 길고 알아보기 힘듦. 재사용성 떨어짐.
- 데이터 소스마다 다른 방식 사용해야함 -> 각 컬렉션 클래스들 같은 기능의 메서드 중복 정의 
## 2.2. 설명
- 데이터 소스를 추상화 -> 소스가 무엇이든 간에 같은 방식으로 다룸, 재사용성 증가
- 모든 같은 방식으로 데이터를 다룰 수 있음
- 데이터 소스가 다른 두 스트림이 정렬하고 출력하는 방법은 완전히 동일하다.(568p)
## 2.3. 특징
### 데이터 소스 변경x
- 데이터 소스를 변경하지 않고 읽기만 함
### 일회용
- iterator로 컬렉션의 요소를 모두 읽고 나면 다시 사용할 수 없듯이, 스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다. 필요시 다시 생성해야 함.
### 내부반복
- 작업을 내부 반복으로 처리한다. 반복문을 메서드 내부에 숨겼다는 것을 의미
### 지연된 연산
- 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다. `distinct()`나 `sort()`같은 중간 연산 호출해도 즉각수행 x, 최종 연산이 수행되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모
### Stream<Integer>와 IntStream
- 기본적으로 Stream\<T>
- 오토박싱&언박싱 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림, IntStream, LongStream, DoubleStream이 제공
- 일반적으로 후자가 더 효율적, int값 작업에 유용한 메서드도 포함
### 병렬 스트림
- 병렬처리가 쉬운 장점 -> 내부적으로 Java에서 제공하는 fork&join프레임웍
- 스트림에 `parallel()`이라는 메서드만 호출해서 병렬 연산을 지시하면 됨
- `sequential()` 병렬로 처리되지 않게 하기 위해 사용 -> `parallel()` 취소할 때만 사용
***
# 3. 스트림 만들기
## 3.1. 컬렉션
- Collection에 stream()이 정의되어 있음 -> List,set도 스트림 생성 가능
- `Stream<E> stream()`
```
List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<Integer> intStream = list.stream();

intStream.forEach(System.out::println);
```
- `forEach`의 경우 요소를 소모하기 때문에 한번 더 출력하려면 스트림 새로 생성해야 함
    - 스트림의 요소가 소모되는 것이지 소스의 요소가 소모되지 않음
    -> 같은 소스로부터 다시 생성 가능
## 3.2. 배열
- 배열을 소스로 하는 스트림을 생성하는 메서드는 Stream과 Array에 static으로 둘다 있음
```
Stream<T> Stream.of(T... vlaues) //가변인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive)
```
- 기본형 배열을 소스로 하는 스트림 생성하는 메서드도 존재. 위에 코드에서 Stream -> intStream 으로 바꾸면 됨(long,double)
- 문자열 스트림 생성도 있음(572p)
## 3.3. 임의의 수
- Random클래스에 포함된 인스턴스 메서드들
```
IntStream ints()
LongStream longs()
DoubleStream doubles()
```
- 해당 타입의 난수들로 이루어진 스트림을 반환
- 이 메서드들은 **무한 스트림**을 반환함 -> `limit()`으로 크기 제한해줘야
```
IntStream intStream = new Random().ints();
intStream.limit(5).forEach(System.out::println);
```
- 메개변수로 스트림 크기를 지정해서 **유한 스트림**을 생성해서 반환할 수도 있다.(limit 불필요)
## 3.4. 특정 범위의 정수
- IntStream, LongStream은 지정된 범위의 연속된 정수를 스트림으로 생성해서 반환하는 메서드들을 가지고 있다.
```
IntStream IntStream.range(int begin, int end)
IntStream IntStream.rangeClosed(int begin, int end)
```
- `range()`는 끝 범위 포함x, `rangeClosed()`는 끝 범위 포함
## 3.5. 람다식 iterate(), generate()
```
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
static <T> Stream<T> generate(Supplier<T> s)
```
- `iterate()`와 `generate()`는 람다식을 매개변수로 받아서, 이 람다식에 의해 계산되는 값들을 요소로 하는 무한 스트림을 생성
- `iterate()`는 **씨앗값(seed)**으로 지정된 값부터 시작해서, 람다식 f에 의해 계산된 결과를 **다시 seed값**으로 해서 계산을 반복(575p)
- `generate()`에 정의된 매개변수 타입은 `Supplier<T>`이므로 매개변수가 없는 람다식만 허용된다.
- `iterate()`와 `generate()`에 의해 생성된 스트림을 기본형 스트림 타입의 참조변수로 다룰 수 없다.
- 굳이 필요하다면 `mapToInt()`와 같은 메서드로 변환
