# Call By Value vs Call By Reference

## Call by Value

- 함수에 인자를 전달할 때, 인자의 값의 복사본이 생성되어 함수에 전달

## Call by Reference

- 함수에 인자를 전달할 때, 인자가 가리키는 객체의 참조(주소)가 전달

## 자바는 Call By Value

- 기본 데이터 타입 : 값 자체가 함수에 복사되어 전달
- 객체 : 객체의 참조(주소의 값)가 복사되어 전달

> 자바에서 메서드에 객체의 참조가 인자로 넘어가면, 메서드 내부에서 해당 객체의 값을 수정 가능하다. 그래서 Call By Reference와 착각할 수 있는데, 엄밀히 말하면 참조값이 복사된 것이라 Call By Value이다.
