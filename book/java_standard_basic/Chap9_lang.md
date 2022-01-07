java.lang패키지와 유용한 클래스
======

# java.lang
- java.lang 패키지는 가장 기본인 클래스들 포함 -> import 없이 사용
# 1. Object 클래스
## 1.1. 설명
- 모든 클래스의 최고 조상
- Object클래스는 멤버변수는 없고 11개 메서드 가짐(324p)
## 1.2. equals()
### 1.2.1. 설명
- 두 객체의 같고 다름을 참조변수의 값으로 판단  
        -> 서로 다른 두 객체 비교 시 항상 false
- 주소값으로 판단하기 때문에 멤버변수와 그것의 값이 같아도 주소가 다르면 false
### 1.2.2. 오버라이딩
- 주소값이 아닌 다른 값으로 비교 기준을 변경 가능
```
class Person{
    long id;
    public boolean equals(Object obj){
        if(obj instanceof Person){
            return id == ((Person)obj).id;
        } else
            return false;
    }
}
...
```
## 1.3. hashCode()
- 해싱 기법에 사용되는 해시 함수
- 데이터를 저장하고 검색하는데 유용
- 저장된 위치를 알려주는 해시코드 반환
- 인스턴스 변수로 객체의 같고 다름 판단하는 경우 -> equals(), hashCode() 오버라이딩 적절히; hashCode()호출 시 결과 해시코드 같아야하니까 왜?
+ String은 이미 오버라이딩 되어 있음
+ System.identityHashCode(Object x)는 객체의 주소값으로 해시코드 생성해서 모든 객체에 항상 다른 해시코드값 반환 보장
## 1.4. toString()
### 1.4.1. 정의
```
public String toString(){
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```
- 그냥 사용 시 클래스 이름과 16진수 해시코드
### 1.4.2. 오버라이딩
- 자손에서 오버라이딩할 때는 조상에 정의된 접근범위보다 같거나 더 넓어야 함
***

# 2. String 클래스
- 문자열을 저장하고 다룸
## 2.1. 정의
```
public final class String implements java.io.Serializable, Comparable {
    private char[] value;
...
```
- 문자형 배열로 정의
- immutable 클래스
## 2.2. 문자열 만들기
    1. 문자열 리터럴 지정
        • 이미 있는 것을 재사용
    2. String클래스의 생성자를 이용
        • 새로운 String인스턴스 생성
        • 같은 문자열을 생성해도 '=='로 비교 시 false
## 2.3. 문자열 리터럴(String 리터럴)
- 자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장
- 같은 내용의 문자열 리터럴은 한번만 저장
- 문자열 리터럴도 String인스턴스
- 변경 불가이니 하나의 인스턴스를 공유하면 됨
- 컴파일 시 한번만 저장 -> 실행 시 String인스턴스가 하나 생성 -> 참조변수들이 모두 String인스턴스 참조
### 리터럴
- 소스코드의 고정된 값, 데이터(값) 자체
## 2.4. 빈 문자열 
 • 길이가 0인 배열
 • 초기화

    String s = null;  --> String s = "";
    char c = '\u0000' --> char c = ' ';

## 2.5. String의 생성자
    • String(String s)
    • String(char[] value)
    • String(StringBuffer buf)
## 2.6. String의 메서드
    • int indexOf(int ch, int pos) -> 지정 위치부터 같은 문자 찾음
    • int lastIndexOf(String str/int ch)
    • boolean contains(CharSequence s)
    • boolean endsWith(String suffix)
    • char charAt(int index)
    • String intern() : 문자열을 상수풀에 등록, 이미 있으면 주소값 반환
    • String replace(CharSequence old, CharSequence nw)
    • String replaceFirst/replaceAll(String regex, String replacement) : 일치하는 것중 첫번째만 변경/ 다 변경
    • String substring(int begin)/(int begin, int end) : 시작 부터 끝 위치 범위 문자열 얻음. 처음 포함, 끝 불포함
    • String toLowerCase()/toUpperCase()
    • String trim : 양쪽 끝 공백만 제거
    • static String valueOf(Object o)/(원시타입들) : 문자열로 변환하여 반환
## 2.7. 문자열과 기본형 간 변환
### 2.7.1.valueOf()
- 빈문자 더하기 보다 valueOf()가 성능이 더 좋음
- Integer.valueOf : 변환 후 오토박싱 -> parseInt를 호출하는 함수라 똑같음. 통일성 부여를 위해 valueOf가 나중에 추가된 것 
### 2.7.2. Integer.parseInt, Double.parseDouble 변환 주의점
- 공백이나 문자 포함 시 NumberFormatException -> trim() 사용하기도 함
- '+', '.' 등 연산기호나 소수점 혹은 float형을 뜻하는 f같은 접미사는 사용 가능
- f같은 경우 자료형 안맞으면 못함. ex) Integer.parseInt는 안됨
## 2.8. java.util.StringJoiner (1.8이상)
    StringJoiner sj = new StringJoiner(",", "[","]");
    ...
    sj.add(str~);
## 2.9. StringBuffer 클래스
### 2.9.1. StringBuffer 정의
```
public final class StringBuffer implements java.io.Serializble{
    private char[] value;
...
}
```
- 생성 시 저정한 문자열 변경 가능
- String 클래스와 유사
### 2.9.2. StringBuffer 생성자
- 인스턴스 생성 시 크기 지정 가능
- 버퍼의 길이를 충분히 잡지 않으면 넘어설 시 추가 작업때문에 효율 감소
- 생성 시 지정 안하면 16개의 문자를 저장하는 버퍼 크기 지정됨
- 버퍼 크기 모자랄 시 내부적으로 버퍼 크기를 증가시킴
- 배열의 길이는 변경될 수 없으므로 새로운 길이의 배열을 생성한 후에 이전 배열의 값을 복사해야함(341p)
### 2.9.3. StringBuffer 변경
- append() : 해당 주소에 문자열을 추가하고 반환타입은 **StringBuffer**로 **자신의 주소**를 반환한다. => 자신의 주소를 반환하기 때문에 **연속적 호출**이 가능하다.
```
//연속적 호출
StringBuffer sb = new StringBuffer("abc");
sb.append("123").append("ZZ");

//주소 반환 -> 같은 주소를 가리킴
StringBuffer sb2 = sb.append("ZZ");
```
### 2.9.4. StringBuffer 비교
- equals()를 오버라이딩 하지 않음 -> "=="과 같은 결과
- toString()은 오버라이딩함 -> 담고 있는 문자열 String으로 반환
- toString() 호출 후 String인스턴스를 얻은 다음, equals사용해 비교해야 함
### 2.9.5. StringBuffer 메서드
- StingBuffer()/(int length)/(String str)
- int capacity() : 버퍼 크기, 문자열 길이는 length()
- StringBuffer delete(int start, int end)
- StringBuffer insert(int pos, ~) : ~를 받아서 pos(0부터)에 추가
- StringBuffer reverse()
- void setCharAt(int index, char ch)
- void setLength(int newLength) : 길이조정, 늘어나면 널문자로 'u/0000'
- toString, subString, charAt, length..
## 2.10. StringBuilder
### 2.10.1. 기능
- "동기화를 뺀 StringBuffer"
- 완전히 똑같은 기능으로 작성되어 있음
### 2.10.2. 필요 이유
- StringBuilder는 멀티쓰레드에 안전하도록 동기화
- 동기화 -> StringBuffer 성능 떨어뜨림 (멀티스레드 아닌경우?)
- 13장에서 나옴
***

# 3. Math 클래스
## 3.1. Math
- 기본적인 수학 계산
- random(), round()..
- 인스턴스 변수가 하나도 없어서 객체가 필요 없다. private으로 되어있음
- 메서드가 모두 static
- 상수 2개만 있음 : 자연로그의 밑 E, 원주율 PI
## 3.2. 올림, 버림, 반올림
- round() : 소수 0번째에서 반올림하여 long 반환
    - 10제곱 활용해서 자릿수 조절 가능
## 3.3. 메서드
- 모두 static, alias:dfil -> double/float/in/long 
- dfil abs(dfil) : 절대값 반환
- double ceil/floor
- dfil max/min(dfil,dfil) : 둘 중 큰거/작은거 반환 -> 자료형 두개 같음
- double rint(double a) : double 값과 가장 가까운 정수값을 double로 반환, 1.5, 2.5 등 가운데 값은 짝수 반환
### round(), rint()
- round(-1.5) = -1 임 -> 가까운 정수로 반올림
- rint도 가까운 정수 반올림하지만 중간값 짝수처리 -> -2.0
***

# 4. wrapper 클래스 
## 4.1. 필요성
- 매개변수로 객체를 요구, 기본형 값이 아닌 객체로 저장해야할 때, 객체간의 비교가 필요할 때 등등 기본형 값들을 객체로 변환하여 수행해야됨.
## 4.2. 특징
- 각 자료형에 알맞은 값을 내부적으로 저장하고 있음
- 숫자인 기본형들의 래퍼클래스의 생성자 매개변수에 String 다 있음. 자료형에 맞지 않는 문자열 제공 시 NumberFormatException 발생
- 래퍼 클래스들은 모두 equals()가 오버라이딩되어 있음 -> 주소가 아닌 객체가 가진 값을 비교
- static상수 공통 : MAX_VALUE, MIN_VALUE, SEIZ, BYTES, TYPE

## 4.3. wrapper와 비교 정리
### equals()
- 보통 equals()는 객체를 받아서 주소값을 비교함. 만약 멤버변수를 비교하고 싶다면 오버라이딩 해야함
- wrapper에서는 equals()가 이미 오버라이딩 되어있어서 값끼리 비교가 됨  
-> ex) Integer는 숫자로 처리되어 비교 
### "=="
- "==" 는 주소값을 비교하기 때문에 객체를 각각 생성 시 false가 나옴
- 리터럴 방식으로 선언 시 한 문자열을 가르키므로 주소가 같음 -> 동등연산자 사용 시 true -> ex)String a = "ab"; String b = "ab"
### 정리
- warpper에서 "=="는 주소값 비교, wrapper를 리터럴 선언시 주소가 같아서 true
- equals()는 원래 객체(주소)를 비교, wrapper에서 자동 오버라이딩 되어서 값을 비교
***

# 5. Number 클래스
## 5.1. 설명
- 숫자형 멤버변수를 가진 wrapper 클래스들의 조상, 모든 숫자와 관련된 래퍼 클래스들의 조상
- BigInteger : long으로 못다루는 큰 범위 정수
- BigDecimal : double로도 못다루는 큰 범위 부동 소수점; 도 포함
- 객체가 가지고 있는 값을 숫자와 관련된 기본형으로 반환하여 반환하는 메서드들을 정의(353p 하단 int가 기본?)
## 5.2. 문자열 숫자로 변환
```
int i = new Integer("100").intValue();
int i2 = Integer.parseInt("100");
Integer i3 = Integer.valueOf("100");
```
## 5.3. 다른 진법 변환
```
static int parseInt(String s, int radix)
static Integer valueOf(String s, int radix)
```
- radix에 진법, 16진수면 s에 A~F도 허용

# 6. 오토박싱 & 언박싱
- JDK1.5 이전에는 래퍼와 기본형사이 연산을 하려면 변환해야 했음
- 현재는 컴파일러가 자동으로 변환하는 코드를 넣어줘서 가능(예제에서는 래퍼에다가 intValue()붙임)
- 객체배열을 가진 Vector클래스나 ArrayList클래스에 기본형 값을 저장해야할 때나 형변환이 필요할 때도 컴파일러가 코드 추가
- 래퍼 -> 기본 : 오토박싱 / 기본 -> 래퍼 : 언박싱
- 컴파일러가 도와주는 것 뿐 **자바의 원칙이 바뀐것은 아님**
