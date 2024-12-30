# Database

## PDO
PHP의 내장 기능, 일반적인 DB 통신을 위한 것이다.
```php
$db = new PDO('mysql:host=localhost;dbname=restaurant', '사용자명', '비밀번호');
$db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION); //에러 발생 시 throw
$rows = $db->exec("SQL 쿼리"); //DML, DDL 등
$q = $db->query("SELECT문");
while($row = $q->fetch()){
    // 결과 순회
}
$rows = $q->fetchAll(); //전체 반환

//Prepared Statement
$stmt = $db->prepare('SQL 쿼리');
$stmt->execute(array(?에 순서대로 들어갈 값));

// 반환 형식 변경
$q->fetch(PDO::FETCH_NUM); // 숫자 키 배열 반환, fetch에 적용
$q->setFetchMode(PDO::FETCH_ASSOC); // 문자 키 배열 반환, 쿼리에 적용
$db->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_OBJ); // 객체 반환, DB에 적용

// 따옴표, 와일드 카드 필터링(순서 중요)
$input = $db->quote($_POST['attack']); // 단일 따옴표를 이중으로 변경
$input = strtr($input, array('_'=>'\_', '%'=>'\%')); // 와일드카드 문자에 역슬래시 추가
```

### DSN
- MySQL : mysql: / host, port, dbname, unix_socket, charset
- PostgreSQL : pgsql: / host, port, dbname, user, password, others
- Oracle : oci: / dbname(hostname:port/database 형식), charset
- SQLite : sqlite: / 콜론 뒤에 경로를 넣는다
- ODBC : odbc: / DSN, UID, PWD
- MSSQL : mssql / host, dbname, charset, appname