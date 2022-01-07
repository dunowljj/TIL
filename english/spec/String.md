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