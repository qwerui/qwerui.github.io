# Annotation

## 목록
### 빈
- @Component : 범용적인 빈 지정, 아래 하위 어노테이션의 부모
- @Controller : 컨트롤러 빈 지정
- @Service : 서비스 빈 지정
- @Repository : 리포지토리 빈 지정
- @Configuration : 설정 빈 지정
- @Bean : 메소드 반환 값을 빈으로 지정, @Configuration과 함께 사용

### URL 매핑
- @RequestMapping(value=”url” method={RequestMethod…}) : 해당 url요청에 따라 실행되는 메소드, RequestMethod가 없으면 모든 요청에 반응한다. class에 붙이면 공통 url을 정의 가능
- @GetMapping(”url”) : Get 요청만 받는 메소드, HTTP 메소드 별로 같은 역할의 어노테이션이 존재하며, [HTTP 메소드 명]Mapping(”url”)로 사용할 수 있다.
- @RequestParam : form 태그로 넘어오는 값에 대한 어노테이션
- @PathVariable : RequestMapping 등의 value의 표현식의 값을 받아옴
- @RequestHeader : 헤더 매핑

### 주입
- @Autowired : 의존성 주입, 타입에 맞는 빈을 자동으로 주입해준다.
- @Qualifier(”빈 이름”) : Autowired와 같이 사용, 명시적으로 빈을 지정
- @RequiredArgsConstructor : 필드를 생성자 주입으로 주입한다. 필드에 final이 붙는 것이 권장사항 

### 반환
- @ResponseBody : 자바 객체를 json 기반의 Http body로 변환
- @RestController : Controller+ResponseBody

### 기타
- @Log : 클래스내에서 Log4J 기반의 로그 사용
- @ExceptionHandler : 해당 에러 발생 시 실행되는 메소드 지정