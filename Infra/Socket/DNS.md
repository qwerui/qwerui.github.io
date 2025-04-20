# DNS

## 개요
IP를 도메인 주소로 변환하는 시스템

## 도메인-IP 변환
아래 함수들은 Windows에서도 동일한 함수를 가진다.

### 도메인->IP
- gethostbyname(const char* hostname) : 성공 시 struct hostent* 반환

hostent는 아래와 같은 멤버를 가지고 있다.
- h_name : 공식 도메인 이름
- h_aliases : 공식 도메인 이름 외의 도메인 이름
- h_addrtype : IPv4(AF_INET), IPv6(AF_INET6)
- h_length : IP의 크기 정보
- h_addr_list : 실제 IP의 문자열 배열

### IP->도메인
- gethostbyaddr(const char* addr, socklen_t len, int family) : IP주소와 길이, 주소체계를 넘긴다. 반환은 gethostbyname과 같은 구조체 포인터를 반환한다. 즉 h_name과 h_aliases를 보면 된다.