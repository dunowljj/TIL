# CleanCode

# 4장 주석

- 잘 달린 주석은 그 어떤 주석보다 유용하나 경솔하고 근거 없는 주석은 코드를 이해하기 어렵게 만든다.
- 주석은 거짓말을 한다. 오래될수록 코드에서 멀어진다.
- 필자는 코드를 깔끔하게 정리하고 표현력을 강화하는 방향, 그래서 애초에 주석이 필요없는 방향에 에너지를 쏟는다.
- 부정확한 주석은 없으니만 못함. 주석을 가능한 줄이자

▷ 주석은 나쁜 코드를 보완하지 못한다

- 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문
- 난장판을 설명하지 말고, 정돈해라

### 코드로 의도를 표현하라!

- 확실히 코드만으로 의도를 설명하기 어려운 경우가 존재함. 그러나 코드는 훌륭한 수단이 아니라고 해석해선 안됨.
- 몇 초만 더 생각하면 코드로 대다수 의도를 표현 가능. 주석으로 달려는 설명을 함수로 만들어 표현해도 충분한 경우가 많다.

### 좋은 주석

▷ 법적인 주석

- 소스 파일 첫머리 저작권 정보와 소유권 정보
- FitNess에서 모든 소스 파일 첫머리에 추가한 표준 주석 헤더

▷ 정보를 제공하는 주석(71p)

- 추상 메서드가 반환할 값에 대한 설명 -> 함수 이름에 담기
- 정규표현식이 날짜임을 알려주는 주석 -> 시각과 날짜를 변환하는 클래스를 만들어 코드를 옮겨주기

▷ 의도를 설명하는 주석(71~72p)

- 저자의 의도가 분명하게 드러나야 함
- 정렬 우선순위를 주석으로 이야기

▷ 의미를 명료하게 밝히는 주석(73p)

- 인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속할 때 의미를 명료하게 밝힘
- 그릇된 주석을 달아놓을 위험성이 상당히 높다.

▷ 결과를 경고하는 주석

- 테스트 케이스를 꺼야하는 이유 - > //여유 시간이 충분하지 않다면 실행하지 마십시오.
    - 요즘엔 @Ignore 속성의 문자열 사용
- //SimpleDateFormat은 스레드에 안전하지 못하다. 인스턴스를 독립적으로 생성해야 한다.
    - 효율을 높이기 위해 정적 초기화 함수를 사용하려던 열성적인 프로그래머가 주석 덕에 실수 면함

▷ TODO 주석

- 앞으로 할일
    - 구현하지 않은 이유와 미래 모습..
- 필요하다 여기지만 당장 구현하기 어려운 업무를 기술
    - 필요없는 기능 삭제 알림, 문제를 봐달라는 요청, 더 좋은 이름 부탁, 앞으로 발생할 이벤트에 맞춰 고치하는 주의 등
- 나쁜 코드의 핑계가 되어선 안됨
- 떡칠해선 안된다. 주기적인 점검으로 없애도 되는 것은 지우라고 권함

▷ 중요성을 강조하는 주석

- 자칫 대수롭지 않다고 여겨질 것이 중요한 경우 주석 사용
    - trim()이 중요해서 설명 부여하는 경우

▷ 공개 API에서 Javadocs

- 좋음
- 하지만 여느 주석과 마찬가지로 자바독스 역시 독자를 오도하거나, 잘못 위치하거나 ,그릇된 정보를 전달할 가능성이 존재

### 나쁜 주석

▷ 주절거리는 주석

- 특별한 이유 없이 의무감으로 혹은 프로세스에서 하라고 하니까 마지못해 주석을 다는 것은 시간 낭비
- 답을 알아내려면 다른 코드를 뒤져야 함. 이해가 안 되어 다른 모듈까지 뒤져야 하는 주석은 독자와 제대로 소통하지 못하는 주석이다.

▷ 같은 이야기를 중복하는 주석

- 코드 내용을 그대로 중복하고, 의미 없는 주석(77~78p)

▷ 오해할 여지가 있는 주석

- 주석에 담긴 '살짝 잘못된 정보'로 인해 this.closed가 true로 변하는 순간에 함수가 변환되리라는 생각으로 함수를 호출한 프로그래머(79p)

▷ 의무적으로 다는 주석

- 모든 함수에 javadocs를 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석다.
- Javadocs를 넣으라는 규칙이 낳은 괴물

▷ 이력을 기록하는 주석

- 예전에는 소스코드 관리 시스템이 없었으므로, 모든 모듈 첫머리에 변경 이력을 기록하고 관리하는 관례가 바람직했다. 지금은 혼란만 가중하므로 완전히 제거하는 편이 좋다.

▷ 있으나 마나 한 주석

- 너무 당연한 사실을 언급하는 주석
- 지나친 참견이라 개발자가 주석을 무시하는 습관에 빠진다.
- 이상한거 쓰지말고 코드를 정리하자

▷ 무서운 잡음

- javadocs도 때로는 잡음이다
- 정보를 제공해야 한다는 잘못된 욕심으로 탄생한 잡음 (84p)

▷ 함수나 변수로 표현할 수 있다면 주석을 달지 마라(84p 다시보기)

- 주석이 필요하지 않도록 코드를 개선하는게 좋다.

▷ 위치를 표시하는 주석

- 너무 자주 사용하지 않는다면 배너는 눈에 띄며 주의를 환기한다.
- 반드시 필요할 때만, 아주 드물게 사용
- 남용 시 독자가 흔한 잡음으로 여겨 무시한다.

▷ 닫는 괄호에 다는 주석

- 작고 캡슐화된 함수에는 잡음일 뿐이다.
- 닫는 괄호에 주석을 달아야겠다는 생각이 든다면 대신 함수를 줄이려 시도하자

▷ 공로를 돌리거나 저자를 표시하는 주석

- 저자 이름으로 코드를 오염시킬 필요가 없다.
- 누구한테 물어볼지 알수 있음? -> 현실적으로 오랫동안 코드에 방치되어 쓸모없는 정보로 변하기 쉬움
- 소스 코드 관리 시스템을 쓰자

▷ 주석으로 처리한 코드(87p 다시보기)

- 주석으로 처리한 코드는 다른 사람들이 지우기를 주저한다.
    - 남겨논 이유가 있으리라 추측해서 못 지움
    - 질나쁜 와인병 바닥에 앙금마냥 쌓임

▷ HTML 주석

- 혐오 그 자체?
- 편집기/IDE에서조차 읽기 어렵다.
- HTML태그를 삽입하는 책임은 프로그래머가 아닌 도구가 져야한다.

▷ 전역 정보

- 주석을 달아야 한다면 근처에 있는 코드만 기술하라
- 전반적인 시스템의 정보를 기술하지 마라
    - 코드 일부에 주석을 달면서  전반적인 시스템 정보가 바뀌었을때, 일부 코드가 수정될거라는 보장이 없다.

▷ 너무 많은 정보

- 주석에 흥미로운 역사나 관련 없는 정보를 장황하게 늘어놓지 마라

▷ 모호한 관계

- 주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.
- 주석 자체가 다시 설명을 요구하는 경우 안타까움

▷ 함수 헤더

- 짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훨씬 좋다.

▷ 비공개 코드에서 Javadocs

- 공개 API는 Javadocks가 유용하지만 공개하지 않을 코드라면 Javadocs는 쓸모가 없다.
- 생설할 필요는 없다.
- 코드만 산만해짐
 
 - 90p 리팩터링 다시 보기