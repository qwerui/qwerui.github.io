# Web Server

## LAMP
Linux+Apache+Mysql+php   
apt -y install lamp-server^로 전체 설치 가능   
php 작동 확인 : <?php phpinfo(); ?>를 php파일에 입력하고 웹에서 접속   
설치 후 포트 개방

## 웹 설정 파일
/etc/apache2/apache2.conf (apache2ctl configtest로 테스트 가능)   
- ServerRoot "/etc/apache2" : 웹 서버 최상위 디렉토리
- Listen 80 : 웹 서버 포트번호 기본값 = 80
- Include conf.modules.d/*.conf : 설정 파일에 포함될 파일의 경로와 파일이름
- User ${APACHE_RUN_USER} : 웹 서비스를 작동하는 사용자
- Group ${APACHE_RUN_GROUP} : 웹 서비스를 작동하는 그룹
- MaxKeepAliveRequests : 처리 가능 최대 요청 수

### WordPress
1. 웹에서 사용할 사용자와 데이터베이스를 mysql에서 생성한다.
2. wordpress를 /var/www/html에서 압축을 해제한 뒤, 디렉토리의 소유자와 그룹을 www-data, 권한을 707로 변경한다.
3. wordpress내부의 wp-config-sample.php를 wp-config.php로 복사하고 에디터로 파일을 열어 DB관련 설정을 한다.
4. systemctl restart apache2
- (선택사항) /etc/apache2/site-enabled/000-default.conf의 DocumentRoot를 html/wordpress로 변경하고, /etc/apache2/apache2.conf에 아래의 코드를 추가한다. 이를 통해 URL을 입력하면 즉시 Wordpress가 보이게 된다.
```
<Directory /var/www/html/wordpress>
    //설정한 디렉토리의 하위 모든 디렉토리와 파일에 대한 접근권한 제어
    Options Indexes FollowSymLinks 
    //.htaccess파일 존재 시 설정 덮어쓰기 여부
    AllowOverride All
    //액세스 허가/거부
    Require all granted
</Directory>
```

### owncloud
1. LAMP 설치
2. owncloud에서 사용할 데이터베이스와 사용자 생성 (※ 사용자를 생성할 때 비밀번호 설정 시 with mysql_native_password by 비밀번호로 해야한다. 인증에서 발생하는 문제라고한다.)
3. owncloud를 다운받아 /var/www/html에서 압축해제 (※ owncloud가 현재 PHP버전을 지원하지 않을 수 있다. a2dismod 현재버전php, a2enmod 목표버전php로 php버전을 변경할 수 있다. 변경 후에는 apache2 재시작)