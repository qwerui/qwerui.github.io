# Proxy Server

## 프록시 서버 작동 방식
1. 브라우저에서 프록시 서버에 요청
2. 프록시 서버가 데이터를 가져온뒤 캐시에 저장
3. 가져온 데이터를 브라우저로 전송
4. 이후 다른 브라우저가 동일 데이터를 요청할 경우 캐시된 데이터를 전송

## 구현
1. apt -y install squid
2. /etc/squid/squid.conf 수정
```
//아래 내용을 추가
acl 네트워크이름 서버IP/넷마스크
http_access allow 네트워크이름
cache_dir ufs 캐시디렉토리경로 캐시용량 캐시하위디렉토리개수 하위디렉토리의하위디렉토리개수
visible_hostname 네트워크이름
```
3. systemctl restart squid