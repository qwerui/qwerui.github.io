# Cookie & Session

## 쿠키
```php
// 쿠키 설정, setrawcookie를 사용하면 PHP가 쿠키값을 인코딩하지 않는다.
// 3번째 인자는 만료시간, 6번째 인자는 https에만 반응할지 여부, 7번째 인자는 HttpOnly 쿠키 여부다.
// 쿠키를 삭제하려면 setcookie로 빈 문자열을 값으로 준다. 
setcookie('쿠키명', '값', time() + 3600, '경로', '도메인', true); 
print $_COOKIE['쿠키명']; // 쿠키 읽기
```

## 세션
```php
//기본적인 세션 지속시간은 24분
ini_set('session.gc_maxlifetime', 3000); // php.ini파일 변경 함수
session_start(); //세션 사용(보통 최상단에 위치), php.ini에서 session.auto_start를 1로 설정하면 없어도 세션을 사용한다.
if(isset($_SESSION['count'])) //세션 존재 여부
{
    $_SESSION['count'] += 1;
}
else
{
    $_SESSION['count'] = 1;
}
```

## 쿠키와 세션 위치
쿠키와 세션은 출력이 시작되기전에 모든 설정을 완료해야한다. setcookie나 session_start는 응답에 헤더를 추가하는데, 헤더는 모든 출력에 앞서 추가되어야만 올바르게 전송되기 때문이다.   
코드가 방대해 어느 위치에서 출력 후에 쿠키나 세션을 추가하는지 모르는 경우 설정에서 output_buffering을 on으로 지정하는 것도 방법이다. 출력 버퍼를 활성화해 요청 처리가 완전히 끝날 때 까지 전송을 대기하기 때문에 응답속도가 조금 느려지는 단점이 있지만 headers already sent 오류가 없어진다.