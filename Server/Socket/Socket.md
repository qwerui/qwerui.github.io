# Socket

## 소켓
네트워크 상에서 데이터 송수신을 위한 소프트웨어 장치

## 소켓 생성 과정
### TCP 서버
1. 소켓 생성 => socket
2. IP주소와 포트 번호 할당 => bind
3. 연결 대기 => listen
4. 연결 수락 => accept

### TCP 클라이언트
1. 소켓 생성 => socket
2. 연결 요청 => connect

## 소켓 생성
### Protocol Family
- PF_INET : IPv4
- PF_INET6 : IPv6
- PF_LOCAL : UNIX 로컬 통신
- PF_PACKET : Low Level 소켓을 위한 프로토콜
- PF_IPX : IPX 노벨

### Type
- SOCK_STREAM : 스트림 통신, 연결, TCP(IPPROTO_TCP)
- SOCK_DGRAM : 데이터그램 통신, 비연결, UDP(IPPROTO_UDP)

### sockaddr_in
- sin_family : 주소 체계, AF_INET, AF_INET6, AF_LOCAL이 존재, PF와 1대1대응
- sin_port : 포트번호
- sin_addr : 주소 저장, 멤버로 s_addr만을 가지며 uint32_t로 되어있다.
- sin_zero : sockaddr과 크기를 맞추기 위한 멤버, 나중에 bind할 때 sockaddr_in을 sockaddr*로 캐스팅한다. sockaddr은 IP와 포트를 char타입의 크기 14의 배열이기 때문에 IP와 포트를 넣기 어렵게되어있다.

### IP & 포트 엔디안 변환
엔디안 변환 함수는 h(호스트) to n(네트워크) s(타입(short))로 구성되어있다.
보통 호스트(CPU)가 리틀 엔디안을 사용하고, 네트워크가 빅 엔디안을 사용하기 때문에 IP나 포트를 변환할 필요가 있다.

- htons : 포트를 호스트->네트워크로 변환
- htonl : IP를 호스트->네트워크로 변환
- ntohs : 포트를 네트워크->호스트로 변환
- ntohl : IP를 네트워크->호스트로 변환
- inet_addr : 문자열을 in_addr_t로 변환한다. 비정상적인 입력도 감지가능하다.
- inet_aton : IP문자열을 in_addr의 멤버에 넣는다. 성공/실패를 반환한다.
- inet_ntoa : inet_aton과 반대 작업을 수행한다.

### 서버의 소켓 생성 및 인터넷 주소 초기화
```c
int serv_sock; //Windows라면 SOCKET 타입을 권장
struct sockaddr_in addr;
char *serv_port="8081";

serv_sock = socket(PF_INET, SOCK_STREAM, IPPROTO_TCP);

memset(&addr, 0, sizeof(addr));
addr.sin_family = AF_INET;
addr.sin_addr.s_addr=htonl(INADDR_ANY); // 소켓이 동작하는 IP를 자동으로 할당
addr.sin_port=htons(atoi(serv_port));

bind(serv_sock, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
```

## TCP 소켓 연결
### 서버
- listen(int sock, int backlog) : 연결 대기 상태(listen)로 전환, sock에는 대기하고자 하는 소켓, backlog에는 연결요청 큐의 크기를 지정한다. 웹 서버 같이 잦은 연결 요청을 받는다면 15 이상 권장, 테스트를 통해 알아가야함
- accept(int sock, struct sockaddr* addr, socklen_t* addrlen) : sock에 요청을 받는 소켓, addr에 클라이언트의 주소 정보를 담을 구조체, addrlen에 구조체의 크기가 들어간다. 클라이언트와의 연결을 담당하는 파일 디스크립터를 반환한다.

### 클라이언트
- connect(int sock, const struct sockaddr* servaddr, socklen_t addrlen) : sock에 서버에 요청을 보낼 소켓, addr에 서버의 주소 정보를 담을 구조체, addrlen에는 구조체의 크기가 들어간다. 서버와 통신할 파일 디스크립터를 반환한다.

### 주의 사항
- 서버와 클라이언트는 데이터를 한 번에 전송하지 않을 수 있다. 보내야할 데이터의 크기가 크면 여러번 나누어 전송할 수 있는데, read를 한 번만 실행하면 일부만 도착한 데이터를 읽어들일수 있다. 따라서 데이터를 전부 수신할 때 까지 read를 수행해야하는데 3가지 방법이 있다.
1. 수신 받을 데이터의 크기를 확실히 알고 있는 경우 read에 알고 있는 크기를 넣는다.
2. 전송할 데이터에 데이터의 크기를 같이 전송한다. 루프를 돌리면서 데이터의 크기를 찾고 수신받은 데이터가 그 크기를 넘어가면 루프를 벗어난다.
3. 구분자를 이용해 구분자를 만날 때 까지 루프를 돌린다.

## UDP 소켓 연결
UDP는 서버와 클라이언트 모두 하나의 소켓만 있으면 된다. 하나의 소켓을 다수의 호스트를 대상으로 데이터를 송신할 수 있다. TCP에서는 bind나 connect로 소켓에 IP와 포트를 할당하는데, UDP에서는 bind를 사용하거나 sendto를 처음으로 호출하면 자동으로 할당한다.
<br/>

- sendto(int sock, void* buff, size_t nbytes, int flags, struct sockaddr* to, socklen_t addrlen) : UDP에서의 송신 함수, 이 함수에서 데이터와 데이터를 보낼 대상을 지정한다. sock에는 UDP소켓, to가 보낼 대상이다. 전송한 바이트 수를 반환한다.
- recvfrom(int sock, void* buff, size_t nbytes, int flags, struct sockaddr* from, socklen_t* addrlen) : UDP에서의 수신 함수, sock에 수신할 소켓, 수신 결과는 buff에 저장된다. from에 발신자 정보가 들어있다. 수신한 바이트 수를 반환한다.
<br/>

UDP 소켓은 데이터의 경계가 존재해 송신 함수와 수신 함수가 서로 호출된 횟수가 정확히 같아야한다.

### connected UDP
sendto가 호출되면 다음과 같은 과정을 거친다.
1. 소켓에 IP와 포트를 할당
2. 데이터 전송
3. 할당된 IP와 포트를 삭제
<br/>

할당과 삭제의 반복은 하나의 호스트와 오랜 시간 통신해야하는 경우면 비효율적으로 작용한다. 이를 위해 연결을 유지하는 방법이 있는데 connect 함수를 호출해 UDP 소켓을 넣으면 된다. connected UDP는 송수신할 대상이 정해졌기 때문에 read, write로도 통신할 수 있다.

## Windows 서버
Unix 환경 함수와 대응되는 함수가 대부분 존재한다. 소켓 라이브러리를 사용하기 전에 WSAStartup을 호출하고 서버를 종료하기 전에 WSACloseup을 호출해야한다. WSAStartup 함수는 WORD와 WSADATA를 매개변수로 받으며, WORD는 소켓 라이브러리의 버전, WSADATA는 초기화된 라이브러리의 정보가 들어간다.   

**대응되는 함수 목록**
- int serv_sock => SOCKET hServSock
- sockaddr_in => SOCKADDR_IN
- sockaddr => SOCKADDR
- -1 반환값 => SOCKET_ERROR
- read => recv
- write => send
- close => closesocket

## 소켓 종료
소켓은 두 호스트 간의 입력 버퍼와 출력 버퍼를 연결하는 것과 같다. 그런데 close나 closesocket은 소켓의 완전종료를 의미하기 때문에 두 버퍼 전부를 닫아버린다. 이런 상황은 통신이 종료되기 전에 송신한 데이터가 제대로 수신되지 않을 수 있다는 의미다. 이를 위해서 두 버퍼 중 한 쪽만 먼저 닫고 완전히 통신이 종료된 후에 소켓을 닫는 방법이 있다.

- shutdown(int socket, int howto) : 소켓과 닫는 방법을 넘긴다. 성공하면 0 실패하면 -1을 반환한다.
</br>

**닫는 방법**   
- SHUT_RD : 입력 버퍼 닫기
- SHUT_WR : 출력 버퍼 닫기
- SHUT_RDWR : 입출력 버퍼 닫기
- SD_RECEIVE : 입력 버퍼 닫기(WINDOWS)
- SD_SEND : 출력 버퍼 닫기(WINDOWS)
- SD_BOTH : 입출력 버퍼 닫기(WINDOWS)

## 소켓의 옵션
### 소켓 옵션 get & set
- getsockopt(int sock, int level, int optname, void* optval, socklen_t* optlen)
- setsockopt(int sock, int level, int optname, void* optval, socklen_t* optlen)
<br/>

**매개변수**
- sock : 참조할 소켓
- level : 프로토콜 레벨(SOL_SOCKET, IPPROTO_IP, IPPROTP_TCP)
- optname : 옵션 이름
- optval : 옵션을 획득/수정할 버퍼
- optlen : optval의 크기

### 대표적인 옵션 이름
- SO_TYPE : 소켓의 타입
- SO_SNDBUF/SO_RCVBUF : 출력/입력 버퍼 크기
- SO_REUSEADDR : Time-wait 상태의 소켓에 할당되어 있는 포트를 새로 시작하는 소켓에 할당되게함
- TCP_NODELAY : Nagle 알고리즘을 중단

## 입출력 함수
### send & recv
send와 recv는 리눅스에서도 사용 가능하다. 기능도 윈도우와 차이나지 않는다. 4번째 매개변수에 데이터 송수신 시 옵션을 지정할 수 있다. OR연산자(|)로 여러 개를 지정할 수 있다. 리눅스에서는 수신 시 SIGURG가 발생하기 때문에 sigaction을 등록해야한다. 윈도우에서는 exception으로 처리되기 때문에 select의 exception buffer를 이용해 처리해야한다.   

**옵션 목록**   
- MSG_OOB : 긴급 데이터 전송/수신
- MSG_PEEK : 입력버퍼에 수신 데이터 존재 유무 확인
- MSG_DONTROUTE : 데이터 전송에 라우팅 테이블을 사용하지 않음 = 로컬 네트워크에서 목적지를 찾기 위한 옵션
- MSG_DONTWAIT : 블로킹하지 않음
- MSG_WAITALL : 모든 데이터가 수신될 때 까지 대기

### readv & writev
```c++
ssize_t writev(int filedes, const struct iovec* iov, int iovcnt);
ssize_t readv(int filedes, const struct iovec* iov, int iovcnt);

// iovec은 데이터를 담는 배열과 그 크기를 담는다.
// iovcnt는 iov 배열의 크기다.
```
데이터를 모아서 송수신 하는 기능을 가지고있다. 전송할 데이터가 여러개의 버퍼에 나뉘어져 있는 경우나, Nagle 알고리즘을 종료한 경우 효과적이다. 함수 호출의 횟수가 적어지고 패킷의 수가 줄어들 확률이 높아 패킷 헤더의 수도 감소한다. 따라서 성능 향상에도 이점이 있다.

## 기타 주의사항
### Time-wait
TCP연결에서 소켓이 먼저 닫히는 쪽이 일정 시간동안 대기 시간을 갖는다. 이는 반대쪽에서 FIN 메시지를 전송하고 난 뒤 연결 종료를 요청한 쪽에서 ACK를 전송할 때 중간에 소멸하는 경우 FIN 메시지를 다시 전송하려 할 것이다. 만약 Time-wait가 없다면 반대편에선 영원히 ACK 메시지를 받을 수 없을 것이다.   
이를 방지하기 위해 연결 종료를 요청한 쪽에서 대기 시간을 갖는데 이것이 Time-wait다. Time-wait인 동안에는 소켓이 해당 포트를 점유하고 있어 사용할 수 없다. 다만 주로 서버가 종료되었을 때에만 문제가 발생하는데, 클라이언트는 프로그램이 실행될 때 마다 유동적으로 PORT가 할당되기 때문에 문제가 발생하지 않는다.

### Nagle
TCP에서 적용되는 알고리즘, 앞서 전송한 데이터에 대한 ACK를 받아야 다음 데이터를 전송하는 알고리즘이다. 이 알고리즘을 사용하는 이유는 네트워크 트래픽 때문이다. 1바이트의 데이터를 보내더라도 헤더 정보가 수십 바이트이기 때문에 출력 버퍼에 데이터가 채워지자마자 전송하는 것은 비효율적이다.   
다만 용량이 큰 파일을 전송할 때에는 이 알고리즘이 오히려 느려지는 원인이 되기도한다. 출력 버퍼에 데이터를 넣는 작업은 오래걸리는 작업이 아니기 때문에 용량이 큰 파일은 금방 출력 버퍼를 꽉 채울 것이다. 이 상황에서 Nagle을 사용해제하면 ACK를 기다릴 필요가 없어 전송속도가 빨라진다.

### 표준 입출력 사용
소켓은 fgets, fputs를 이용해 읽기 쓰기가 가능하다. 송신 시에는 입출력 버퍼->소켓 버퍼->전송, 수신 시에는 소켓 버퍼->입출력 버퍼->수신 순으로 이루어진다. fclose를 호출하면 상대 호스트로 EOF가 송신된다.
<br/>

**장점**   
- 이식성이 좋다
- 버퍼링을 통한 성능 향상

**단점**   
- 양방향 통신이 쉽지 않음
- 파일 디스크립터릴 FILE*로 변환해야함
- fflush의 빈번한 호출 가능성

```c++
FILE* fdopen(int filedes, const char* mode); // FILE*로 변환
int fileno(FILE* stream); // 파일 디스크립터로 변환

// 파일 디스크립터 복사
int dup(int filedes); // 복사된 파일 디스크립터 반환
int dup2(int filedes, int filedes2); //명시적으로 지정

/*
표준 입출력을 이용할 경우 파일 디스크립터도 복제해야한다. fclose를 호출했을 때 파일 디스크립터도 같이 사라지기 때문에 복제하지 않으면 소켓이 소멸된다. Half-close를 하기 위해서는 쓰기모드의 파일 디스크립터를 shutdown해야한다. shutdown을 호출해도 클라이언트로 EOF가 전송된다.
*/
```