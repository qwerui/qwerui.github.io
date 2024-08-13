# Webform

## Webform
ASP의 웹페이지는 비주얼을 담당하는 .aspx와 코드를 담당하는 .aspx.cs로 구성되어있다. 처음 생성했을 경우 기본적으로 runat속성을 갖는 form 태그가 있으며, 보통 이 form 태그 안에 태그를 작성한다. ASP에서 제공하는 요소들의 태그는 보통 asp:로 시작한다. Toolbox에서 목록을 확인할 수 있다.

<br/>
태그 중에 서버에서 작업을 처리하는 태그들을 서버 컨트롤이라고 하며 runat='server' 속성이 있어야한다.

### System.Web.UI.Page
.aspx.cs는 기본적으로 System.Web.UI.Page를 상속받는다. 이 클래스가 웹사이트 제작에 필요한 대부분의 기능을 가지고 있다.   

**주요 멤버**   
- IsPostBack : 현재 페이지를 처음 로드했는지, 다시 게시 했는지 확인
- ClientScript.RegistarClientScriptBlock() : 자바스크립트를 동적으로 추가
- Header : head 태그 정의
- Title : 제목 설정
- SetFocus() : 웹 폼이 로드될 때 포커스를 받는 개체 지정

### Page 지시문
웹 폼은 <%@ Page %>로 되어있는 Page 지시문으로 시작한다.   

**주요 속성**   
- Language : 기본 언어 설정
- AutoEventWireup : 이벤트를 코드 파일과 자동으로 연결
- CodeFile : 현재 페이지를 담당하는 코드 파일 지정
- Inherits : 코드 숨김 파일의 클래스 지정
- Trace : 웹 폼을 추적하는 코드를 페이지 아래에 출력
- Debug : 웹 폼 실행시 발생되는 에러 메시지를 자세히 출력
- ValidateRequest : 웹 폼에서 입력된 HTML를 서버 측에 전송
- MaintainScrollPositionOnPostback : 상하 스크롤 바가 존재하는 페이지에서 새로고침하거나 버튼이 클릭될 때 이전의 스크롤바 위치로 스크롤 고정
- IsValid : 페이지가 유효성 검사를 통과하면 true 반환