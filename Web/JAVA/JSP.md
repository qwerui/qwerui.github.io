# JSP

## JSP

html에서 자바 코드를 사용할 수 있게 해주는 기술, 서버에서 실행되어 클라이언트에게 보내진다. 다만 JSP내에서 자바 코드를 다량으로 사용하는 것은 지양하는 것이 좋다. JSP는 Servlet에서 받은 데이터에 따라 표시되는 HTML을 다르게 해주는 역할에 불과하다.

```java
<% 스크립트릿 %> // 자바 코드를 작성
<%= 표현식 %>   // 표현식을 작성하여 결과를 출력
<%! 선언식 %>   // 메서드나 변수를 선언
<%@ 디렉티브 %> // 페이지의 전체 속성을 설정하는데 사용
<!-- 주석 -->
```

## JSP의 작동 원리

- JSP를 JAVA 파일로 변환을 하고, 그 파일을 컴파일한다. 이 결과를 클라이언트의 웹 브라우저에 전송하게 된다. 이 컴파일 과정은 보통 맨 처음 요청되었을 때 진행되며, 이후 요청에 대해서는 컴파일된 결과를 재사용한다.
- 모든 스크립트릿 블록은 하나의 메소드로 취급된다. 따라서 위에서 선언하지 않은 변수를 사용할 수 없다. 그에 반해 선언문은 클래스의 멤버 변수, 메소드로 취급되어 어느 위치에서 선언을 하더라도 스크립트릿에서 참조할 수 있다.

## 디렉티브 태그

<%@ %>로 사용

- page : JSP 페이지에 대한 정보를 설정
- include : JSP 페이지의 특정 영역에 다른 문서를 포함
- taglib : JSP 페이지에서 사용할 태그 라이브러리를 설정

## 액션 태그

서버나 클라이언트에게 어떤 행동을 하도록 명령하는 태그 <jsp:[액션태그] /> 형식으로 사용

- forward : 다른 페이지로의 이동과 같은 페이지 흐름 제어
- include : 외부 페이지 내용을 포함하거나 페이지 모듈화
- useBean : JSP페이지에 자바빈즈를 삽입
- setProperty/getProperty : 자바빈즈의 프로퍼티 설정/획득
- param : forward, include, plugin 태그에 인자를 추가
- plugin : 웹 브라우저에 자바 애플릿을 실행, 자바 플러그인에 대한 OBJECT 또는 EMBED 태그를 만드는 브라우저별 코드를 생성
- element : 동적 XML 요소를 설정
- attribute : 동적으로 정의된 XML 요소의 속성 설정
- body : 동적으로 정의된 XML 요소의 몸체를 설정
- text : JSP 페이지 및 문서에서 템플릿 텍스트를 작성

```jsx
// forward
<jsp:forward page="page2.jsp">
    <jsp:param name="username" value="john_doe" />
    <jsp:param name="email" value="john@example.com" />
</jsp:forward>

// include
// include 또한 param을 사용할 수 있다. flush를 true로 설정하면
// 일단 출력 버퍼를 비우고 클라이언트에 전송하는데 이 때 헤더 정보도 같이 전송된다.
// 헤더 정보가 웹 브라우저에 전송되고 나면 헤더 정보를 추가해도 반영되지 않는다.
<jsp:include page="파일명" flush="false"/>

// useBean
<jsp:useBean id="자바빈즈 ID" class="패키지 포함 클래스 이름" scope="저장 영역">
	<jsp:setProperty name="자바빈즈 id" property="프로퍼티 이름" value="값"/>
	<jsp:setProperty name="자바빈즈 id" property="프로퍼티 이름" param="request의 파라미터 이름"/>
	<jsp:getProperty name="자바빈즈 id" property="프로퍼티 이름"/>
</jsp:useBean>
```

### include 액션 태그와 include 디렉티브 태그의 차이

| 구분 | include 액션 태그 | include 디렉티브 태그 |
| --- | --- | --- |
| 처리 시간 | 요청 시 자원을 포함한다 | 번역 시 자원을 포함한다 |
| 기능 | 별도의 파일로 요청 처리 흐름을 이동한다 | 페이지 내의 변수를 선언한 후 변수에 값을 저장한다 |
| 데이터 전달 방법 | request 기본 내장 객체나 param 액션 태그를 이용하여 파라미터를 전달 | 페이지 내의 변수를 선언한 후 변수에 값을 저장한다. |
| 용도 | 화면 레이아웃의 일부분을 모듈화할 때 주로 사용 | 다수의 JSP 웹 페이지에서 공통으로 사용되는 코드나 저작권과 같은 문장을 포함하는 경우에 사용 |
| 기타 | 동적인 페이지에 사용, JSP 페이지 개수만큼 서블릿이 생성 | 정적인 페이지에 사용, JSP 페이지가 많아도 하나의 서블릿이 생성 |

### Scope

- page : include, forward, redirect 모두 페이지 범위 변수를 유지하지 않음
- request : include, forward는 요청 범위 변수 유지, redirect는 새로운 요청을 생성하므로 유지하지 않음
- session : include, forward, redirect 모두 세션 범위 변수를 유지, 동일한 세션 내에서 유효
- application : include, forward, redirect 모두 애플리케이션 범위 변수를 유지, 애플리케이션 전체에서 유효 → 세션이 끊겨도 서버가 유지되면 다시 접속 시 데이터가 유지된다.

## 내장 객체(Implicit Objects)

JSP에서 사용할 수 있도록 미리 컨테이너에 정의된 객체

**객체 종류**

out         : JspWriter 객체로, 클라이언트로 출력할 내용을 작성합니다   
request     : HttpServletRequest 객체로, 클라이언트의 요청 정보를 포함합니다   
response    : HttpServletResponse 객체로, 클라이언트로 보낼 응답 정보를 포함합니다   
<br/>
config      : ServletConfig 객체로, 서블릿의 초기화 파라미터 정보를 제공합니다   
session     : HttpSession 객체로, 클라이언트와 서버 간의 세션을 관리합니다   
application : ServletContext 객체로, 웹 애플리케이션 전체에 대한 정보를 제공합니다   
<br/>
pageContext : PageContext 객체로, 페이지 컨텍스트를 제공합니다   
page        : 현재 JSP 페이지의 객체를 가리킵니다   
exception   : 예외가 발생했을 때 예외 정보를 포함하는 객체입니다 (오류 페이지에서만 사용 가능)   

### EL 내장 객체와 영역

pageContext      : Page   
<br/>
param            : Request   
paramValues      : Request   
header           : Request   
headerValues     : Request   
cookie           : Request   
initParam        : Application   
<br/>
pageScope        : Page   
requestScope     : Request   
sessionScope     : Session   
applicationScope : Application   

## Filter

URL에 따라 자동으로 수행되는 클래스, 생성자, destroy, init, dofilter로 구성되어 있으며, FilterChain을 통해 다음 필터 혹은 웹 어플리케이션으로 request와 response가 넘어간다.
<br/>
web.xml에서 filter 태그로 Spring 필터를 사용할 수 있다.

## JSessionID

Tomcat에서 세션 사용자를 구분하기 위한 쿠키, 클라이언트가 처음 접속했을 때 WAS가 보내주는 값이다. 클라이언트가 요청을 보낼 때 헤더에 쿠키를 보내기 때문에 서버에서 이 JSessionID를 비교해 세션을 구분한다. 이 쿠키의 값을 지우면 새 연결로 간주하며, 강제로 쿠키의 값을 다른 세션의 쿠키 값으로 변경하면 다른 브라우저나 컴퓨터로도 해당 세션에 접근할 수 있다. 보안 위협이 되기 때문에 조심히 다뤄야한다.