# Form

## 폼 매개변수 접근
$_에 HTTP 메소드에 해당하는 배열을 사용한다. $_GET['name']이면 GET 메소드의 name 파라미터를 가져온다.

### 폼 매개변수 검증
- filter_input(입력 형태, 이름, 필터, 옵션) : filter_input(INPUT_POST, 'age', FILTER_VALIDATE_INT, array('options'=>array('min_range'=>10)));
- is_null : 널 체크
- === : 항등 연산자 값과 타입이 모두 같으면 true를 반환한다.
- strip_tags : 문자열에서 HTML 태그 제거
- htmlentities : HTML의 태그에 사용되는 특수문자를 &lt, &gt, &amp, &quot 등으로 인코딩
- var_dump : 폼 매개변수를 출력

## $_SERVER
- QUERY_STRING : 쿼리 문자열
- PATH_INFO : URL의 추가 경로 정보
- SERVER_NAME : 웹 사이트의 이름
- DOCUMENT_ROOT : 웹 서버 컴퓨터의 최상위 디렉토리
- REMOTE_ADDR : 사용자 IP
- REMOTE_HOST : 사용자 IP를 호스트명으로 전환(거의 사용하지 않음)
- HTTP_REFERER : 사용자가 링크 클릭 시 이전 링크
- HTTP_USER_AGENT : 브라우저 정보
- REQUEST_METHOD : HTTP 메소드
- argv : 명령 행 인수 배열