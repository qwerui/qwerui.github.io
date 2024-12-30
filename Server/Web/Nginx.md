# Nginx

## 주요 디렉토리
- /etc/nginx/ : 기본 설정이 저장된 루트 디렉토리
- /etc/nginx/nginx.conf : 기본 설정 파일
- /var/log/nginx/ : 로그 저장 디렉토리

## 주요 명령어
- nginx -h : 도움말
- nginx -v : 버전 확인
- nginx -V : 빌드 정보 포함 버전
- nginx -t : 설정 테스트
- nginx -s (stop/quit/relad/reopen) : 프로세스 중지, 종료, 설정 읽기, 로그 파일 읽기

## Docs
[Document](https://nginx.org/en/docs/)

## 부하 분산
### HTTP
```
upstream backend {
    # 부하분산 알고리즘
    # 라운드로빈 : 기본값, 순서대로 호출
    # 리스트 커넥션 : least_conn, 가장 연결이 적은 곳으로 호출
    # 리스트 타임 : least_time, 연결 수가 가장 적고 응답시간이 가장 빠른 곳으로 호출, Nginx plus 전용
    # 제네릭 해시 : hash, 해시값으로 접속 서버를 결정
    # 랜덤 : random, 임의의 서버로 연결
    # IP해시 : ip_hash, IP주소를 이용해 해시를 생성
    server one.server.com:80 weight=1;
    server two.server.com:80 weight=2;
    server backup.server.com:80 backup; #위 두 서버의 요청이 전부 실패하면 요청을 보내는 서버
}

server {
    location / {
        proxy_pass http://backend;
    }
}

```

### TCP/UDP
```
stream {
    upstream tcp_server {
        server one.tcp.com:11111;
        server two.tcp.com:11111;
        server backup.tcp.com:11111;
    }
    server {
        listen 11111; #뒤에 udp를 붙이면 udp 통신, 여러번 패킷을 주고 받아야한다면 udp reuseport를 사용
        proxy_pass tcp_server;
    }
}
```

## 헬스 체크
```
upstream backend {
    # 타임아웃 3초, 3번 실패
    server one.backend.com:1234 max_fails=3 fail_timeout=3s;
    server two.backend.com:1234 max_fails=3 fail_timeout=3s;
}

http {
    server {
        location / {
            proxy_pass http://backend;
            #Nginx plus 전용
            health_check interval=2s 
                         fails=2 
                         passes=5 
                         uri=/ 
                         match=welcome;
        }
    }
    # 아래 조건 확인
    match welcome {
        status 200; # 200 응답코드
        header Content-Type=text/html; # Content-Type이 text/html
        body ~ "Welcome to nginx!"; # Body에 Welcome to nginx!가 포함
    }
}
```

## 트래픽 관리
```
split_clients "${remote_addr}AAA" $variant {
    20.0% "backendv2"; # $variant는 20%확률로 backendv2가 할당 
    *     "backendv1";
}
```

### 국가 단위 접근 차단
```
http {
    # 1번 변수값에 따라 2번 변수값을 매핑
    # country_code의 출처는 geoip등을 사용
    map $country_code $country_access {
        "KOR" 1;
        default 0;
    }
    server {
        if($country_access = '0') {
            return 403;
        }
    }
}
```

### 실제 IP 조회
```
location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

### 클라이언트 당 연결 제한
```
http {
    # 10MB의 공유 메모리
    # $binary_remote_addr을 키값으로 가짐
    limit_conn_zone $binary_remote_addr zone=limitbyaddr:10m rate=3r/s;
    # 지정된 연결 수 초과 시 반환 코드
    limit_conn_status 429;

    server {
        limit_conn limitbyaddr 40; # 40개 까지 요청 허용
        limit_req zone=limitbyaddr; # 요청 빈도 제한
        limit_req zone=limitbyaddr burst=12 delay=9; # burst 이하는 허가, delay 이상은 지연 처리
    }
}
```

### 대역폭 제한
```
location /download/ {
    limit_rate_after 10m; # 대역폭 제한을 적용할 기준이 될 누적 전송량
    limit_rate 1m; # 누적 전송량 초과 시 대역폭 제한
}
```

## 캐싱
### 캐시 영역
```
proxy_cache_path /var/nginx/cache # 캐시를 저장할 디렉토리
                 keys_zone=CACHE:60m # 공유메모리 영역
                 levels=1:2 # 디렉토리 구조 레벨
                 inactive=3h # 마지막 요청에서 캐시를 삭제할 시간
                 max_size=20g; # 캐시 최대 크기
proxy_cache CACHE;
```

### 캐시 락
```
# 동일 요청에 대해 하나의 요청에 대해서만 캐시 생성
proxy_cache_lock on;
proxy_cache_lock_age 10s; # 캐시 생성 제한 시간, 실패시 다른 요청을 캐시 시도
proxy_cache_lock_timeout 3s; # 캐시 생성 제한시간, 실패시 upstream으로 요청을 보냄, 캐시 생성 X
```

### 캐시 키
```
# 동적인 페이지를 캐시하지만 다른 사용자의 콘텐츠가 잘 못 전달되지 않도록 함
proxy_cache_key "$host$request_uri $cookie_user";
```

### 캐시 우회
```
# 캐시를 가져오지 않는 조건 정의
proxy_cache_bypass $http_cache_bypass;
```

## 인증
### HTTP 인증
```
# C언어의 crypt() 함수 형식 비밀번호, openssl passwd로 얻을 수도 있다.
location / {
    auth_basic "Private Site"; # 인증 팝업 창 출력 내용
    auth_basic_user_file conf.d/passwd; # 사용자 이름, 비밀번호 파일 경로
}
```

### 인증 하위 요청
```
# http_auth_request_module
location /private/ {
    auth_request /auth;
    ...
}

location = /auth {
    proxy_pass ...
    proxy_pass_request_body off;
    proxy_set_header Content-Length "";
    proxy_set_header X-Original-URI $request_uri;
}
```

## 보안
### IP 기반 접근 제어
```
location /private/ {
    deny 10.0.0.1;
    allow 10.0.0.0/20;
    allow 2001:0db8::/32;
    deny all;
}
```

### CORS
```
server {
    location / {
        if($request_method = POST) {
            add_header 'Access-Control-Allow-Methods' 'POST';
            add_header 'Access-Control-Allow-Origin' 'localhost:3000';
            add_header 'Access-Control-Allow-Headers' 'Content-Type' 'User-Agent'
        }
    }
    if($request_method = OPTIONS) {
        add_header 'Access-Control-Allow-Methods' 'OPTIONS';
        add_header 'Access-Control-Allow-Origin' 'localhost:3000';
        add_header 'Access-Control-Allow-Headers' 'Content-Type' 'User-Agent'
        add_header 'Access-Control-Max-Age' 1728000; # 정책 캐싱
        return 204;
    }
}
```

### SSL
```
http {
    server {
        listen 7777 ssl http2; # http2도 여기서 설정
        ssl_protocols TLSv1.2 TLSv1.3; # TLS 버전
        ssl_ciphers HIGH:!aNULL:!MD5; # 암호화 알고리즘
        ssl_certificate /etc/nginx/ssl/ssl.crt; # 인증서
        ssl_certificate_key /etc/nginx/ssl/ssl.pem; # 비밀키
        # 연결 결과 캐싱
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;

        # 업스트림 암호화
        location / {
            proxy_pass https://example.site.com;
            proxy_ssl_verfy on;
            proxy_ssl_verify_depth 2;
            proxy_ssl_protocols TLSv1.2;
        }
    }
}
```

### HTTPS 리다이렉트
```
server {
    listen 80;
    return 301 https://$host$request_uri;
}
```

## 스트리밍
### MP4 & FLV
```
http {
    server {
        location /videos/ {
            mp4;
        }
        location ~ \.flv$ {
            flv;
        }
    }
}
```