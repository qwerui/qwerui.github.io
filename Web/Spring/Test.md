# Test

## 개요
Spring은 JAVA 프레임워크이기 때문에 JUnit을 테스트 도구로 사용한다. assert계열 메소드를 통과해야 정상 코드, 통과하지 못하면 오류 발생

## First
- Fast : 테스트는 빠르게 실행되어야한다.
- Isolated : 테스트는 서로 독립적이어야한다.
- Repeatable : 테스트는 반복할 수 있어야한다.
- Self-validating : 테스트가 스스로 성공/실패를 검증할 수 있어야한다.
- Timely : 테스트코드는 코드 작성 직후 혹은 코드 작성과 동시에 작성되어야한다.

## 관련 어노테이션
### 공통
- @Test : 테스트용 메소드 지정

### JUnit4
- @Before/After : 테스트 메소드 실행 전/후 수행할 메소드
- @Before/AfterClass : 테스트 실행 전/후 실행할 메소드
- @RunWith : 매개변수로 SpringJUnit4ClassRunner.class 클래스가 들어감, 실행환경을 설정, 주로 테스트 코드에 사용
- @WebAppConfiguration : 웹 애플리케이션 컨텍스트를 로드하여 Spring MVC 환경을 테스트할 때 사용. 주로 웹 관련 설정과 빈들을 테스트하는 데 사용됩니다.
- @ContextConfiguration: 일반적인 Spring 애플리케이션 컨텍스트를 로드하여 테스트 환경을 설정할 때 사용. XML 설정 파일이나 자바 기반 설정 클래스를 통해 설정을 지정할 수 있습니다

### JUnit5
- @SpringBootTest : 스프링 부트 테스트 클래스 지정, 없으면 빈을 인식하지 못한다.
- @BeforeEach/AfterEach : 테스트 메소드 실행 전/후 수행할 메소드
- @BeforeAll/AfterAll : 테스트 실행 전/후 실행할 메소드
- @ExtendsWith : 매개변수로 SpringExtension.class 클래스가 들어감, 실행환경을 설정, 주로 테스트 코드에 사용
- @Timeout : 테스트 코드의 만료 시간 지정, value로 시간 값, unit으로 단위를 지정한다.
