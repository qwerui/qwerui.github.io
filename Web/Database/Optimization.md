# Optimization

## 튜닝 시 알면 좋은 것
1. 기본적으로 탐색 범위를 최대한 줄이는 방향으로 튜닝하는 것이 좋다. 
2. 전체 테이블에 비해 소량의 데이터를 조회하는 경우(5% 이하 정도)는 테이블 풀 스캔보다 인덱스 스캔이 효율적이다.
3. Update 시 인덱스도 영향을 받는다. 대규모의 Update가 필요할 시에는 인덱스 수정에 의해 오랜 시간이 걸릴 수 있다.


## 튜닝 대상 기준 (실행계획)
||Good|Bad|
|---|---|---|
|select_type|Simple, Primary, Derived|dependent, uncacheable|
|type|system, const, eq_ref|index, all|
|extra|Using index|Using filesort, Using temporary|

- select_type
    - SIMPLE : 내부 쿼리가 없는 SELECT
    - PRIMARY : 서브쿼리가 포함된 쿼리의 첫 번째 SELECT 쿼리
    - SUBQUERY : 독립적으로 수행되는 서브쿼리
    - DERIVED : 인라인 뷰
    - UNION : UNION에서 첫 번째 SELECT를 제외한 나머지
    - UNION RESULT : UNION ALL이 아닌 쿼리로 UNION을 수행했을 경우
    - DEPENDENT SUBQUERY : 메인 테이블에 영향을 받는 서브쿼리
    - DEPENDENT UNION : 메인 테이블에 영향 받는 UNION
    - UNCACHEABLE SUBQUERY : 서브 쿼리 안에 사용자 정의 함수나 변수가 포함되거나 조회시 마다 값이 변경되는 경우
    - MATERIALIZED : IN 절의 서브쿼리
- type
    - system : 데이터가 없거나 1개만 있는 경우
    - const : 데이터가 1건만 조회되는 경우
    - eq_ref : 고유 인덱스 혹은 기본 키로 단 1건의 데이터를 조회
    - ref : driven table의 접근 범위가 2건 이상인 경우
    - ref_or_null : IS NULL 구문에 의한 인덱스 활용, NULL 데이터가 많다면 튜닝 대상이 될 수 있다.
    - range : 테이블 내의 연속된 범위 조회, BETWEEN이나 비교 연산 등
    - fulltext : 텍스트 검색 전용 전문 인덱스
    - index_merge : 두 개 이상의 인덱스가 병합되어 적용
    - index : 인덱스 풀 스캔
    - all : 테이블 풀 스캔

## 튜닝 대상이 될 만한 쿼리
- 기본 키를 변형하는 쿼리
- 사용하지 않는 함수를 포함하는 쿼리
- 형변환으로 인덱스를 활용하지 못하는 쿼리 (컬럼과 타입이 일치하지 않는 WHERE 문)
- 열을 결합하여 사용하는 쿼리
- 의미없는 중복제거를 사용하는 쿼리
- 다수 쿼리를 UNION으로만 합치는 쿼리
- 인덱스를 고려하지 않는 쿼리
- 작은 테이블이 먼저 조인에 참여하는 쿼리
- 메인 테이블에 의존하는 쿼리
- 불필요한 JOIN을 사용하는 쿼리