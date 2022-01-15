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
## 3.6. 파일과 빈 스트림
### 파일
- java.nio.file.Files 파일을 다루는데 필요한 유용한 메서드 제공
```
Stream<Path> Files.list(Path dir)
```
- `list()`는 지정 디렉토리에 있는 파일의 목록을 소스로 하는 스트림을 생성해서 반환
```
Stream<String> Files.lines(Path path)
Stream<String> Files.lines(Path path, Charset cs)
Stream<String> lines()
```
- **한 행을 요소**로 하는 스트림을 생성하는 메서드도 있다. 3번째 것은 `BufferedReader클래스`에 속한 것으로 **파일** 뿐만 아니라 **다른 입력대상**으로부터도 데이터를 **행단위**로 읽어올 수 있다.
### 빈 스트림
- 요소가 하나도 없는 빈 스트림 생성 가능
- 연산 수행 결과가 하나도 없을때 null보다 빈 스트림을 반환하는 것이 낫다.
```
Stream emptyStream = Stream.empty();
long count = emptyStream.count();
```
- `empty()` : 빈 스트림을 생성해서 반환
- `count()` : 스트림 요소의 개수를 반환 -> 위 문장은 0
***

# 4. 스트림의 연산
## 4.1. 설명
- 중간 연산 : 연산결과를 스트림으로 반환, 중간 연산을 연속해서 연결 가능
    - stream.distinct().limit(5).sorted().
- 최종 연산 : 스트림의 요소를 소모하면서 연산을 수행. 단 한번만 연산이 가능
    - forEach(~)
## 4.2. 중간연산
### skip(), limit()
- 스트림 일부를 잘라낼 때 사용
```
Stream<T> skip(long n)
Stream<T> limit(long maxSize)
```
- `skip()` : 처음 요소 n개를 건너뜀
- `limit()` : 스트림 요소 개수를 제한
- 기본형 스트림에도 정의되어 있고, 반환형은 기본형
### filter(), distinct()
```
Stream<T> filter(Predicate<? super T> predicate)
Stream<T> distinct()
```
- `distinct()` : 스트림에서 중복된 요소 제거
- `filter()` : 조건에 맞지 않는 요소 걸러냄
### sorted()
```
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```
- 스트림을 정렬할 때 사용
- Comparator 미지정 시 요소의 기본 정렬 기준으로 정렬
- Comparator대신 int값을 반환하는 람다식을 사용하는 것도 가능
- 582p 하단 다양한 정렬 방법
- naturalOrder, reserved, CASE_INSENSITIVE_ORDER ...
### Comparator의 메서드
- 많은 static, 디폴트 메서드들 모두 `Comparator<T>`를 반환, 가장 기본적인 메서드는 `comparing()`
```
comparing(Function<T, U> keyExtractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)
```
- 스트림의 요소가 Comparable을 구현한 경우, 매개변수 하나짜리를 사용하면 되고 그렇지 않은 경우 매개변수로 정렬기준(Comparator)을 따로 지정해 줘야 한다.(582~583 참조)
```
comparingInt/Long/Double(ToInt/Long/DoubleFuction<T> keyExtractor)
```
- 비교대상이 기본형인 경우 `comparing()`대신 위의 메서드를 사용하면 오토박싱과 언박싱 과정이 없어서 더 효율적이다.
- 정렬 조건 추가 시 `thenComparing()` 사용
```
studentStream.sorted(Comaprator.comparing(Student::getBan)
    .thenComparing(Studnet::getTotalScore)
    .thenComparing(Student::getName))
    .forEach(System.out::println);
```
### map()
- 원하는 필드만 뽑아내거나 특정 형태로 변환해야 할 때 `map()` 유용
```
Stream<R> map(Function<? super T,? etends R> mapper)
```
- 파일이름만 간단히 뽑기
```
Stream<String> filenameStream = fileStream.map(File::getName);
filenameStream.forEach(System.out::println);
```
### peek()
- 연산과 연산 사이에 올바르게 처리되었는지 확인하고 싶다면 사용
- 중간 연산이라 스트림의 요소를 소모하지 않음
### flatMap()
- 스트림의 타입이 `<Stream<T[]>`인 경우 `Stream<T>`로 변환해야할 때 사용
- Arrays.stream(T[])를 함께 사용한다고 치면,
    - `map()` 사용 시
        ```
        Stream<Stream<String[]> strStrStrm = strArrStrm.map(Arrays::stream);
        ```
    - `map()`자리에 `flatmap()` 사용 시
        - `Stream<String>` 형태 반환
- 589p 예제 한번 더보기
***

## 4.3. Optional\<T>
### 4.3.1. 설명
- JDK 1.8부터 추가
- T타입의 객체를 감싸는 래퍼 클래스
- 모든 타입 객체를 담을 수 있다.
```
public final class Optional<T> {
    private final T value;
}
```
- `Optional객체`에 담아서 반환을 하면 정의된 메서드로 간단히 null체크 가능
- NullPointerException이 발새하지 않는 보다 간결하고 안전한 코드 작성 가능
### 4.3.2. 생성
- 생성 시`of()` 또는 `ofNullable()` 사용
- `of()`는 null일 때 NullPointerException이 발생 -> null 가능성 있으면 `ofNullable()` 사용
```
//생성
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = Optional.of(new String("abc"));

//null 차이
Optional<String> optVal =  Optional.of(null);
Optional<String> optVal =  Optional.ofNullable(null); //ok

//초기화
Optional<String> optVal =  null; //가능하나 바람직x
Optional<String> optVal =  Optional.<String>empty();//빈 객체 초기화
```
### 4.3.3. `Optional<T>` 객체의 값 가져오기
- `get()` : 저정된 값 가져옴 null 이면 NoSuchElementException
- `orElse()` : null일때 대체할 값 지정
```
Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get();
String str2 = optVal.orElse("");
```
- `orElse()`변형
    - `orElseGet()` : null 대체 람다식 지정
    - `orElseThrow()` :null일때 지정된 예외 발생
```
T orElseGet(Supplier<? extends T> other)
T orElseThrow(Supplier<? extends x> exceptionSupplier)

//사용
String str3 = optVal2.orElseGet(String::new);
String str4 = optVal2.orElseThrow(NullPointerException::new);
```
- `isPresent()` : Optional객체가 null일 경우 false 아니면 true 반환
- `ifPresent(Consumer<T> block)` : 값이 있으면 람다식 실행, 없으면 실행x
```
Optional.ofNullable(str).ifPresent(System.out::println)
```
### 4.3.4. OptionalInt, OptionalLong, OptionalDouble
- Optional에 저장된 값을 꺼낼 때 사용하는 메서드는 이름이 조금씩 다르다는 것에 주의(593참조)
- 기본형 int의 기본값은 0이기 때문에 OptionalInt에 저장되는 값은 0이다.
-> empty()로 선언한 객체와 0 저장한 객체 isPresent()로 구분 가능
```
OptionalInt opt = OptionalInt.of(0);
OptionalInt opt2 = OptionalInt.empty();

System.out.println(opt.isPresent());    //true
System.out.println(opt.isPresent());    //false
System.out.println(opt.equals(opt2));   //false
```
***
## 4.4. 스트림의 최종 연산
### 4.4.1. forEach()
```
void forEach(Conseumer<? super T> action)
```
### 4.4.2. 조건검사
- `allMatch()` : 모두 일치하면 참
- `anyMatch()` : 하나라도 일치하면 참
- `noneMatch()` : 모두 불일치 시 참

```
boolean allMatch(Predicate<? super T> predicate)
boolean anyMatch(Predicate<? super T> predicate)
boolean noneMatch(Predicate<? super T> predicate)
```
- `findFirst()` : 스트림 요소 중 일치하는 첫 번째 것 반환
    - 주로 `filter()`와 함께 조건에 맞는 스트림 요소 확인에 사용
    - 병렬 스트림의 경우 대신에 `findAny()`사용해야 함
```
Optional<T> findFirst() //조건 일치하는 첫 요소 반환
Optional<T> findAny() //조건 일치하는 요소 하나 반환(병렬스트림)
```
### 4.4.3. reduce()
- 스트림의 요소를 줄여나가면서 연산을 수행하고 최종결과를 반환
- 스트림의 요소가 하나도 없는 경우 초기값이 반환되므로 반환 타입이 T이다.
```
T reduce(T identity, BinaryOperator<T> accumulator)
U reduce(U identity, BinaryOperator<U,T,U> accumulator, BinaryOperator<U> comnbiner))
```
- `combiner`는 병렬 스트림에 의해 처리된 결과를 합칠 때 사용하기 위해 사용
- `count(), sum()`등은 내부적으로 모두 `reduce()`사용
```
int count = intStream.reduce(0, (a,b) -> a + 1);
int sum = intStream.reduce(0, (a,b) -> a + b);
int max = intStream.reduce(Integer.MIN_VALUE, (a,b) -> a>b  a:b);
int min = intStream.reduce(Integer.MAX_VALUE, (a,b) -> a>b  a:b);
```
### 4.4.4. collect()
- 복잡하지만 유용
    • `collect()` : 스트림의 최종연산, 매개변수로 컬렉터를 필요로 함
    • `Collector` : 인터페이스, 컬렉터는 이 인터페이스를 구현해야 함
    • `Collectors` : 클래스, static메서드로 미리 작성된 컬렉터를 제공
```
Object collect(Collector collector)
Object collect(Supplier supplier, BiConsumer accumulator, BiConsumer combiner)
```
### 4.4.5. 스트림을 컬렉션, 배열로 변환
- `toCollection()`에 원하는 컬렉션의 생성자 참조를 매개변수로 넣으면 됨
```
List<String> names = stuStream.map(Student::getName).collect(Collectors.toList());
ArrayList<String> list = names.stream().collect(Collectors.toCollection(ArrayList::new));
```  

- Map은 어떤 필드를 키로 사용할지와 값으로 사용할지를 지정해줘야 한다.
```
Map<String,Person> map = personStream.collect(Collectors.toMap(p->p.getRegid(), p->p));
```
- regId를 키로, 값으로 Person객체를 저장
- p->p 대신 Function.identity 사용 가능
```
Student[] stuNames = studentStream.toArray(Student[]::new); //ok
Stduent[] stuNames = studentStream.toArray(); //error
Object[] stuNames = studentStream.toArray();  //ok
```
### 4.4.6. 스트림의 통계 counting(), summingInt()
- 최종 연산들이 제공하는 통계 정보를 `collect()`로 똑같이 얻을 수 있음
- `groupingBy()`와 함께 사용
### 4.4.7. 스트림을 리듀싱 reducing()
- IntStream에는 매개변수 3개짜리 collect()만 정의되어 있음
    - `boxed()`를 통해 IntStream을 `Stream<Integer>`로 변환해야 매개변수 1개짜리 `collect()`를 쓸 수 있다.
```
Collector reducing(BinaryOperator<T> op)
Collector reducing(T identity, BinaryOperator<T> op)
Collector reducing(U identity, Function<T,U> mapper, BinaryOperator<U> op)
```
- 와일드 카드 생략한 내용
- 세 번째 것은 map()과 reduce()하나로 합친 것 뿐
### 4.4.8. 스트림을 문자열로 결합 joining()
- 문자열 스트림의 모든 요소를 하나의 문자열로 연결해서 반환
- 구분자, 접두사, 접미사 지정 가능
- 스트림의 요소가 String이나 CharSequence의 자손인 경우에만 결합 가능 -> 요소가 문자열이 아닌 경우 먼저 `map()`으로 문자열로 변환
- `map()`없이 스트림에 바로 `joining()`하면, 스트림의 요소에 `toString()`을 호출한 결과를 결합한다.
## 4.5. 스트림의 그룹화와 분할
### 4.5.1. 설명
- 다른 연산 놔두고 왜 쓸까?
- 그룹화 : 스트림의 요소를 **특정 기준**으로 그룹화
- 분할 : 스트림의 요소를 **두 가지**, **지정된 조건**에 일치하는 그룹과 그렇지 않은 그룹으로 분할
```
Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate, Collector downstream)

Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)
```
- 그룹화와 분할의 결과는 Map에 담겨 반환된다.
### 4.5.2. 스트림의 분할 partitioningBy()
```
// 1. 기본 분할
Map<Boolean, List<Student>> stuBySex = stuStream.collect(partitioningBy(Student::isMale));

List<Student> maleStudent = stuBySex.get(true);
List<Stduent> femaleStudent = stuBySex.get(false);
```
- `counting()`추가
```
// 2. 기본 분할 + 통계 정보
Map<Boolen, Long> stuNumBySex =stuStream.collect(partitioningBy(Student::isMale, counting()))l

System.out.println("남학생 수 :"+ stuNumBySex.get(true));
System.out.println("여학생 수 :"+ stuNumBySex.get(false));
```
- 대신 `summingLong()`등 사용 가능
```
Map<Boolean, Optional<Student>> topScoreBySex = stuStream
    .collect(
        partitioningBy(Student::isMale, 
            maxBy(comparingInt(Student::getScore))
        )
    );
```
- `maxBy()`의 반환타입이 `Optional<Student>`라서 출력 시 Optional의 toString() 값이 나온다.
- `Optional<Student>`가 아닌 `Student`로 반환 결과를 얻으려면 `collectiongAndThen()`과 `Optional::get`을 함께 사용하면 된다.(607)
+ 성적이 150점 아래인 학생들을 불합격 처리? -> `partitioningBy()` 한번 더 사용해서 이중 분할하면 된다.
```
Map<Boolean, Map<Boolean, List<Student>>> failedStuBySex = stuStream
    .collect(
        partitioningBy(Student::isMale,
            partitioningBy(s -> s.getScore() < 150)    
        )
    );
List<Student> failedMaleStu = failedStuBySex.get(true).get(true);
List<Student> failedFemaleStu = failedStuBySex.get(false).get(true);
```

### 4.5.3. 스트림의 그룹화 groupingBy()
```
Map<Integer, List<Student>> stuByBan = stuStream
    .collect(groupingBy(Student::getBan)); //toList() 생략가능
```
- `groupingBy()`로 그룹화를 하면 기본적으로 `List<T>`에 담는다. 원한다면, toList()대신 toSet()이나 toCollection(HashSet::new)등 사용 가능
    - 단 Map의 지네릭 타입 적절히 변경해야 함(611p)
- `groupingBy()`여러 번 사용하여 **다수준 그룹화** 가능.
- `mapping()`
- 615~616참조
### 그룹화
- 반환되는 Map의 키 값 쌍을 잘 보자.
- key -> 정렬 기준, groupingBy() 괄호 안의 반환값 기준 정렬
- value -> 변수의 value값, List,Set 등 가능, Long 등 쓰고 gB뒤에 counting() 같은 메서드 사용 가능
- **표로 정리된 스트림 변환 618p**


### 예제
- 14-3 다시 풀기 - 함수형 인터페이스 종류
