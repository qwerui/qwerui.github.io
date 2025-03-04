# SQL
## SQL 문의 분류
- DDL : 데이터베이스 객체를 조작하는 명령어
- DML : 데이터를 조작하는 명령어
- DCL : 데이터베이스 권한을 조작하는 명령어
- TCL : 트랜잭션(작업의 최소 수행 단위)을 제어하는 명령어

## SELECT
```sql
--데이터를 조회하는 명령어(명령어 옆의 숫자는 처리 순서)
--SELECT, GROUP BY, HAVING은 처리 순서가 뒤바뀔 수 있다.
SELECT      --5
FROM        --1
WHERE       --2 NOT - AND - OR 순으로 연산
GROUP BY    --3
HAVING      --4
ORDER BY    --6
```

### 다량의 데이터의 일부를 SELECT 최적화
1. Primary Key에 대한 의미없는 조건절을 추가한다. 이는 인덱스를 통해 SELECT하도록 할 수 있다. /*+ INDEX_DESC(테이블명 제약조건)*/을 지정하면 역순으로 인덱스를 탈 수 있다. 

## SQL 연산자
- 산술연산 : +, -, *, /
- 연결연산 : ||
- 비교연산 : =, >, <, >=, <=, <>, BETWEEN AND, IN, LIKE, IS NULL
- 논리연산 : AND, OR, NOT
- 다중 행 비교 연산 : ANY, ALL, EXISTS
- 집합 연산자 : UNION, UNION ALL, INTERSECT, MINUS

※LIKE에서 %나 _를 검색하고 싶을 경우 ESCAPE '이스케이프 문자'를 사용한다.

## 의사컬럼
SELECT는 가능하지만 DML은 불가능한 컬럼, 어느 테이블이던 사용가능
- ROWID : 행의 주소를 나타내는 의사컬럼
- ROWNUM : SELECT 된 행의 순서를 나타내는 의사컬럼

## 함수
### 문자함수
- LOWER/UPPER : 소문자/대문자로 변환
- INITCAP : 첫 번째 문자 대문자, 이후 소문자로 변환
- CONCAT : 문자열 연결
- SUBSTR : 문자열 자르기
- LENGTH : 문자열 길이
- INSTR : INDEXOF
- LPAD : 정해진 길이 중 남는 공간의 왼쪽에 문자 삽입
- RPAD : 정해진 길이 중 남는 공간의 오른쪽에 문자 삽입
- REPLACE : 문자열 대체
- TRIM : 양 끝 문자 제거

### 숫자함수
- ROUND : 반올림
- TRUNC : 버림
- CEIL : 소수점 올림
- FLOOR : 소수점 버림

### 날짜함수
- MONTHS_BETWEEN : 두 날짜 간의 개월 수
- ADD_MONTHS : 날짜에 월 추가
- NEXT_DAY : 지정 날짜의 다음 날
- LAST_DAY : 지정 월의 마지막 날
- ROUND : 날짜 반올림
- TRUNC : 날짜 버림

### 타입 변환함수
- TO_NUMBER : 문자->숫자
- TO_CHAR : 숫자, 날짜 -> 문자
- TO_DATE : 문자 -> 날짜

### NULL 함수
- NVL : 결과가 NULL일 경우 특정 값으로 변환
- NVL2 : 조건을 입력해 NULL일 경우와 아닌 경우를 지정
- COALESCE : NULL이 아닌 첫 번째 값 반환
- NULLIF : 두 표현식을 비교해 같으면 NULL 반환
- LNNVL : 조건식의 결과가 거짓 또는 NULL일 경우 참 반환

### 그룹함수
- SUM/AVG/STDDEV/VARIANCE : 합, 평균, 표준편차, 분산
- MIN/MAX/COUNT : 최솟값, 최댓값, 개수
- DISTINCT : 중복 제거
- ROLLUP : 첫 번째 인수에 대한 소그룹 합계
- CUBE : 모든 그룹별 합계
- GROUPING SETS : 각각의 컬럼을 기준으로 GROUP BY 합계

### 기타함수
- DECODE : IF ELSE, DECODE(컬럼, [조건값, 반환값], ELSE값)
- RANK, DENSE_RANK : 순위 반환 RANK는 중복 발생 시 동일 순위를 부여하고 그 개수만큼 순위를 건너뜀, DENSE_RANK는 건너뛰지 않음. RANK() OVER (ORDER BY [조건])으로 사용
- PARTITION BY : 컬럼으로 그룹화
- OVER : 분석함수로 변환, GROUP BY 없이 통계함수를 사용할 수 있다. 결과는 각 행에 저장된다.
- NTILE : 전체 결과를 N등분 후 각 결과에 순번 부여
- LAG : N번째 이전 행 값을 가져옴

## 조건부 표현식
```sql
--CASE 문
CASE column WHEN ... THEN ... ELSE ... END
--DECODE 함수
DECODE([CASE WHEN THEN ELSE문을 키워드 없이 ,으로 구분해 사용])
```

## 트랜잭션 처리
- COMMIT : DML 작업에 문제가 없어 데이터베이스에 반영
- ROLLBACK : DML 작업에 문제가 생겨 트랜잭션 시작 전으로 되돌림
- SAVEPOINT : 트랜잭션 중간으로 되돌릴 수 있도록 중간 지점을 설정

## MERGE
```sql
MERGE INTO 변경테이블
USING (서브쿼리)
ON (조건문) -- USING에서의 서브쿼리 결과과 비교하기 위한 조건문
WHEN MATCHED THEN
    조건 만족 시 수행할 구문
WHEN NOT MATCHED THEN
    조건 불만족 시 수행할 구문
```

## 계층형 쿼리
```sql
SELECT LEVEL -- LEVEL이 계층 단계
...
START WITH [조건] -- 루트 지정
CONNECT BY PRIOR [조건] -- 연결 지정, PRIOR는 부모 행의 조건, 없으면 현재 행의 조건
```