# Others

## 패키지 관리
PHP의 패키지 관리는 컴포저를 사용하면 가능하다. 내장되어있지 않아 별도로 설치해야한다. 패키지를 찾는 대표적인 사이트는 packagist
<br/>

- php composer.phar require 설치 패키지 명 : 패키지 설치, composer.json을 갱신한다.
- require 'vendor/autoload.php' : composer를 통해 설치한 패키지 import

### 메일
- 사전 준비 : 컴포저로 swiftmailer 설치
```php
$message = Swift_Message::newInstance();
$message->setFrom('from');
$message->setTo(array('to'=>'name'));
$message->setSubject('제목');
$message->setBody(<<<_TEXT_
본문 내용
_TEXT_
);
$message->addPart(<<<_HTML_ 
HTML내용 
_HTML_, "text/html");

//SMTP로 전송
$transport = Swift_SmtpTransport::newInstance('호스트', 25);//두번째는 포트
$mailer = Swift_Mailer::newInstance($transport);
$mailer->send($message); //전송

```

### 라라벨
- 사전 준비 : 컴포저로 라라벨 설치 후 laravel new 프로젝트명 실행
```php
// 라우팅 프레임워크
Route::get('/url', function(){
    return view('php파일이름', [전달 파라미터]);
});
```

### 심포니
- 사전 준비 : 컴포저로 심포니 설치 후 symfony new 프로젝트명 실행
```php
// 심포니는 src/AppBundle/Controllers에서 라우팅을 처리한다.
class SomeController extends Controller
{
    // 주석으로 라우팅 주소와 메소드 지정
    /**
    * @Route("/some")
    * @Method("GET")
    */
    public function someAction()
    {
        return $this->render('뷰 템플릿 파일 이름', [전달 파라미터]);
    }
}
```

### Laminas(Zend framework)
- 사전 준비 : 젠드 프레임워크 설치 후 프로젝트 생성
```php
//Controller 폴더에 라우팅 컨트롤러 파일 추가
class SomeController extends AbstractActionController
{
    public function someAction()
    {
        return new ViewModel(array(전달 파라미터));
    }
}

// module.config.php
'controllers' => array(
    'invokables' => array(
        'URL' => 'Controller Path'
    )
)
```

## PHP REPL
php -a 명령을 실행하면 입력한 PHP코드의 결과를 바로 보여주는 환경으로 이동할 수 있다.

## 지역화
```php
$en = new Collator('en_US');
$en->sort($words); // 영문 뿐만 아니라 다양한 언어를 정렬할 수 있다.

$msg = "{0}";
$fmt = new MessageFormatter('en_US', $msg);
print $fmt->format(array(123)); // 결과는 123이 출력된다.
```