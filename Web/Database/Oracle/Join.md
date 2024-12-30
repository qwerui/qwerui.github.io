# Join

## 조인 유형
- 크로스 조인 : 조인 조건이 없는 조인 m*n의 결과
- 내부 조인 : 교집합, JOIN의 기본값
- 자연 조인 : 이름이 같고 데이터 형이 호환되는 컬럼끼리 동등 조인
- 외부 조인 : 조인 조건을 만족하지 않는 행도 새로운 재료 집합에 포함

## 문법
```sql
--크로스 조인
SELECT last_name, department_name
FROM employees CROSS JOIN departments;

--내부 조인
SELECT e.last_name, d.department_name
FROM employees e JOIN departments d
ON e.department_id = d.department_id;

--자연조인 USING(컬럼명)으로 자연조인할 컬럼 한정 가능
SELECT last_name, department_name
FROM employees NATURAL JOIN departments;

--외부 조인
SELECT e.last_name, d.department_name
FROM employees e [LEFT/RIGHT/FULL] OUTER JOIN departments d
ON e.department_id = d.department_id;

```