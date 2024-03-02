# AutoBoxing vs AutoUnboxing

## 설명

- 각 Wrapper class에 상응하는 Primitive data type이라면 필요에 따라 자동으로 변환해주는 기능이다.
- JDK 1.5 부터는 컴파일러가 AutoBoxing과 AutoUnboxing이 필요한 경우 자동으로 처리해준다.
- 때문에 명시적으로 타입을 변환하지 않아도 된다.

## 기본 타입에 대응하는 Wrapper 클래스들

- 기본 타입 : int, long, float, double, boolean 등
- Wrapper 클래스 : Integer, Long, Float, Double, Boolean 등

## boxing, unboxing

- boxing : 기본 타입 데이터 -> 대응하는 Wrapper 클래스
- unboxing : Wrapper 클래스 -> 기본 타입 변환

## 성능에 주의할 점

- 편의성을 위해 오토 박싱과 언박싱이 제공되고 있지만, 내부적으로 추가 연산 작업이 거치게 된다.
- 무분별하게 오토 박싱&언박싱이 일어나지 않고, 동일한 타입 연산이 이루어지도록 구현하는게 좋다.

## 참고

- https://gyoogle.dev/blog/computer-language/Java/Auto%20Boxing%20&%20Unboxing.html
