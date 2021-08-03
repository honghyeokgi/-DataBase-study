## 🛢 데이터베이스 EXPLAIN, 인덱스 스캔

--------------------------------------------------------------------------------

### 📑 EXPLAIN :  쿼리의 실행  순서 및 계획을 확인할수 있다. ( SELECT,INSERT,UPDATE,DELETE)

동일한 결과를 뽑아 내더라도 성능이 다른 경우들이 종종있다. 쿼리 처리속도가 느리다면 EXPLAIN 으로

실행계획을 확인해보고 쿼리를 튜닝 할 필요가 있다.

```mysql
EXPLAIN SELECT ... FROM ...FROM ...WHERE ...
```

위와 같은 방법으로 EXPLAIN 을 사용할수 있다.

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC1.png?raw=true)

위 사진은 Smart Farm KOREA 의 API 를 받아서 제가 구성한 db의 EXPLAIN 결과 입니다.

| Column        | JSON Name     | Meaning                                    |
| ------------- | ------------- | ------------------------------------------ |
| id            | select_id     | SELECT 식별자                              |
| select_type   | NONE          | SELECT 유형(서브쿼리가 있는지 없는지 등등) |
| table         | table_name    | 출력 행에 대한 테이블                      |
| type          | access_type   | 조인 유형                                  |
| possible_keys | possible_keys | 선택할 수 있는 인덱스 종류                 |
| Key           | Key           | 실행과정에서 사용한 인덱스                 |
| key_len       | key_length    | 선택한 키의 길이                           |
| ref           | ref           | 인덱스와 비교한 열                         |
| rows          | rows          | 연산중 확인할 rows 추정치                  |
| filtered      | filtered      | 조건으로 필터링된 행의 비율                |
| Extra         | none          | 추가정보                                   |

1. id:  어떤 SELECT 구문을 나타내는 지를 알려주는 것으로 구문에 서브 쿼리나 UNION이 없다면 SELECT는 하나밖에 없기 때문에 모든 행에 대해 1이란 값이 부여되지만 이외의 경우에는 원 구문에서 순서에 따라 각 SELECT 구문들에 순차적으로 번호가 부여된다.

   

2. select_type

| select_type Value        | JSON Name                  | Meaning                                                      |
| ------------------------ | -------------------------- | ------------------------------------------------------------ |
| SIMPLE                   | None                       | UNION,SUBQERY가 존재하지 않는 단순한 SELECT                  |
| PRIMARY                  | None                       | 서브쿼리를 사용한다면 가장 바깥쪽에 있는 쿼리, UNION 이라면 첫번 째 SELECT 쿼리이다. |
| UNION                    | None                       | UNION 쿼리에서 PRIMARY 를 제외한 나머지 SELECT               |
| DEPENDENT                | dependent (true)           | UNION과 동일하지만 외부쿼리에 의존적임                       |
| UNION RESULT             | union_result               | UNION 쿼리의 결과                                            |
| SUBQUERY                 | None                       | 서브쿼리 혹은 서브쿼리를 구성하는 여러 쿼리들 중에서 첫 번째 SELECT 문이다 |
| DEPENDENT<br />SUBQUERY  | dependent ( true )         | 서브쿼리와 동일하지만 외곽 쿼리에 의존적이다.                |
| DERIVED                  | None                       | SELECT 로 추출된 테이블(FROM 절에서 서브쿼리 혹은 INLINE VIEW) |
| DEPENDENT<br />DERIVED   | dependent( true )          | 다른 테이블에 종속되어있는 파생 테이블                       |
| MATERIALIZED             | materialized_from_subquery | 구체화된 서브쿼리?                                           |
| UNCAHEABLE<br />SUBQUERY | cacheable ( false )        | 서브쿼리와 동일하나 공급되는 모든 값에 대해 서브쿼리를 재처리,공급되는 값이 같더라도 캐시된 결과를 사용할수 없다. |
| UNCACHEABLE<br />UNION   | cacheable( false )         | UNION과 동일하지 공급되는 값에 대하여 UNION 쿼리를 재 처리한다 |



3. table  : 출력되는 row들이 참조하는 테이블의 이름

| table     | meaning                                                      |
| --------- | ------------------------------------------------------------ |
| unionm,n  | id 값이 m과 n 인 행의 합집합을 나타낸다                      |
| derivedN  | id 값이 N인 행에 대한 파생 테이블 결과를 참조합니다. 파생 테이블은 예를 들어 FROM 절의 하위 쿼리 결과일 수 있습니다. |
| subqueryN | id 값이 N인 행에 대한 구체화된 하위 쿼리의 결과를 나타냅니다. |



4. type

| type            | meaning                                                      |
| --------------- | ------------------------------------------------------------ |
| system          | 테이블에 단 한개의 데이터만 있는경우 (드문 경우이다..)       |
| const           | SELECT 에서 Primary Key 혹은 Unique Key 를 사용해서 조회하는 경우 |
| eq_ref          | 조인을 할때 Primary Key                                      |
| ref             | 조인을 할 때 Primary Key 혹은 Unique Key 가 아닌 Key로 매칭하는경우 |
| ref_or_null     | ref 와 같지만 null 이 추가되어 검색되는 경우                 |
| index_merge     | 두 개의 인덱스가 병합되어 검색이 이루어지는 경우             |
| unique_subquery | IN 절 안의 서브쿼리에서 Primary Key가 오는 특수한경우        |
| index_subquery  | 위와 비슷하지만 Primary Key 가 아닌 인덱스인 경우            |
| range           | 특정 범위 내에서 인덱스를 사용하여 원하는 데이터를 추출하는 경우 |
| index           | 인덱스를 처음부터 끝까지 찾아서 검색하는경우, (인덱스 풀스캔) |
| all             | 테이블을 처으부터 끝까지 검색하는 경우, (테이블 풀 스캔)     |

5. possible_keys : 쿼리에서 검색중 사용할수 있는 키
6. key : 해당 쿼리 연산에서 사용한 인덱스를 나타낸다
7. key_len : 해당 쿼리 연산에서 사용한 키의 길이를 나타낸다
8. ref : 키 컬럼에 나와 있는 인덱스에서 값을 찾기 위해 선행 테이블의 어떤 컬럼이 사용되었는지 나타냄.
9. rows: 쿼리 연산에서 원하는 행을 찾기 위해 총 검색해야 하는 rows의 예측값 
10. extra

 

| extra           | meaning                                                      |
| --------------- | ------------------------------------------------------------ |
| using index     | 인덱스 자료 구조를 이용해서 데이터 검색 및 추출              |
| using where     | where 조건으로 데이터 추출. type 이 all 혹은 index 타입과 함께 표현되면 성능 개선이 필요함. |
| using filesort  | 데이터 정렬이 필요한 경우로 메모리 혹은 디스크상에서 정렬을 모두 포함.<br />데이터가 많은 경우 성능에 영향을 줌...조심! |
| using temporary | 쿼리 처리시 내부적으로 temporary table 이 사용되는 경우를 의미 |

EXPLAIN 함수를 활용하여 쿼리의 연산 계획을 살필수 있으며 TYPE ALL 일경우 

ROWS	의 숫자가 크다면 쿼리의 개선이 필요로한다.



