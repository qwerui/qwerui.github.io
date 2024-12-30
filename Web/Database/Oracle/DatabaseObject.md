# Database Object

## 데이터베이스 객체 목록
- 테이블 : 기본 저장 단위, 행으로 구성
- 뷰 : 하나 이상의 테이블의 부분 집합을 논리적으로 구성
- 시퀀스 : 숫자 값 생성
- 인덱스 : 일부 query의 성능 향상 목적
- 동의어 : 객체에 다른 이름 부여

### 이름 지정 규칙
- 문자로만 시작, 문자와 숫자 가능, 특수문자는 _, $, #만 가능
- 길이 최대 30자
- 예약어 사용 불가
- 큰 따옴표 사용 시 길이 제한을 제외한 규칙 무시
- 같은 이름 공간에 동일한 이름 존재 불가능

## 테이블
```sql
-- 기본적인 테이블 생성
CREATE TABLE TABLE_NAME(
    column_1 NUMBER,
    column_2 NUMBER DEFAULT 1,
    column_3 NUMBER [CONSTRAINT...],
    [TABLE_CONSTRAINT...]
);

-- 쿼리 결과로 생성
CREATE TABLE TABLE_NAME AS
SELECT...

-- 테이블 수정
ALTER TABLE TABLE_NAME [OPTIONS...]

--옵션 예시
RENAME TO : 테이블 이름 변경
READ ONLY : 읽기 전용
READ WRITE : 읽기 쓰기
ADD : 컬럼 추가
MODIFY : 컬럼 수정(빈 테이블이 아닌 경우 데이터 타입 변경 불가, 크기 변경 및 기본값 변경만 가능)
RENAME COLUMN : 컬럼명 수정
DROP COLUMN : 컬럼 삭제

--테이블 삭제, 테이블을 삭제하면 데이터베이스에서 제거되는 것이 아니라 삭제된 테이블이 recyclebin의 공간으로 인식된다.
DROP TABLE TABLE_NAME [OPTIONS];

--옵션 예시
PURGE : 완전 제거, FLASKBACK으로 복구 불가

--테이블 복구, 종속 객체는 Recycle Bin에 있던 이름으로 복원되기 때문에 복원 후 이름을 변경해야한다.
--참조 제약 조건또한 복원되지 않아 직접 추가해야한다.
FLASHBACK TABLE TABLE_NAME TO BEFORE DROP;
```

## 뷰
데이터의 논리적 부분 집합 또는 조합을 나타내는 스키마 객체, 쿼리에 이름을 붙인 것으로 볼 수 있으면 데이터 딕셔너리에 SELECT문으로 저장되어 필요 시에 언제든지 호출 할 수 있다.

**뷰 사용 시 이점**   
1. 컬럼을 선택적으로 표시해 데이터 액세스 제한→보안 향상
2. 쿼리 단순화
3. 데이터 독립성 제공
4. 하나의 테이블로 여러 뷰를 생성할 수 있음

```sql
--뷰 생성
CREATE [OR REPLACE] [FORCE|NOFORCE] VIEW VIEW_NAME
[(alias[, alias]...)]
AS SUBQUERY
[WITH CHECK OPTION [CONSTRAINT CONSTRAINT_NAME]]
[WITH READ ONLY [CONSTRAINT CONSTRAINT_NAME]];

--뷰를 생성하면 구조를 수정할 수 없다. 
--OR REPLACE로 재정의하거나 삭제하고 다시 생성해야한다.

--뷰 수정(제약조건이나 읽기 권한 등만 수정할 수 있다.)
ALTER VIEW VIEW_NAME ADD/MODIFY/DROP/READ ...;

--뷰 삭제
DROP VIEW VIEW_NAME;

--뷰 정의 시 컬럼 별칭 지정
--1. 서브쿼리에서 AS로 지정
--2. 뷰 이름 다음에 ()로 컬럼 별칭 정의
```

## 시퀀스

일련의 고유 정수를 생성하는 스키마 객체

- 고유 번호를 자동으로 생성
- 유저, 객체 간 공유 가능
- 메모리에 캐시될 경우 액세스 속도 향상
- 시퀀스 갭 발생 가능성 상존
- Primary Key, Unique Key 값을 생성하는데 유용
- 응용 프로그램 코드 대체 가능

```sql
--시퀀스 생성
CREATE SEQUENCE SEQUENCE_NAME
	[INCREMENT BY N]                 --시퀀스 증감, 기본 1
	[START WITH N]                   --시퀀스 시작 값, 기본 1
	[{MAXVALUE N | NOMAXVALUE}]      --시퀀스 최댓값, 기본 NOMAXVALUE
	[{MINVALUE N | NOMINVALUE}]      --시퀀스 최솟값, 기본 NOMINVALUE
	[{CYCLE | NOCYCLE}]              --시퀀스 사이클, 최대->최소, 최소->최대
	[{CACHE N | NOCACHE}];           --메모리에 캐시할 시퀀스 개수, 기본 20
	
--시퀀스 참조
SEQUENCE_NAME.NEXTVAL --사용 가능한 시퀀스 값 반환, 반환 후 증감 발생
SEQUENCE_NAME.CURRVAL --현재 생성된 시퀀스 값 반환, NEXTVAL 사용 후 사용

--시퀀스 수정(이미 캐시된 시퀀스 값에는 영향을 미치지않는다.)
ALTER SEQUENCE ...
```

### 시퀀스 의사컬럼

시퀀스 값 참조를 위한 의사컬럼

- NEXTVAL : 사용가능한 시퀀스 값을 행당 하나씩 생성하여 반환(행에 여러번 사용해도 1번만 실행)
- CURRVAL : 현재 생성된 시퀀스 값 반환 NEXTVAL을 한번이라도 사용한뒤 사용해야한다.

**시퀀스 의사컬럼 사용 가능 영역**   

- 서브 쿼리의 일부가 아닌 SELECT 문의 select-list
- INSERT문에서 서브쿼리의 select-list
- INSERT문에서 VALUES절
- UPDATE문에서 SET절

**시퀀스 의사컬럼 사용 불가 영역**   

- 뷰의 select-list
- DISTINCT 키워드가 존재하는 SELECT문
- GROUP BY, HAVING, ORDER BY 절이 있는 SELECT문
- SELECT, DELETE, UPDATE문의 서브쿼리
- CREATE/ALTER TABLE문의 DEFAULT 식

## 인덱스

테이블에 저장된 행을 빠르게 찾기위한 스키마 객체, 인덱스 키 값과 포인터가 저장된 일종의 테이블, 키 값은 오름차순으로 정렬된다.

- 자동 인덱스 : 테이블에 PK, UK를 정의할 때 컬럼에 인덱스가 없다면 자동으로 생성
- 수동 인덱스 : CREATE INDEX문 사용, UNIQUE를 생량하면 비고유 인덱스
- 유니크 인덱스 : 키 값이 고유함, 인덱스 블록을 1번만 접근한다.
- 넌유니크 인덱스 : 키 값 중복 허용, 인덱스 블록에 필요한 접근 횟수 + 1번 접근한다.

```sql
--인덱스 생성
CREATE TABLE ... (...PRIMARY KEY USING INDEX(
	CREATE INDEX INDEX_NAME ON TABLE_NAME(COLUMN_NAME))...);

CREATE [UNIQUE] [BITMAP] INDEX INDEX_NAME
ON TABLE_NAME(COLUMN_NAME[, COLUMN_NAME]...);

--인덱스 삭제(인덱스는 테이블 삭제 시 같이 삭제되면 테이블 복구 시 같이 복구된다.)
DROP INDEX INDEX_NAME;

--인덱스 변경
ALTER INDEX INDEX_NAME OPTIONS

--옵션 예시
RENAME TO : 인덱스 이름 변경
```

### 인덱스 생성 지침

- 인덱스 생성 권장
1. 열에 광범위한 값이 포함된 경우
2. 열에 NULL이 많이 포함된 경우
3. 하나 이상의 열이 WHERE절이나 조인 조건에서 자주 사용되는 경우
4. 테이블이 크고 대부분의 쿼리가 테이블에서 2~4% 미만의 행을 검색할 것으로 예상되는 경우

- 인덱스 생성 지양
1. 열이 쿼리에서 조건으로 자주 사용되지 않는 경우
2. 테이블이 작거나 대부분의 쿼리가 테이블에서 2~4% 이상의 행을 검색할 것으로 예상되는 경우
3. 테이블이 자주 갱신되는 경우
4. 인덱스화된 열이 표현식의 일부로 참조되는 경우

## 동의어

데이터베이스 객체의 별칭, 객체의 긴 이름을 짧게 만들거나 다른 유저 소유의 객체게 쉽게 접근하거나 다른 유저 소유의 객체 접근시 유저 이름을 숨기고 싶을 때 사용한다.

```sql
--동의어 생성(PUBLIC은 모든 유저가 사용할 수 있는 동의어를 생성한다)
CREATE [PUBLIC] SYNONYM SYNONYM_NAME
FOR OBJECT_NAME;

--동의어 삭제
DROP SYNONYM SYNONYM_NAME;

--동의어 확인
SELECT *
FROM USER_SYNONYMS;
```