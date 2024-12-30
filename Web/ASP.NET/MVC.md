# MVC

## 개요
ASP에서의 MVC 구조 템플릿, URL이 IP:포트/컨트롤러/액션 형식으로 구성된다. WebForm과는 달리 시작 페이지 개념이 존재하지 않는다. 컨트롤러 클래스의 public 인스턴스 메소드는 액션 메소드가 된다. 반환타입은 제한이 없으나 일반적으로 IActionResult의 파생 클래스가 된다. 액션 메소드는 HttpGet, HttpPost 등 어트리뷰트로 반응하는 HTTP 메소드를 지정할 수 있다.

## ASP에서의 MVC
- Model : C# 클래스, 주로 Entity Framework와 함께 사용
- View : UI, 렌더링 담당, cshtml파일 사용(html+razer=>@로 서버 측 출력 포함)
- Controller : 요청 처리, 모델 조작, 출력 UI 결정

### 액션 메소드의 반환값
- View() - 액션 메소드의 이름을 가진 뷰 출력
- RedirectToAction(URL) : 특정 액션 메소드 실행
- Content("문자열") : 특정 문자열 반환
- ContentResult() : HTML 직접 반환

## 스캐폴딩
하나 이상의 클래스를 기반으로 CRUD와 뷰 페이지를 생성해주는 기능, 이름을 기준으로 자동으로 외래키를 지정해준다. virtual 키워드의 필드가 모델 간의 관계를 나타내며 타입이 ICollection 계열이냐 아니냐에 따라 1의 관계, 다의 관계가 결정된다. 

1. C# 클래스로 된 모델을 생성한다.
2. 프로젝트를 빌드한다.
3. 컨트롤러를 생성할 때 Entity Framework를 사용하며 뷰가 포함된 MVC 5 컨트롤러를 선택한다.
4. 모델 클래스와 데이터 컨텍스트 클래스를 지정한 뒤 생성한다.
5. Controllers 폴더에 컨트롤러, Views 하위의 모델명 폴더에 CRUD와 Index View가 생성된다.
6. 다른 모델도 생성하려면 2번부터 수행한다.

## Model
### 어트리뷰트
ModelState.IsValid 속성으로 모델이 유효성을 통과하는지 검사할 수 있다.
- Datatype : 유효성 검사
- Display : 모델을 표현할 때 제약 조건 또는 표시 형식을 지정
- Required : 필수 입력
- StringLength : 문자열 길이지정 (MinLength, MaxLength도 존재)
- RegularExpression : 정규표현식 사용
- Range : 범위 지정
- EmailAddress : 이메일 형태

## View
### Razor
ASP의 뷰 엔진 @로 서버 측 C# 코드를 출력한다. @* *@로 주석을 작성할 수 있다. 루트 경로는 ~/를 사용

### environment 태그 헬퍼
css나 js파일 링크를 개발, 스테이징, 프로덕션 환경으로 나눠 지정할 수 있다. 속성으로 names를 사용하며 Development, Staging, Production을 값으로 사용한다. ,로 여러 환경을 지정할 수 있다.

### Partial View
웹 폼 사용자 정의 컨트롤과 비슷하게 하나 이상의 페이지에서 호출되는 뷰, 일반적으로 _가 접두어로 붙는다. @await Html.PartialAsync("_PartialView")나 @Html.Partial("_PartialView")로 뷰 페이지를 포함시킬 수 있다.

**기본 생성 Partial View**   
- _ViewStart : 모든 뷰에 지정되는 레이아웃, 뷰가 렌더링 되기 전에 제일 먼저 호출된다.
- _ViewImports : 모든 뷰에서 특정 네임스페이스를 사용하기 위한 파일, @using 구문으로 사용할 네임스페이스를 지정해야한다.

### 데이터 전달
- View Bag : ViewBag은 컨트롤러에서 View에 데이터를 넘길 때 사용하며, ViewData도 같은 기능을 가진다. ViewBag.식별자, ViewData["식별자"]로 사용할 수 있다.
- View(Model) : Controller에서 View를 반환할 때 매개변수로 모델을 보낼 수 있다. 컨트롤러에서 넘어온 모델은 @model로 받을 수 있다. 컬렉션 또한 가능하며 컬렉션일 경우 IEnumerable로 받아야한다.

### 헬퍼 메소드
뷰에서 사용하는 요소들의 생성을 돕는 메소드, URL이나 HTML이 있다.
**종류**   
- Form
- Input
- Label
- Link
- Select
- TextArea
- Validation

### 태그 헬퍼
태그에 asp-로 시작하는 속성을 추가하거나 별도의 태그를 만들어 HTML 태그에 추가 기능을 구현한다. 내장 태그 헬퍼는 @tagHelperPrefix로 접두사를 지정할 수 있다. 사용자 태그 헬퍼는 @addTagHelper로 뷰 페이지에 추가해야한다.   

**내장된 헬퍼 종류**   
- controller, action : 링크나 폼의 컨트롤러와 액션 메소드 지정
- for : 모델의 데이터를 가져오거나 items와 결합해 items의 요소를 가져옴
- items : select와 같이 사용, 전체 데이터 배열
- validation-for : 유효성 검사

**<cache> 태그 헬퍼**   
- expires-after : 특정 시간 동안 캐시 적용
- expires-on : 특정 시간에 캐시 해제
- expires-sliding : 지정 시간 후에 출력되지 않는 내용 설정
- vary-by-user : 로그인한 사용자별로 캐싱
- vary-by-route : 특정 라웉트에 따른 캐싱
- vary-by-query : 쿼리에 따른 캐싱
- vary-by-cookie : 쿠키에 따른 캐싱
- vary-by-header : 헤더에 따른 캐싱
- vary-by : 문자열에 따른 캐싱
- priority : 캐싱 우선순위