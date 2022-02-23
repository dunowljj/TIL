String
======


***
Interface CharSequence
=====
- String 메서드에 매개변수로 있는 걸 본 것 같은데, 아직 뭐하는 놈인지 잘 모르겠다.
- 읽을수 있는 char 값의 시퀀스이다.
- 많은 종류의 연속된 문자에 대해 형식과 읽기 전용 접근을 제공한다.
>This interface does not refine the general contracts of the equals and hashCode methods.  

- equals 나 hashCode 메서드를 지원하지 않는다는 의미같은데, refine the general contracts 라는 문장이 잘 와닿지 않는다.
- 인스턴스를 다른 것들의 인스턴스와의 동등성을 테스트하는 것이 보증이 안됨 -> CharSequence의 인스턴스들을 set 혹은 map의 키들로 임의로 사용하면 부적절하다.

>dudmy 님의 CharSequence 와 String의 차이  
https://dudmy.net/android/2017/09/15/difference-char-string/
***

StringBuilder
===
### java.lang.StringBuilder


### All implementated method
- Serializable, Appendable, CharSequence
***
```
public final class StringBuilder 
extends Object
implements Serializable, CharSequence
```
- 가변적인 문자열의 집합이다. **StringBuffer**와 비슷한 API를 제공하고, 동시성을 보장하지 않는다.
- (일반적인 경우) 싱글쓰레드에서 string buffer가 사용될때 **StringBuffer**를 drop-in replacement한다.
>drop-in replacement는 작은 노력으로 혹은 코드 변화 없이 대체 가능하다는 의미이다.
- 일반적으로 **StringBuffer** 보다 우선적으로 사용하는 것을 권장한다. 대부분의 구현에서 더 빠를 것이다.  

+ 주요한 **StringBuilder**의 작업은 **append**와 **insert** 메서드이다.오버로드되어있기 때문에 어느 타입의 데이터든 사용가능하다.
+ 주어진 데이터를 효과적으로 문자열로 변환하며, 그런 다음에 그 문자열의 문자들을 string builder에 apppend하거나 insert한다.
+ **append**메서드는 항상 이 문자들을 builder의 뒤에 더한다.
+ **insert**메서드는 특정 지점에 문자들을 더한다.  

- 예를 들어서 현재 내용이 "start"인 z가 string builder 객체를 참조한다면, z.append("le") 호출 시 string builder가 "startle"를 포함하고, z.insert(4,"le") 호출 시 "starlet"을 포함하도록 대체할 것이다.  

+ 일반적으로, sb가 **StringBuilder**의 인스턴스를 참조한다면, **sb.append(x)** 는 **sb.insert(sb.legnth(), x)** 와 같은 효과다.  

- 모든 string builder가 용량이 있듯이,string builder에 포함된 character sequence가 용량을 초과하지 않는 한, 새로운 내부 버퍼를 할당하는 것은 필수적이지 않다.
- 내부 버퍼가 오버플로우되면, 자동적으로 더 커질 것이다.  

+ **StringBuilder**의 인스턴스는 멀티쓰레드에서 사용하기에 안전하지 않다
+ 동시성이 요구된다면 **StringBuffer**를 사용하는것을 권장한다.