# 0️⃣ 학습테스트 실습

# String 클래스에 대한 학습테스트

## 🌵 메서드, 에너테이션 모음

### JUnit

✓ assertj

- contains, containsExactly : 배열로 받을때 사용, 순서까지 확인하는게 Exactly
- @Nested, @DisplayName("")  : 가독성, 정리
- assertThatThrownBy()
- assertThatExceptionOfType()
- isInstanceOf(), isThrownBy()
- hasMessageContaining() , withMessagingMatching()
- contains(), containsExactly()

- 자주 발생하는 Exception의 경우 Exception별 메서드 제공하고 있음
    - assertThatIllegalArgumentException()
    - assertThatIllegalStateException()
    - assertThatIOException()
    - assertThatNullPointerException()

###  String

- split
- substring
- charAt

* * *

# Set Collection에 대한 학습 테스트
>[AssertJ Exception Assertions](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#exception-assertion)  
>[Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)
  
## Parameterized Test 기능
Guide to JUnit 5 Parameterized Tests 내용  
한 개의 테스트 메서드를 다른 파라미터로 여러번 테스트 할 수 있다.

### 1. Overview의 예시
- @ValueSource 배열에 있는 값을 숫자 파라미터에 넣어서 `isOdd`  메서드가 6번 실행된다.
- 인수의 소스가 int 배열이다.
- 접근하는 방법이 숫자 매개변수이다.
- String도 가능

### 문제
- 사용 가능 타입이 제한되어 있음
- 메서드 하나에 한개 인수밖에 통과 못함
- null을 통과 못한다.

### @NullSource , @EmptySource
- null 하나 통과 가능 @NullSource
- primitive 타입은 null 값을 받을 수 없기 때문에, @NullSource도 못씀
- @EmptySource
- @NullAndEmptySource도 있음
- @EmptySource와 함께 구성된 에너테이션은? Strings, Collections, arrays에 적용된다.
- 빈 문자열의 변화를 통과하기 위해? 아랫부분의 코드처럼 다 합쳐서도 씀


### Enum 사용 가능
- Enum도 때려박을 수 있다.
- names로 일치하는 것만, mode와 EXCLUDE로 제외하고 사용 가능
- 나중에 enum 넣어서 돌려보자
- names, MATCH_ANY로 정규표현식도 가능



### @CsvSource @CsvFileSource

✓ CSV란?

**CSV**(영어: comma-separated values)는 몇 가지 필드를 쉼표(,)로 구분한 텍스트 데이터 및 텍스트 파일이다. 확장자는 . **csv**이며 MIME 형식은 text/**csv**이다. comma-separated variables라고도 한다. - 위키 백과 -

toUpperCase() 메서드의 경우 예측된 값이 대문자 값이다. @ValueSource는 매개변수로 넘길놈만 알려주기 때문에 그것만으로 테스트하긴 무리다.매개변수는 여러개 넘길 수 있지만, 테스트 될 값도 따로 지정하기 위해서 @CsvSource가 존재한다.

본래 이런 절차가 필요하다.

- 입력값과 예상값을 테스트 메서드에 통과시킨다.
- 실제 값과 입력값을 계산한다.
- 실제값과 예측값을 assert 한다.

### Csv 사용
이름과 걸맞게 ,로 값을 구분하는 배열을 @CsvSource에 입력한다.
위에 두개의 코드 중 아래의 코드는 delimiter를 지정해서 , 대신 : 을 사용했다.
***

## 🌵 메서드, 에너테이션 모음

### JUnit

- @ParameterizedTest : 이거 쓸 때 @Test는 빼야한다 안그럼 테스트 각각 두번된다.
- @ValueSource : 파라미터에 넣을 값들 지정
- @NullSource ,@EmptySource
- @NullAndEmptySource
- @EnumSource
    - (month.class)
    - (values = month.class, names = {"SEPTEMBER", "DECEMBER"} )
    - (values = month.class, names = {"SEPTEMBER", "DECEMBER"}, mode = EnumSource.Mode.EXCLUDE )
    - (values = month.class, names =".+BER" , mode = EnumSource.Mode.MATCH_ANY )
        - names, mode, mode.EXCLUDE, mode.MATCH_ANY
- @CsvSource(value = {":", ":"}, delimeter= ':')