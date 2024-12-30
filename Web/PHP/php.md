# PHP

## 기본
```php
<?php
    // 태그내에서는 php코드, 그 외에는 HTML
    print 4;
    // 대소문자 구별 X
    PRINT 5;
    /*여러 줄 주석*/
    // 한 줄 주석(권장)
    # 한 줄 주석

    $variable = true; // 변수
    $a = 1<=>3 // strcmp와 비슷한 연산자, 모든 자료형에 사용 가능
?>
```

### 조건식&반복문&try-catch
C언어와 동일하게 사용하면 된다.

### 사용자 예외 처리
set_exception_handler에 함수명을 문자열로 전달해 해당 함수에서 예외를 처리하도록 한다.

## 문자열
PHP의 문자열 처리는 C언어와 비슷하게 처리하면 된다.
```php
print "abc\nedf"; // 이스케이프 문자로 개행
print 'abc
edf'; // 작은 따옴표 내에서는 개행 가능

print <<< HTMLBLOCK
<h3> 다량의 HTML 블록 </h3>
HTMLBLOCK;

print '문자열'.'연결'; //.도 연산자이기 때문에 .=같은 축약형도 있다.
print "변수의 값은 {$variable}이다.";

// 문자열 끼리 비교 연산이 가능하며, 사전 순으로 비교한다.
```

### 함수 목록
- trim : 양 끝 공백 제거
- strlen : 문자열 길이
- strcmp : 문자열 비교
- strcasecmp : 두 문자열을 대소문자 구분 없이 비교
- printf : 텍스트 포맷팅
- wcwords : 단어의 첫 글자만 대문자 나머지 소문자
- substr : 문자열 추출, 문자열, 위치, 바이트 순으로 매개변수가 들어간다.
- str_replace : 문자열 치환
- mb_strlen : 멀티바이트 문자열의 길이(많은 함수가 멀티바이트를 대상으로 사용할 때에는 mb_접두어가 붙는다.)

## 배열
```php
$array1 = array(0=>1, 1=>2, 2=>3); //배열은 key=>value의 형태
$array2 = ['1'=>1, '2'=>2, '3'=>3]; //단축 문법
$array2['1'] = 10; //배열 요소 사용

foreach($array1 as $key => $value){
    // 배열 순회 $value를 변경해도 원본 배열에는 영향이 없다.
    // 숫자키라면 $key =>를 생략할 수 있다
    // foreach는 배열에 원소가 추가된 순서를 따른다.
}

```
### 함수 목록
- count : 배열 크기
- array_key_exists : 키 존재 여부
- in_array : 값 존재 여부
- array_search : 값으로 키 찾기
- unset : 원소 제거
- implode : 배열의 모든 값을 구분 문자를 넣어 하나의 문자열로 생성
- explode : 문자열을 구분자로 나눠 배열 생성
- sort : 값 기준 정렬, 키를 새로 할당해 숫자키 배열에만 사용 가능
- asort : 값을 기준으로 정렬, 키가 유지됨
- ksort : 키를 기준으로 정렬
- rsort/arsort/krsort : 역순 정렬

## 함수
```php
// 매개변수나 함수의 오른쪽에 타입을 붙여 제약조건을 추가할 수 있다.
function func(int $argu = 123) : int
{
    $GLOBALS['outer'] = 123; // 전역 변수(권장)
    global $outer; // 지역 변수 => 전역 변수
    $variable = 123; // 지역 변수
    //매개변수에 기본값 지정가능, 기본값을 지정하면 그 매개변수는 선택사항이 된다.
    return $argu * 2;
}

require 'some.php'; // 다른 파일 읽기, 현재 파일은 이 파일에 있는 함수를 실행할 수 있다.
```

## 클래스
```php
namespace name;

// 클래스 선언(상속)
class _Class extends _Parent{
    public $name;

    public function __construct(){
        //생성자
                _Parent::__construct(); //상속 받은 메소드 사용

    }

    public function func($argu){
        //->로 내부 요소 접근
        //$this는 현재 객체
        if(is_array($argu)){
            throw new Exception('예외 발생');
        }
        return $argu.$this->name;
    }

    public static function staticFunc(){
        //정적 메소드
        print 123;
    }
}

//클래스 생성
$Instance = new _Class;

//정적 메소드 호출
_Class::staticFunc();

//네임 스페이스 사용
\name\_Class::staticFunc();
use name as myname; //using namespace std 같은 것
```

## 파일 처리
- file_get_contents : 파일을 문자열로 읽기
- file_put_contents : 문자열을 파일에 쓰기
- file : 파일의 일부를 다룰 때 사용 foreach를 통해 1줄 씩 가져올 수 있다.
- fopen : 모드를 사용해서 파일 열기
- feof : 파일이 eof면 true
- fgets : 파일 1줄 읽기
- fclose : 파일 닫기
- fgetcsv : csv파일을 열었을 때 1줄을 읽어서 배열로 반환
- fputcsv : 배열을 csv파일에 쓰기
- file_exists : 파일 존재 여부
- is_readable : 읽기 가능 확인
- is_writable : 쓰기 가능 확인

### 웹 통신
file_get_contents 등으로 다른 웹사이트와 통신을 할 수 있다.
- http_build_query : 배열의 key-value를 쿼리스트링으로 변환
- json_decode : json형식 문자열을 배열로 변환
- json_encode : 배열을 json 문자열로 변환
- stream_context_create('http'=>array()) : 헤더 추가, 배열을 받는다. content에 쿼리스트링을 넣으면 POST 요청으로 전송 가능
- http_response_code : 응답 코드 변경

**cURL**   
- curl_init : url을 curl로 변환
- curl_setopt : 요청/응답 설정
- curl_exec : 실행
- curl_getinfo : 접속 정보 가져오기
- curl_errno : 에러 번호
- curl_version : curl 버전 확인

## 테스팅
- 사전 작업 : PHP 유닛 설치

1. 클래스에 PHPUnit_Framework_TestCast를 상속한다.
2. phpunit.phar 테스트php파일 실행

## 날짜 처리
new Datetime()으로 현재 시각을 얻을 수 있다. 메소드로 format을 가지고 있으며 형식 문자에 따라 출력가능하다. 설정 파일에서 date.timezone을 설정하거나 date_defualt_timezone_set함수를 사용해 시간대를 설정할 수 있다.   
!["형식 문자"](https://www.php.net/manual/en/datetime.format.php)   
- setDate : datetime의 시간 지정
- checkDate : 날짜의 실제 존재 여부
- modify : 현재 날짜에서 변경
- diff : 두 날짜 차이 계산