# Authentication

## 개요
템플릿, 쿠키, 소셜 인증을 기본으로 제공

## 쿠키 인증
```c#
// 인증 구성
builder.Services.AddAuthentication("Cookies")
    .AddCookie(options =>
    {
        options.LoginPath = "/Account/Login";
        options.LogoutPath = "/Account/Logout";
        options.AccessDeniedPath = "/Account/AccessDenied";
    });

// 인증
var claims = new List<Claim>
{
    new Claim("Key", "Value");
}

var ci = new ClaimsIdentity(claims, "인증 스키마");

await HttpContext.Authentication.SignInAsync("Cookies", new ClaimsPrincipal(ci));

// 인증 확인
User.Identity.IsAuthenticated == true;

```

Authorize 어트리뷰터를 액션 메소드에 붙이면 인증된 사용자만 접근 가능하고, AllowAnonymous를 붙이면 아무 사용자나 접근이 가능하다.

## 역할 기반 인증
```c#
builder.Services.AddAuthentication(options=>{
    options.AddPolicy("권한명", policy=>policy.RequireRole("역할명"));
});

```