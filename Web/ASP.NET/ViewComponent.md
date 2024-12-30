# View Component

## 개요
UI와 데이터를 묶어서 뷰 페이지에 출력해주는 컴포넌트, 서버 컨트롤이나 부분 뷰와 비슷하다. 원래 View에서 호출하지만 ViewComponent() 메소드로 컨트롤러에서도 호출할 수 있다.

## 사용
**뷰 컴포넌트 경로**   
1. Views/컨트롤러/Components/뷰 컴포넌트 이름/뷰 이름
2. View/Shared/Components/뷰 컴포넌트 이름/뷰 이름
```c#
namespace AspNetCoreWebApplication.Views.Home.Components
{
    public class DefaultViewComponent : ViewComponent
    {
        public IViewComponentResult Invoke()
        {
            return View();
        }
    }
}
```
```html
@await Component.InvokeAsync("뷰 컴포넌트")
```