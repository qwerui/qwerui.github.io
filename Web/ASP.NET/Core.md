# .Net Core

## 개요
Web Pages + Web API + MVC, WebApplicationBuilder에서 웹 사이트를 설정하고 WebApplication에서 미들웨어를 실행한다.

## wwwroot
웹 사이트의 루트 폴더, 이 폴더의 정적인 파일(css, js)등이 실행되려면 Program.cs에서 app.UseStaticFiles()를 사용해야한다.

## 미들웨어
Program.cs에서 미들웨어를 추가할 수 있다.
- UseStaticFiles : wwwroot의 정적 파일들을 실행할 수 있다.
- UseDirectoryBrowser : 디렉토리 목록 보기
- UseStatusCodePages : 상태 코드 표시
- UseWelcomePage : 환영 페이지 출력
- UseDeveloperExceptionPage : 개발자용 에러 메시지

## 라우팅
Program.cs에 라우팅에 대한 정보가 기본으로 들어있다. 기본적으로는 controller나 action에 해당하는 값은 컨트롤러 클래스 이름과 인스턴스 메소드명이지만, [Route("")] 어트리뷰트를 통해 다른 URL을 사용하도록 지정할 수 있다.
```c#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

## DI
Program.cs의 builder의 Services에서 의존성을 주입할 객체를 지정할 수 있다. 주입을 받을 클래스의 생성자에 주입할 객체의 인터페이스를 인자로 받으면 된다.
- AddSingleton<Interface, Implementation>() : 싱글톤이 주입
- AddTransient<Interface, Implementation>() : 새로운 객체를 생성해 주입
- AddScpoed<Interface, Implementation>() : 현재 스코프에서는 싱글톤, 즉 같은 요청에 대해서는 같은 인스턴스를 사용한다.

<br/>
서비스에 등록된 인스턴스는 뷰에서도 의존성을 주입하는 것이 가능하다. @Inject 클래스타입 식별자를 통해 뷰에서 주입된 객체에 접근할 수 있다.

## JSON->C#
```c#
builder.Configuration.AddJsonFile("json파일 경로", optional: true);
builder.Services.Configure<SomeClass>(builder.Configuration.GetSection("json파일 이름"));

//이후 클래스의 생성자에서 IOptions<SomeClass>로 클래스를 얻어올 수 있다.
```

## 로깅
생성자 주입으로 ILogger<컨트롤러 클래스>를 받아 사용하면 로깅을 할 수 있다.