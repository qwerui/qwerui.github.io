# State

## 상태 관리
서버 또는 클라이언트에 데이터를 저장해 상태를 관리한다.   

**클라이언트 측**   
- Hidden Field
- View State
- Cookies
- Control State
- Query Strings
<br/>

**서버 측**   
- Session
- Application

## Global.asax
Global.asax 파일을 전역 응용 프로그램 클래스다. Application과 Session 레벨 이벤트를 제공한다.

## 캐싱
자주 바뀌지 않는 페이지를 서버에서 저장하고 있다가 요청이 들어오면 저장된 페이지를 바로 출력해주는 성능 향상 기법
```
//태그 방식
<%@ OutputCache Duration="sec" VaryByParam="None" %>
```
```C#
//코드 방식
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetExpires(DateTime.Now.AddSeconds(60));
Response.Cache.VaryByParams["*"] = true;
```

## Web.config
웹 프로젝트 관련 환경설정 파일, 크로스플랫폼 기반의 ASP.NET Core에서는 appsettings.json이 이 파일의 기능을 대부분 대체한다. 대표적인 태그로 appSettings, connectionStrings가 있으며, add 태그로 데이터를 추가할 수 있다. 코드에서는 WebConfigurationManager, ConfigurationManager를 통해 접근할 수 있다.

### coneectionStrings의 형식
"server=DB서버;database=데이터베이스;uid=사용자;pwd=비밀번호"