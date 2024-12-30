# Implicit Object

## Implicit Object
ASP에는 JSP와 비슷하게 여러 내장 객체를 제공한다.   
**내장 객체 목록**   
- Response
- Request
- Server
- Application
- Session

### Response
클라이언트로 응답을 보내는 객체   

**주요 멤버**   
- Write() : 페이지에 문자열을 출력, HTML을 포함해 JS 실행 가능
- Redirect() : 리다이렉팅 기능
- Expires : 현재 페이지의 소멸 시간 지정
- Buffer : 버퍼링 사용 여부
- Flush() : 현재 버퍼의 내용 출력
- Clear() : 현재 버퍼의 내용 삭제
- End() : 현재 페이지 종료, End() 이후의 코드는 실행되지 않음
- WriteFile() : 스트림(파일) 출력
- Cookies[] : 쿠키 저장

### Request
클라이언트의 요청을 다루는 객체   

**주요 멤버**
- QueryString[] : Get방식으로 넘어온 값을 key:value로 가짐
- Form[] : Post 방식으로 넘어온 값을 key:value로 가짐
- Params[] : Get/Post 방식을 모두 값으로 가짐, Request[]와 같은 역할
- UserHostAdderss : 현재 접속자의 IP 주소
- ServerVariables[] : 현재 접속자의 주요 서버 환경 변숫값
- Cookies[] : 저장된 쿠키 값
- Url : 현재 웹페이지의 URL
- PhysicalApplicationPath : 현재 웹 사이트의 가상 디렉터리의 물리적인 경로

### Server
서버에 있는 특정 페이지를 현재 페이지에 포함하거나 경로 등의 기능을 다루는 객체   

**주요 멤버**   
- MapPath(".") : 현재 파일과 같은 경로 값 반환
- Execute() : include, 제어권 돌아옴
- Transfer() : include, 제어권 넘김
- UrlPathEncode() : 넘어온 쿼리스트링을 유니코드로 변환
- ScriptTimeout : 서버에서 현재 페이지를 몇 초간 처리할 지 결정

### Application
프로그램 영역에서 값을 다루는 객체, Locking이 필요할 수 있다.  

**주요 멤버**   
- Lock() : 어플리케이션 변수를 잠금
- UnLock() : 어플리케이션 변수를 잠금 해제
- Add() : 어플리케이션 변수 생성
- Application_Start() : 웹 어플리케이션 시작할 때(첫 사용자 접속) 발생
- Application_End() : 웹 어플리케이션 종료할 때(마지막 사용자 접속 종료) 발생

### Session
세션 유지 동안 저장되는 값을 다루는 객체   

**주요 멤버**   
- SessionID : 세션 고유 번호
- SessionTimeout : 세션 유지 시간
- Abandon() : 세션 삭제
- Session_Start() : 세션 생성 시 실행
- Session_End() : 세션 종료 시 실행