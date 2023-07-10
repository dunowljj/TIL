효과적인 쿼리 저장
===
### 1. SELECT 시에는 꼭 필요한 컬럼만 불러오기
- 많은 필드를 불러올수록 DB는 더 많은 로드를 부담하게 된다.
### 2. 조건 부여 시, 가급적이면 기존 DB값에 별도의 연산을 걸지 않기
```sql
-- Inefficient
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
    FROM movie m 
    INNER JOIN rating r 
    ON m.id = r.movie_id 
    WHERE FLOOR(r.value/2) = 2 
    GROUP BY m.id;

-- Improved
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
    FROM movie m 
    INNER JOIN rating r 
    ON m.id = r.movie_id 
    WHERE r.value BETWEEN 4 AND 5 
    GROUP BY m.id;
```
- Improved의 경우,
  - r.value가 가진 index를 그대로 활용해서 모든 필든 값을 탐색할 필요가 없다.
- Inefficient의 경우,
  - Full Table Scan을 해서 모든 Cell 값을 탐색하고, 수식을 건 뒤, 조건 충족 여부를 판단한다.
  - 간단한 비교 연산자들은 인덱스를 사용할 수 있다.
  - 그러나 컬럼을 비교하기 전에 값을 수정하는 함수나 연산자를 사용하면 인덱스 사용을 못할 수 있다.
  - 위의 FLOOR가 비교 전에 컬럼 값을 변경해서 
### 3. LIKE 사용 시 와일드카드 문자열(%)을 String 앞부분에는 배치하지 않기
- 앞부분에 % 붙여서 사용 시 인덱스를 사용하지 못하며, Full Scan을 한다.
  - WHERE name LIKE '%john%'과 같이 쓴다면, 모든 위치에서 john을 찾아야한다. 
  - value IN (...), value ="...", value LIKE "...%"는 인덱스를 활용할 수 있다.
### 4. SELECT DISTINCT, UNION DISTINCT와 같이 중복 값을 제거하는 연산은 최대한 사용하지 않기
```sql
-- Inefficient
SELECT DISTINCT m.id, title 
    FROM movie m  
    INNER JOIN genre g 
    ON m.id = g.movie_id;

-- Improved
SELECT m.id, title 
    FROM movie m  
    WHERE EXISTS (SELECT 'X' FROM rating r WHERE m.id = r.movie_id);
```
- 중복 값을 제거하는 연산은 시간이 많이 걸린다.
- 불가피하게 써야할 경우 distinct 연산을 대체하거나 연산의 대상이 되는 테이블의 크기를 최소화하기
- EXISTS 활용 (해당 값이 1건이라도 있으면 검색을 멈추고 TRUE 반환)
### 5. 같은 내용의 조건이라면, GROUP BY 연산 시에는 가급적 HAVING 보다는 WHERE 절을 사용하기
```sql
-- Inefficient
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
    FROM movie m  
    INNER JOIN rating r 
    ON m.id = r.movie_id 
    GROUP BY id 
    HAVING m.id > 1000;

-- Improved
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
    FROM movie m  
    INNER JOIN rating r 
    ON m.id = r.movie_id 
    WHERE m.id > 1000
    GROUP BY id ;
```
- WHERE 절이 HAVING 절 보다 먼저 실행된다.
- 미리 데이터를 작게 만들어서 GROUP BY에서 다루는 데이터 크기를 줄일 수 있다.
### 6. 3개 이상의 테이블을 INNER JOIN 할 때, 크기가 가장 큰 테이블을 FROM 절에 배치, INNER JOIN 절에는 남은 테이블을 작은 순서대로 배치하기
- 복잡한 쿼리에서는 완전하게 최적화되지 않은 INNER JOIN 연산이 실행될 때가 있으므로 사전에 방지하기 위해 입력 단계에서 순서를 조정해 두는 게 도움이 된다.
  - INNER JOIN 과정에서 최소한의 조합을 탐색하면 좋다. 하지만 항상 통용되지 않는다. query planner가 효과적인 순서로 바꿔준다.
  - 테이블의 증가 --> Planning 비용 증가 : 복잡한 쿼리에서는 최적화되지 않은 INNER JOIN 연산이 실행되기도 한다. 순서를 정렬하는 시간이 소요되어 순서를 직접 주는게 나을때가 있다.
### 7. 자주 사용하는 데이터의 형식에 대해서는 미리 전처리된 테이블을 따로 보관/관리하기
- RDBMS의 원칙에 어긋나는 측면이 있고, DB의 실시간성을 반영하지 못할 가능성이 높기 때문에, 대부분 분석계에서 더 많이 사용한다.
- 핵심 서비스 지표를 주기적으로 계산해서 따로 모아두는 것 등이 가장 대표적

## 참고 및 출처
- https://medium.com/watcha/%EC%BF%BC%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94-%EC%B2%AB%EA%B1%B8%EC%9D%8C-%EB%B3%B4%EB%8B%A4-%EB%B9%A0%EB%A5%B8-%EC%BF%BC%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-7%EA%B0%80%EC%A7%80-%EC%B2%B4%ED%81%AC-%EB%A6%AC%EC%8A%A4%ED%8A%B8-bafec9d2c073