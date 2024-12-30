# Web API

## 개요
브라우저 및 모바일 등 다양한 클라이언트에 연결할 수 있는 RESTful HTTP 서비스, JQuery나 앵귤러 등을 사용한 SPA를 구현하는데 필요한 항목 중 하나다.

## 사용
Web API로 파일을 생성하면 인스턴스 메소드로 Get, Post 등 HTTP 메소드로 메소드명을 사용할 수 있다. URL 경로는 /api/클래스 이름으로 API에 접근할 수 있다.

## .NET Core API
```c#
namespace ApiHelloWorld.Controllers
{
    [Route("api/[Controller]")]
    public class ValuesController : Controller
    {
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new[] { "value1", "value2" };
        }

        //:은 제약조건이다.
        [HttpGet("{id:int}")]
        public string Get(int id)
        {
            return "value";
        }
    }
}
```

### CORS 설정
```c#
// 모든 Web API가 다른 웹사이트의 데이터를 가져다 사용
app.UseCors(options.AllowAnyOrigin().WithMethods("GET"));
```

## Settings
- launchsettings.json : VS에서 실행할 때 환경을 정의
- appsettings.json : 앱의 실행환경을 정의, 환경변수로 덮어 씌울 수 있으며 json 계층은 __(더블 언더스코어)로 구분