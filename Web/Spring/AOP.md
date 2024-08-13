# AOP

## Aspect

메인 로직과는 별개로 공통적으로 사용하는 기능들을 자동으로 실행

### 사전 준비

```xml
<!--pom.xml-->
<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

<!-- AspectJWeaver -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>
```

### 작동 시점(Join Point)

매개변수로 JoinPoint를 받을 수 있으며, 실행 메소드의 정보가 들어있다.

- @Before : 메소드 실행 전
- @After : 메소드 실행 후
- @Around : 메소드 실행 전후, 메소드에 ProceedingJoinPoint 매개변수가 존재해야하며, preceed메소드로 원래 수행하려던 작업을 수행하고 결과를 리턴해야한다.(Object 타입)
- @AfterThrowing : 예외 발생 후
- @AfterReturning : 값 반환 후

### 작동 메소드 지정(Pointcut)

- 기본 형식 : execution(ret-type package.class.method(param))

각 경로 및 시그니처에 와일드카드를 사용할 수 있으며, 문자열과 와일드카드를 결합해 사용할 수 있다.  매개변수의 경우 ..은 0개 이상의 파라미터다.