# Spring

## 개요
JAVA 환경 웹 어플리케이션 프레임워크, DI를 통해 IoC를 구현함으로써 의존성을 관리하기 용이하고, AOP를 이용해 메인 로직에 같이 사용되는 공통 로직들을 관리하기 용이하다.   
어플리케이션에서 사용되는 객체들은 빈으로 불리며, 스프링 컨테이너에서 이 빈들의 생명주기와 의존성을 관리한다.

## 설치(Eclipse)
1. eclipse 2020-09 버전 이하를 사용한다.
2. 마켓플레이스에서 sts3 add on을 설치한다.
3. Preference의 Server에서 사용할 Tomcat을 지정한다. 이때 Tomcat은 9.0이하여야한다.
4. Maven Repositories의 Global Repositories의 Central을 우클릭해서 Rebuild한다.

## IoC & DI
- IoC : 제어의 역전
- DI : 의존성 주입

Spring은 프레임워크다. 따라서 객체 등의 관리를 개발자가 관리하지 않고, Spring에서 관리한다. 이는 코드의 결합도를 낮추고 유지보수하기 용이하게 해준다. Spring에서는 이를 DI를 통해 구현한다. DI는 외부에서 의존성을 주입한다는 의미로, 빈의 의존성에 따라 Spring 컨테이너가 생성자 혹은 Setter를 통해 적절한 빈을 생성해 할당해준다.

## Application Context

스프링 프레임워크에서 ApplicationContext는 특정한 역할을 하는 인터페이스로
애플리케이션에서 사용할 모든 빈의 생성,초기화,소멸을 관리합니다.이를 통해 객체의 생명주기를 효과적으로 제어할 수 있습니다.또한,애플리케이션 컨텍스트는 빈 간의 의존성을
설정하고 주입하여 코드의 결합도를 낮추고 유연성을 높입니다.
<br/>
애플리케이션 컨텍스트는 다양한 설정 방법을 지원합니다.XML설정 파일,자바 설정 클래스,어노테이션 등 다양한 방법을 통해 빈을 설정할 수 있습니다.이를 통해 애플리케이션 컨텍스트는 빈을 검색하고,필요한 빈을 주입 받을 수 있는 기능도 제공합니다.
<br/>
애플리케이션 컨텍스트는 서술한 바와 같이 애플리케이션의 모든 빈과 리소스를 관리하는컨테이너 역할을 하는 스프링의 핵심 요소입니다.빈의 생성,의존관계 주입,이벤트 전달,다국어처리 등 다양한 기능을 통해 애플리케이션의 유연성과 유지보수성을 높이는 역할을 합니다.
스프링 애플리케이션의 중심에서 모든 구성 요소를 연결하고 관리하는 중요한 역할을 합니다.

### root-context & servlet-context
- root-context : 전역으로 사용되는 빈을 정의
- servlet-context : 특정 서블릿과 관련된 빈을 정의

model과 service는 root-context, controller는 servelt-context에서 component-scan을 처리하는 것이 좋다. model과 service는 요청 뿐만 아니라 테스트 코드에서도 사용할 수 있기때문에 일반적인 자원으로 취급하는 것이 좋고, controller는 요청 처리 외에는 잘 사용하지 않기 때문이다.

## Spring Run

Spring에서 Run하기 위해서는 Project를 통해서 Run을 진행해야한다. jsp등에서 Run을 진행하면 web.xml을 통하지 않아 DispatcherServlet에 접근하지 못한다.

## MVC 관련 클래스

DispatcherServlet : web.xml에 선언되어있음, 기본값으로 spring/appServlet/servlet-context.xml을 설정파일로 갖는다. 여기서 component-scan이 이루어짐
<br/>
ViewResolver : servlet-content.xml에 선언되어있음, 기본값으로 prefix는 /WEB-INF/views/, suffix는 .jsp로 되어있다.

### Controller&Service&Repository
- Repository : 동적 쿼리만 있어야함, DBMS에 종속적인 로직은 배제할 것
- Service : 모든 비즈니스 로직과 개발자, 관리자 용 예외 처리 및 모델 생성
- Controller : 요청을 분석하고, 모델을 뷰에 렌더링해 클라이언트에 응답, Restful일 경우 데이터와 HttpStatus를 ResponseEntity<T>에 담아서 브라우저로 바로 전송, 사용자용 예외 처리
- Model : 비즈니스 로직 처리 후 만들어진 오브젝트