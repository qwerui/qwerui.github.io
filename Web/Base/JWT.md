# JWT

## 개요
토큰 기반 인증으로 널리 사용되는 토큰. 헤더, 페이로드, 시그니처를 base64로 인코딩한 것

## Access & Refresh
- Access Token : 실제로 인증에 사용되는 토큰. 인증 가능 시간이 짧고 private js 변수에 저장해야한다.
- Refresh Token : Access Token을 발급 요청하기 위한 토큰. 인증 가능 시간이 길고 클라이언트에 저장되기 때문에 보안 처리가 필요

## 직접 구현할 때 쿠키 사용시 고려해야할 사항
1. ajax, axios 등의 요청을 보낼 때 클라이언트에서 Credential을 포함해야한다.
2. cors 설정에서 Credential을 포함해야한다.
3. 3-tier-architecture에서 HTTPOnly, Secure, SameSite=none 설정 확인
4. 로컬 개발 시 mkcert로 https 적용 확인 (검증 필요)