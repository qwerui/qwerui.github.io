# Web

## 프런트엔드 서버 구축
1. nginx 기반 docker 생성
2. nginx의 root에 Vue 등의 파일 위치
3. nginx의 프록시 설정
```
server {
    listen       3000;
    listen  [::]:3000;
    server_name  <<ip>>;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    location /api/ {
        proxy_pass http://[backend api container]:8083/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real_IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

## Spring 서버 구축
1. Gradle의 Bootjar로 jar 파일 생성
2. Dockerfile 작성
```
FROM openjdk:21
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
3. docker build후 run

## 주의사항
- 코드 내에서 시스템의 시간을 사용하는 경우 동기화가 필요하다.
- 리다이렉팅은 외부에서 접속 가능한 주소를 지정해야한다. 전달받은 url로 재접속 하기 때문에 docker 내부 네트워크는 사용할 수 없다.