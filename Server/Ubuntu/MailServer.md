# Mail Server

## 프로토콜
- SMTP : 클라이언트에서 송신, 서버 송/수신에 사용
- POP3 : 서버에서 클라이언트로 수신할 때 사용
- IMAP : POP3와 동일 역할

## 센드메일 서버 구축
**네임서버**   
1. [네임서버 도메인이름 설정파일 생성 까지 수행](/Server/Ubuntu/NameServer.md)
2. 아래의 코드를 삽입 (이름이 mail.mid.com일경우)
```
    IN  MX  10  mail.mid.com
mail    IN  A   192.168.111.100
```

**센드메일 서버**   
1. apt -y install sendmail
2. /etc/hostname 수정으로 호스트 이름 수정
3. /etc/host에 IP 호스트 이름 추가
4. /etc/mail/local-host-names에 호스트 이름 추가
5. /etc/NetworkManager/system-connections/[연결 네트워크 이름] 파일에서 dns를 네임서버의 IP로 변경한다.
6. systemctl restart NetworkManager
7. apt -y install dovecot-poip3d
8. /etc/mail/sendmail.cf 에서 수정
```
98 : Cw[도메인 이름]
269 : Addr=127.0.0.1 삭제
270 : Addr=127.0.0.1 삭제
```
9. /etc/mail/access 수정
```
[IP 또는 도메인] RELAY
```
10. makemap hash /etc/mail/access > /etc/mail/access
11. /etc/dovecot/dovecot.conf 수정
```
30 : 주석제거
33 : 주석제거
34 : disable_plaintext_auth = no 추가
```
12. /etc/dovecot/conf.d/10-mail.conf 수정
```
121 : mail_access_groups = mail
177 : 주석제거
```
13. systemctl restart sendmail/dovecot

## 웹 메일 서버 구축
1. apt -y install dovecot-imapd lamp-server^
2. apt -y install roundcube
3. /etc/apache2/conf-enabled/roundcube.conf 수정
```
3 : Alias /webmail /var/lib/roundcube
5 : AddType application/x-httpd-php .php 추가
```
4. /etc/roundcube/config/inc/php 수정
```
36 : localhost
51 : 25
55 : ''내부 제거
```
5. systemctl restart apache2/mysql
6. 네임서버의 도메인 이름 설정 파일에 www IN A [웹 서버 IP] 추가

### 웹 메일 용량 변경
1. /etc/phjp/7.4/apache2/php.ini 수정
```
388 : 최대 실행시간 설정(초 단위)
694 : 최대 용량 설정
845 : 최대 파일 용량 설정
```
2. /etc/roundcube/config/inc/php 수정
```
$config['max_message_size']='[파일크기]'; 추가
```

