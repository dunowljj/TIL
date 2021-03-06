챕터 7 OOP2
==========
# 1. 내부 클래스
- 클래스 내부에 클래스
- 외/내부 클래스가 서로 긴밀한 관계
- 서로 쉽게 접근, 외부에 불필요한 클래스 감춤
- 인스턴스/스태틱/지역/익명 클래스
- 컴파일 시 클래스 파일 "파일명$(숫자).class"와 같이 클래스마다 하나씩 생김
## 1.1. 장점
- 내부 클래스 -> 외부 클래스 멤버 접근 쉽게
- 코드의 복잡성 줄임, 캡슐화 증가
## 1.2. 제어자
- 접근 제어자 모두 가능, abstract, final 제어자 가능
## 1.3. 접근성
- 스태틱 클래스 -> 인스턴스 멤버 사용 x
- 지역 클래스 -> final 붙은 지역변수만 사용 가능  
    -> 메서드가 수행을 마치고 지역변수가 소멸된 시점에 지역 클래스의 인스턴스가 소멸된 지역변수를 참조하려는 경우가 발생 가능  
    -> jdk1.8부터 컴파일러가 final 알아서 붙여줌. (값 수정 불가)
- 실제로 외부 클래스가 아닌 다른 클래스에서 내부 클래스를 생성하고 멤버에 접근하는 경우는 내부 클래스로 선언하면 안되는 것을 선언했다는 의미
- 외/내부에서 선언한 변수명 같을 시 this 혹은 아우터이름.this로 구분 가능
## 1.4. 사용
- 인스턴스 : 외부인스턴스 생성 -> 외부참조명.new로 생성
- 스태틱 : new 외부이름.내부이름
***
# 2. 익명 클래스
- 클래스 선언과 객체 생성을 동시에 -> 1회용, 객체 1개 생성 가능
- new 조상이름||구현인터페이스이름() {}
- 이름이 없음
***
