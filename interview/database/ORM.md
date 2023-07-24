ORM(Object-Relational Mapping)
===
- 객체와 관계형 데이터베이스를 자동으로 매핑해 주는 것
- 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법.
객체 지향 언어에서 사용할 수 있는 "가상" 객체 데이터베이스를 구축하는 방법이다.

## 영속성(Persistence)
- 데이터를 생성한 프로그램의 실행이 종료되더라도 사라지지 않는 데이터의 특성.
- 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 된다. 
때문에 파일 시스템, 관계형 테이터베이스 혹은 객체 데이터베이스 등을 활용하여 데이터를 영구하게 저장하여 영속성 부여한다.

## 영속 계층(Persistence layer)
- 프로그램의 아키텍처에서 데이터에 영속성을 부여해주는 계층.
- 자바의 겨우 JDBC를 이용하여 직접 구현할 수 있지만, Persistence framework를 이용한 개발이 많이 이루어진다.

## Persistence Framework
- JDBC 프로그래밍의 복잡함이나 번거로움 없이 간단한 작업만으로 데이터베이스와 연동되는 시스템을 빠르게 개발할 수 있으며 안정적인 구동을 보장한다.

### Jdbc 사용
```java
public String getPersonName(long personId) throws SQLException {
    PreparedStatement st = null;
    try {
        st = connection.prepareStatement (
            "SELECT name FROM people WHERE id = ?");
            st.setLong(1, personId);
            ResultSet rs = st.excuteQuery();
            if (ts.next()) {
                String result = rs.getString("name");
                assert !rs.next();
                return result;
            } else {
                return null;
            }
    } finally {
        if (st != null) {
            st.close();
        }
    }
}
```

### JPA 사용
```java
public String getPersonName(long personId) {
    Person p = em.find(Person.class, personId);
    return p.getName();
}
```
### 종류
SQL Mapper
  - SQL 문장으로 직접 데이터베이스의 데이터를 다룬다.
  - 객체와 SQL문을 매핑한다. SQL 의존적이다.
  - ex) MyBatis

ORM
  - 객체를 통해 간접적으로 데이터베이스 데이터를 다룬다.
  - 객체와 DB 테이블을 매핑한다.
  - ex) Hibernate, EclipseLink 

## ORM 장단점
### 장점
- 패러다임의 불일치를 해결한다. 객체지향 언어가 가진 장점을 활용할 수 있다.
- 객체지향적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 된다.
  - 선언문, 할당, 종료 같은 부수적인 코드가 없거나 줄어든다.
  - 각종 객체에 대한 코드를 별도로 작성하기 때문에 코드의 가독성을 올려준다.
  - SQL의 절차적이고 순차적인 접근이 아닌 객체 지향적인 접근으로 생산성이 증가한다.
- 재사용 및 유지보수의 편리성이 증가한다.
  - ORM은 독립적으로 작성되어있고, 해당 객체들을 재활용 할 수 있다.
  - 때문에 모델에서 가공된 데이터를 컨트롤러에 의해 뷰와 합쳐지는 형태로 디자인 패턴을 견고하게 다지는데 유리하다.
- DBMS에 대한 종속성이 줄어든다.
  - 대부분 ORM 솔루션은 DB에 종속적이지 않다.
  - 종속적이지 않다는 것은 구현 방법 뿐만 아니라 많은 솔루션에서 자료형 타입까지 유효하다.
  - DBMS를 교체하는 거대한 작업에도 비교적 적은 리스크와 시간이 소요된다.
### 단점
- 완벽한 ORM 으로만 서비스를 구현하기가 어렵다.
  - 일부 자주 사용되는 대형 쿼리는 속도를 위해 SP를 쓰는등 별도의 튜닝이 필요한 경우가 있다.
  - jpa를 사용할때도 복잡한 쿼리문에는 jdbcTemplate이나 Mybatis를 사용하기도 한다.
  - DBMS의 고유 기능을 이용하기 어렵다.(고유 기능을 쓰면 이식성이 저하되기 때문에 단점만은 아니다.)
- 사용하기는 편하지만 설계는 매우 신중하게 해야한다.
  - 프로젝트의 복잡성이 커질경우 난이도 또한 올라갈 수 있다.
  - 잘못 구현된 경우에 속도 저하 및 심각할 경우 일관성이 무너질 수 있다.
- DBMS의 고유 기능을 이용하기 어렵다. (하지만 이건 단점으로만 볼 수 없다 : 특정 DBMS의 고유기능을 이용하면 이식성이 저하된다.)
- 프로시저가 많은 시스템에선 ORM의 객체 지향적인 장점을 활용하기 어렵다.
  - 이미 프로시저가 많은 시스템에선 다시 객체로 바꿔야하며, 그 과정에서 생산성 저하나 리스크가 많이 발생할 수 있다.
  - 프로시저는 데이터베이스에 저장되어 실행되는 일련의 SQL 명령어를 말하며, 이미 많이 작성된 시스템은 작어 소요가 크기 떄문에  ORM을 도입하기 어렵다는 의미이다.
- 러닝 커브가 높은 편이다.


## 참고 및 출처
- https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#orm%EC%9D%B4%EB%9E%80
- https://firework-ham.tistory.com/110