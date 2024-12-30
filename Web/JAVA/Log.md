# Log

## Log4J

단계적으로 로그를 출력할 수 있게 해주는 라이브러리, Log4J.xml에서 최소 출력 단계를 설정할 수 있다.

## Log4Jdbc

DB 작업 처리 로그를 출력해준다.
```xml
<!--root-context.xml-->
	<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
		<property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy" />
		<property name="url" value="jdbc:log4jdbc:oracle:thin:@localhost:1521:xe" />
		<property name="username" value="사용자" />
		<property name="password" value="비밀번호" />
	</bean>
```
```xml
<!--log4j.xml-->
<logger name="org.springframework.jdbc">
	<level value="info"/>
</logger>
```