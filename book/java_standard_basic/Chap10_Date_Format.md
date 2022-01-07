날짜와 시간&형식화
========
- Date는 JDK 1.0부터 제공되어 옴, 1.1부터 Calendar
- 1.8부터 java.time패키지로 기존의 단점을 개선한 클래스들 추가 -> LocalDate, LocalTime, LocalDateTime
- 그러나 20년 넘게 사용된 Date 와 Calendar 무시 불가, 깊게 배우지 말고 이해하고 활용정도만

# 1. Calendar 클래스
## 1.1. 설명
- 추상 클래스 -> getInstance 사용
    - getInstance를 사용하는 쪽은 변경하지 않아도 된다는 장점이 있다.(추상화)
- 얻은 인스턴스는 기본적으로 현재 시스템의 날짜와 시간에 대한 정보 담고 있음

- 월 값이 0~11
## 1.2. get,set
- 가져오기, 설정하기
- Calendar.
    YEAR/MONTH
    WEEK_OF_YEAR/MONTH : 이 해/달의 몇 째 주
    DATE, DAY_OF_MONTH : 이 달의 몇 일
    DAY_OF_YEAR : 이 해의 몇 일 (1~365?)
    DAY_OF_WEEK : 요일
    DAY_OF_WEEK_IN_MONTH : 몇 번째 요일
    HOUR(0~11)/MINUTE/SECOND/MILLISECOND, HOUR_OF_DAY(0~23)
    ZONE_OFFSET : 
## 1.3. 날짜 간 차이
- getTimeInMillis()로 초단위로 변경 (1/1000초)
- 일 단위 : (/1000)24 * 60 * 60
## 1.4. add, roll
- add(Calendar.DATE/MONTH/YEAR, 숫자) : 해당 필드 증가 혹은 감소
- roll은 형식과 기능은 같으나 다른 필드에 영향을 안 미침. 31일 넘어가도 MONTH 안올라감
## 1.5. 달력만들기
- 다음달의 1일에서 하루를 빼면 마지막 일을 알 수 있다.
## 1.6. Calendar <-> Date
```
//cal -> date
Calendar cal = Calendar.getInstance();
...
Date d = new Date(cal.getTimeInMillis());

//date -> cal
Date d = new Date();
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```
***

# 2. 형식화 클래스
## 2.1. 설명
- 날짜의 형식을 직접 출력하려면 복잡함
- java.text패키지에 형식화 클래스 사용
- 데이터를 정의된 패턴에 맞춰 형식화 할 수 있을 **뿐만 아니라** 형식화된 데이터에서 원래 데이터를 얻어낼 수도 있다.
## 2.2. DecimalFormat(376p)
### 2.2.1. 사용
- 숫자를 형식화 하는데 사용
- 정수, 부동소수점, 금액 등 다양하게 표현 가능
- 일정한 형식 텍스트 데이터 숫자로 쉽게 변환 가능
- 0 # . - , E ; % \u2030 \u00A4 '(escape)
### 2.2.2. 문자열 -> 숫자
```
DecimalFormat df = new DecimalFormat("#,###.##");
Number num = df.parse("1,234,567.89")
```
- **parse**를 이용해서 문자열을 숫자로 쉽게 변환 가능
    - DecimalFormat 조상인 NumberFormat에 정의된 메서드
    ```
    public Number parse(String source) throws ParseException
    ```
- format-숫자 / parse-문자
## 2.3. SimpleDateFormat
### 2.3.1. 기본사용
- 형식 패턴표 (379p)
```
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-mm-dd");

String result = df.format(today)
```
- SimpleDateFormat 인스턴스 생성 후, Date인스턴스를 사용하여 format 호출
### 2.3.2. 문자열 -> 숫자
- 마찬가지로 parse("")사용  -> 저장된 형식과 입력한 형식이 일치하지 않는 경우에는 예외발생, 적절한 예외처리가 필요
- format-날짜 / parse-문자

