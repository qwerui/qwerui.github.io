# Mybatis

## 개요

ORM의 일종, Mybatis를 이용하면 리포지토리가 DB에 종속적이게 되지 않아 DB를 변경하더라도 그대로 사용이 가능하다.

## Mybatis 사용

```xml
	<!--root-context.xml-->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<!--스캔할 경로 설정-->
		<property name="mapperLocations" value="classpath:mapper/**/*.xml"/>
	</bean>
```

```xml
<!--Mybatis를 사용하는 경우 인터페이스만 만들고 구현 클래스를 만들 필요가 없다-->
<!--SQL구문에서 SQL developer에서 처럼 ;을 붙이지 말자-->
<mapper namespace="리포지토리 인터페이스">
    <!--Param 어노테이션이 있기에 parameterType은 생략하는 것이 좋다.-->
	<select id="메소드명" parameterType="파라미터타입" resultType="반환타입">
		SELECT COUNT(*) FROM TABLE
		<if test="조건식">
			<!--넘어온 파라미터는 #{파라미터명}으로 사용할 수 있다.-->
			 WHERE ID = #{파라미터}
		</if>
	</select>
	<resultMap id="map" type="모델 클래스">
		<result property="필드명" column="컬럼명" />

	</resultMap>
	
	<!--update,delete,insert도 같은 형식-->
    <!--컬럼명과 VO의 변수명이 같으면 자동으로 Setter를 호출해준다.-->
	<select id="메소드명" resultMap="map">
		SELECT * FROM TABLE
	</select>
</mapper>
<!--sql구문을 재사용하기 위해서는 sql 태그를 사용한다.-->
```

```java
//구현체가 존재하지 않는다.
public interface IEmpRepository {
	int getCount();
	int getCount(int id);
	List<VO> getList();
	VO getInfo(int id);
	void updateVO(VO vo);
	void insertVO(VO vo);
	int deleteEmp(@Param("id") int id, @Param("email") String email);
	List<Map<String, Object>> getAll();
}
```

## 동적 SQL

JSTL과 비슷한 문법으로 if, choose, when, else를 제공하고, 추가로 where문과 set문을 작성해주는 where, set 태그를 지원한다. 그외 trim, foreach를 지원.

## Mybatis에서 CDATA 사용 이유
<![CDATA[쿼리문]]>에서의 쿼리문은 문자열로 그대로 치환된다. 즉 부등호를 사용해도 태그 괄호로 인식하지 않는다.