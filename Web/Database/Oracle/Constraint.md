# Constraint

## 제약 조건

테이블에 부적절한 자료가 입력되는 것을 방지하기 위해 각 컬럼에 대해 정의하는 규칙, 제약 조건의 결과가 TRUE 혹은 NULL인 경우 작업을 허용하며 테이블에 대해 정의되고 데이터 딕셔너리에 저장된다. 일시적으로 활성화/비활성화 할 수 있다.

- 기본 제약조건
1. NN : NOT NULL, NULL값 불가능
2. UK : UNIQUE, 값이 고유해야함 ⇒ NOT IN(VALUES)⇒NULL은 중복으로 입력가능
3. PK : PRIMARY KEY, 기본키
4. CK : CHECK, 조건에 맞는 데이터만 가능
5. FK : FOREIGN KEY, 외래키

```sql
--제약조건 정의, 이름을 생략하면 자동으로 생성한다
column [CONSTRAINT CONSTRAINT_NAME] CONSTRAINT_TYPE --컬럼 레벨
[CONSTRAINT CONSTRAINT_NAME] CONSTRAINT_TYPE(column, ...) --테이블 레벨

--제약조건 확인
SELECT * FROM USER_CONSTRAINTS;
SELECT * FROM USER_CONS_COLUMNS;
```

## FK

외래키, 두 테이블 간의 참조 관계를 선언하는 제약조건

자식 테이블은 부모 테이블의 컬럼 값 중 하나이거나 NULL이어야한다.

```sql
--제약조건 설정
column [CONSTRAINT CONSTRAINT_NAME] REFERENCES TABLE_NAME(COLUMN_NAME)
[CONSTRAINT CONSTRAINT_NAME] FOREIGN KEY (COLUMN_NAME) REFERENCES TABLE_NAME(COLUMN_NAME)
```

### FK의 DML

- INSERT : 자식 테이블은 부모의 값 또는 NULL이어야한다
- UPDATE : 자식 테이블에서 참조하고 있는 컬럼은 부모 테이블에서 수정할 수 없다.
- DELETE : 자식 테이블에서 참조하고 있는 컬럼은 아무조건 없이 삭제할 수 없다. 자식 테이블은 얼마든지 삭제 가능
    - ON DELETE CASCADE : 자식도 같이 삭제
    - ON DELETE SET NULL : 자식은 NULL로 설정
    

## 제약 조건 관리

주로 ALTER TABLE로 관리한다.

- 추가 : ADD, 추가는 한 번에 여러개가 가능하다.
- 수정 : MODIFY
- 이름 변경 : RENAME

## 제약 조건 상태

기본 상태 : ENABLE VALIDATE NOT DEFERRABLE INITIALLY IMMEDIATE

상태 변경 구문 : MODIFY 절, ENABLE/DISABLE 절

- ENABLE : 새로운 데이터에 무결성 제약조건을 적용, PK,UK에 인덱스가 없으면 인덱스 자동생성
- DISABLE : 테이블에 무결성 제약 조건을 적용하지 않음, PK,UK가 자동인덱스라면 자동 삭제
- VALIDATE/NOVALIDATE : 이미 들어있는 데이터에 제약 조건 적용할지 여부, PK는 PK제약조건을 만족하지 않는 데이터가 있다면 DISABLE NOVALIDATE만 가능

| 조합 | 설명 |
| --- | --- |
| ENABLE VALIDATE | - 기본 설정
- 테이블의 모든 제약 조건이 이 상태이어야 테이블의 데이터가 무결함
- 기존의 데이터와 새로운 데이터 모두 제약 조건의 적용을 받음
- 모든 데이터가 제약 조건을 만족해야만 이 상태로 변경가능 |
| ENABLE NOVALIDATE | - 새로운 데이터는 제약 조건을 따라야함
- 기존의 데이터는 제약 조건을 만족하지 않아도 됨 |
| DISABLE VALIDATE | - DML 불가
- 인덱스 자동 삭제 또는 인덱스 크기가 증가하지 않음
- Data Warehouse에서 다량의 데이터를 데이터베이스에 적재할 때 유용 |
| DISABLE NOVALIDATE | - 제약 조건을 유지하는데 노력을 들이지 않고 제약조건이 참인지 보장하지 않음
- 다른 상태에서 언제나 이 상태로 설정 가능 |

## FK 자동 삭제/비활성화

- FK로 인해 실패하는 DDL작업⇒CASCADE, CASCADE CONSTRAINT로 해결
1. 부모 테이블 삭제 ⇒ FK 자동 삭제, 자식 테이블은 삭제되지 않고 제약 조건도 같이 삭제되어야하므로 CONSTRAINT 키워드 필요
2. 부모 컬럼 삭제 ⇒ FK 자동 삭제, 자식 컬럼은 삭제되지 않음
3. 참조되는 PK, UK제약조건 삭제 ⇒ FK 자동 삭제, 같은 종류인 제약 조건을 삭제하므로 CONSTRAINT 키워드 불필요
4. 참조되는 PK, UK제약조건 비활성화 ⇒ FK 자동 비활성화, PK,UK를 활성화하더라도 참조하고 있던 값이 삭제되거나 변경됐을 수 있기 때문에 FK는 자동으로 활성화되지 않음.