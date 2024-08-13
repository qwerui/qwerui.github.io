# Name Server

## 개요
네임서버는 DNS 서버라고도 한다. URL=>IP 변환 담당 서버   

## 설정 파일
**클라이언트**   
/etc/resolv.conf 파일이 클라이언트가 접속하는 네임서버가 설정된 파일
네임서버에 이상이 생기면 URL을 통한 접속이 불가능하지만, IP를 통한 접속은 가능하다.   
<br/>

**호스트**
/etc/hosts 파일에서 URL과 IP를 매핑

## 네임서버를 통한 접속 과정
1. URL 입력
2. etc/host.conf 조회 => URL 입력 시 우선순위를 정의, 기본은 order hosts(/etc/hosts 조회), bind(네임서버 질의)
3. etc/hosts 조회
4. etc/hosts에 존재 시 IP 획득, 없을 경우 /ect/resolv.conf에서 네임서버 IP 획득
5. 네임서버 IP 획득 성공 시 네임서버에 질의, 실패 시 IP 획득 실패

### 로컬 네임서버 작동 순서
1. URL 입력
2. 로컬 네임서버의 캐시 DB 검사
3. 캐시에 존재하지 않으면 루트 네임 서버에 질의
4. 하위 네임서버로 질의를 반복하면서 목표 네임서버에서 IP 획득 (보통 URL의 역순 ex: ROOT -> com -> naver)

## 캐싱 전용 네임서버
URL을 통해 IP를 얻고자 할 때, 해당 URL의 IP를 알려주는 네임서버

### 캐싱 전용 네임서버 구축
1. apt -y install bind9 bind9utils
2. /etc/bind/named.conf.options 수정
**예시**   
21 : dnssec-validation no;   
22 : recursion yes;   
23 : allow-query { any; };   
3. systemctl restart/enable named, ufw allow 53
<br/>
작동 확인 : dig @[네임서버 IP] [URL] 또는 nslookup으로 server [IP] => [URL]

## 마스터 네임서버
외부에서 URL로 컴퓨터의 IP를 알고자할 때 해당하는 컴퓨터의 IP를 알려주는 네임서버.

### 마스터 네임서버 구축
**사전작업**   
Server A : 웹 서버 구축   
Server B : FTP 서버 구축   
</br>

1. /etc/bind/named.conf 수정 / named-checkconf로 검사
Ex   
```
zone "URL" IN {
    type master;
    file "도메인 이름 설정 파일 경로";
}
```
2. 도메인 이름 설정 파일 생성 / named-checkzone [URL] [설정파일 이름]으로 검사
Ex
```
$TTL    3H
@   IN  SOA @   root.   (2 1D 1H 1W 1H)
@   IN  NS  @
    IN  A   네임서버 IP

www IN  A   웹 서버 IP
ftp IN  A   FTP 서버 IP

// $TTL : Time To Live
// @ : name.conf에 정의된 URL
// IN : internet
// SOA : Start of Authority, 괄호 내부는 (버전정보 리프레시 재접속 만료 정보삭제시간) 순
// NS : Name Server
// MX : Mail Exchanger, 메일 서버 컴퓨터
// A : 호스트 이름에 상응하는 IP 주소
// CNAME : 호스트 이름의 별칭
```
3. systemctl restart named

## 라운드 로빈 네임서버
서버의 부하를 분산시키기 위해 여러대의 서버를 운영할 경우 사용하는 네임서버

### 라운드 로빈 네임서버 구축
1. 마스터 네임서버의 2번까지 수행후 도메인 이름 설정 파일을 작성한다.
Ex
```
$TTL    3H
@   IN  SOA @   root.   (2 1D 1H 1W 1H)
@   IN  NS  @
    IN  A   네임서버 IP

ftp IN  A   FTP 서버 IP
www IN  CNAME webserver.[URL]
webserver   100 IN  A   [서버1 IP]
            200 IN  A   [서버2 IP]
            300 IN  A   [서버3 IP]

```
2. systemctl restart named