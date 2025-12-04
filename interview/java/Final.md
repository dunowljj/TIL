# final 키워드

## 역할

### final class

다른 클래스가 상속받지 못한다.

### final method

자식 클래스에서 상위 클래스의 final method를 오버라이드 하지 못한다.

### final variable

변하지 않는 상수 값이 되어 새롭게 값을 할당할 수 없는 변수가 된다.

주의 : 객체(컬렉션, 배열 등)의 경우 final로 선언을 해도, 해당 변수에 다를 객체를 재할당하지 못할 뿐, 내부 값을 수정할 수 있다.

## 왜 필요 할까?

### final class

- 불변객체 생성
  - ex, String
- 보안, 또는 일관성을 위해서
  - ex) java.lang.System을 통한 시간 조작, Integer HashMap 기반 보안 기능 우회
- (드물게) 성능 최적화
  - JIT는 final 클래스를 보면 최적화가 수월해짐.
  - 주된 필요 이유는 아님

#### final class, 보안에 꼭 필요한가? 개발자가 나쁜 마음만 안먹으면 되는거 아니야?

- 샌드박스 보안 모델, 플러그인 환경, 외부의 신뢰할 수 없는 코드가 섞이는 경우 등 다양한 변수가 존재한다.
- ex) IDE, JNLP, DSL, Jenkins, Java Applet 등

#### Applet이 사장된 이유

- Applet은 브라우저가 “단순 실행자” 역할이다. Applet을 실제로 실행한 것은 브라우저 내부의 외부 JVM 플러그인이다.
- 애초에 자바나 C/C++ 등은 OS에 접근 가능한 성향이 다른 언어라 브라우저 환경에서 힘들다.
- 반면, JS/WASM은 다음 “브라우저 내부 엔진”에서만 실행.
- 브라우저는 폐쇄적 샌드박스. HTML, CSS, JavaScript는 애초에 브라우저를 위해 설계됨.

### final method

- 의도된 대로 동작하게 만드는 것
- 중요한 동작을 보호하기 위해

### final variable
- 불변성, 안정성 보장
- Thread-safe, side-effect 방지, defensive copy 제거
- 캐싱 가능