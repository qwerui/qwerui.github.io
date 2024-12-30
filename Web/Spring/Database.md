# Database

## JDBC Template

```xml
    <!-- 보통 root-context에 기재 -->
	<!-- 데이터소스 -->
	<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
		<property name="driverClassName" value="드라이버 클래스"/>
		<property name="url" value="jdbc:데이터베이스URL"/>
		<property name="username" value="사용자 이름"/>
		<property name="password" value="비밀번호"/>
	</bean>
	
	<!-- JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"/>
	</bean>
```

```java
// args에는 sql의 ?에 해당하는 값을 나열한다. Object[]로도 가능
int update(String sql, Object... args) //삽입, 수정, 삭제
List<T> query(String sql, RowMapper<T> rowMapper, Object... args) //다중 행 조회
T queryForObject(String sql, Class<T> requiredType, Object... args) //단일 행 조회
List<Map<String, Object>> queryForList(String sql, Object... args) //컬럼명:값의 리스트
```

insert는 void로 return해도 되지만(에러를 발생시키므로), update나 delete는 int로 return해 반영된 레코드의 개수를 파악하는 것이 좋다.
