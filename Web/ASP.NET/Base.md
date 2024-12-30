# Base

## ASP에서 사용하는 특수 폴더
- App_Code : 웹 사이트에서 사용할 클래스 파일이 저장, 웹 사이트 형식으로 생성된 프로젝트에서 사용
- App_GlobalResources : 웹 사이트 전체에서 사용되는 리소스 파일 저장
- App_LocalResources : 웹 페이지 각각에서 사용되는 리소스 파일 저장
- App_Data : 데이터베이스 및 XML 같은 데이터 파일 저장
- App_Browsers : 브라우저와 관련된 정보 저장
- App_Theme : 서버 컨트롤에 대한 스타일 정의

## 웹 폼 사용자 정의 컨트롤
웹 페이지에서 반복 사용되는 부분을 따로 파일로 구성, .ascx파일 사용.

```
//ascx파일을 웹 폼에 드래그 하면 자동으로 기재된다.
//정의
<%@ Register Src="~/Control.ascx" TagPrefix="uc1" TagName="Control" %>
//사용
<uc1:NavigatorUserControl runat="server" ID="Control"/>
```

## 마스터 페이지
웹 페이지 전체에 공통적으로 사용하는 뼈대를 구성할 때 사용하는 기능, MVC에서는 레이아웃으로 구현, 마스터 페이지는 중첩할 수 있다. 웹 폼에서 Page 지시문의 MasterPageFile 속성으로 적용할 마스터 페이지를 지정할 수 있고, <asp:ContentPlaceHolder> 태그의 ID를 기준으로 마스터 페이지에 삽입될 내용이 결정된다.

## 테마
컨트롤에 사용할 디자인을 미리 정의, .skin파일에 정의된다.   

**테마 적용 순서**
1. App_Theme 폴더 하위에 테마의 이름이 될 폴더를 생성한다.
2. 내부에 skin파일이나 css파일을 생성해 작성한다. skinID를 지정하지 않으면 전체에 적용되며, skinID로 적용할 디자인을 지정할 수 있다.
3. 웹 폼의 Page 지시문에 Theme 속성을 지정한다.
4. 웹 사이트 전체에 적용하기 위해서는 Weg.config파일의 system.web에 <pages theme="테마명"></pages>를 기재한다.