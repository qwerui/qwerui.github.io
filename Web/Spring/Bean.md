# Bean

## Spring Bean

스프링 빈(SpringBean)은 스프링 프레임워크의 핵심 개념 중 하나로,스프링 IoC(Inversionof
Control)컨테이너에 의해 관리되는 객체를 의미합니다.스프링 빈의 주요 특징은 다음과
같습니다.

1. 객체의 생명 주기 관리:스프링 컨테이너는 빈의 생성,초기화,사용,소멸까지의 생명
주기를 관리합니다.
2. 의존성 주입(DependencyInjection):빈은 다른 빈들과의 의존 관계를 설정할 수 있습니다.
의존성 주입을 통해 객체 간 결합도를 낮추고,코드의 유연성과 테스트 용이성을 높입니다.
3. 싱글톤 스코프 기본:기본적으로 모든 빈은 싱글톤(Singleton)스코프로 생성됩니다.즉,
컨테이너 내에서 한 번 생성된 빈은 컨테이너가 종료될 때까지 하나의 인스턴스만
존재합니다.필요에 따라 프로토타입(Prototype)스코프 등의 다른 스코프를 설정할 수
있습니다.
4. 초기화 시점의 빈 생성(EagerInstantiation):스프링은 기본적으로 애플리케이션
컨텍스트가 로드될 때 모든 싱글톤 빈을 미리 생성(EagerInstantiation)합니다.이는
애플리케이션의 시작 속도를 늦출 수 있지만,런타임 시점에서의 성능을 높이는 데 도움이
됩니다.
5. 설정 방법의 유연성:스프링 빈은 XML설정 파일,자바 어노테이션,자바 설정
클래스(@Configuration)등 다양한 방법으로 설정할 수 있습니다.
6. 자동 주입(Auto-wiring):스프링은 빈 간의 의존성을 자동으로 주입할 수 있는 기능을
제공합니다.@Autowired,@Inject,@Resource어노테이션 등을 사용하여 자동 주입을 설정할
수 있습니다.
7. POJO(PlainOldJavaObject)지원:스프링 빈은 특별한 클래스를 상속받거나 특정
인터페이스를 구현할 필요가 없습니다.일반적인 자바 객체를 스프링 빈으로 사용할 수
있습니다.
8. AOP(Aspect-OrientedProgramming)통합:스프링은 AOP기능을 통해 빈에 부가적인
기능(로깅,보안,트랜잭션 등)을 쉽게 추가할 수 있습니다.
9. 유연한 빈 정의:스프링은 FactoryBean인터페이스를 통해 복잡한 빈의 생성 과정을
커스터마이즈할 수 있도록 지원합니다.
10. 환경 설정 및 프로파일 지원(EnvironmentandProfiles):스프링은 다양한 환경 설정 및
프로파일을 지원합니다.이를 통해 개발,테스트,운영 등 다양한 환경에 따라 빈의 구성을
다르게 할 수 있습니다.@Profile어노테이션을 사용하여 특정 프로파일에서만 빈이
생성되도록 설정할 수 있으며,application.properties또는 application.yml파일을 통해
환경별 설정을 관리할 수 있습니다.

Bean을 정의하는 방법은 2가지가 있다.

```xml
<!-- XML을 통한 정의 -->
<bean name="Bean 이름" class="패키지를 포함한 클래스 경로">
</bean>
```

```java
//어노테이션을 통한 정의
@Component("Bean 이름")
public class SampleBean{
	//fields...
}
```

 java 코드에 Bean을 injection하는 방법은 2가지가 있다.

```xml
<!-- XML을 통한 주입 -->
<bean name="Bean 이름" class="패키지를 포함한 클래스 경로">
	<constructor-arg index="인덱스(생략하면 타입 비교로 주입)" ref="참조할 Bean name"/> <!-- 생성자 주입 -->
	<property name="변수 이름" ref="참조할 Bean name"/> <!-- Setter 주입 -->
	<property name="변수 이름" value="값"/>
</bean>
```

```java
//어노테이션을 통한 주입
//Spring 제공 어노테이션
@Autowired 
@Qualifier("Bean 이름")

//Spring 외 프레임워크 호환 어노테이션
@Inject
@Named("Bean 이름")

//이름 우선 탐색 어노테이션
@Resource
```