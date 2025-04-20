# Epoll

## select의 문제점
1. select 호출 후에 모든 파일 디스크립터를 대상으로 하는 반복문
2. select에 매번 전달해야하는 관찰 대상 정보

## Epoll 개요
리눅스 환경에서 사용가능한 select의 단점을 개선한 기술이다. 아래와 같은 함수를 사용한다.   
```c++
struct epoll_event
{
    __uint32_t events; // 이벤트
    epoll_data_t data;
}

// 이벤트 종류
EPOLLIN // 수신 데이터 존재
EPOLLOUT // 출력 버퍼 EMPTY
EPOLLPRI // OOB 수신
EPOLLRDHUP // 연결 종료 OR HALF-CLOSE
EPOLLERR // 에러 발생
EPOLLET // 엣지 트리거 방식 이벤트 감지
EPOLLONESHOT // 한 번 발생 이후 발생하지 않음

int epoll_create(int size); // epoll 파일 디스크립터 생성, size는 참고용이다. 리눅스 버전이 2.6.8 이상이면 완전히 무시된다.
int epoll_ctl(int epfd, int op, int fd, struct epoll_event* event); // 관찰 대상 등록

// op(관찰 대상 지정 옵션)
EPOLL_CTL_ADD // epoll에 등록
EPOLL_CTL_DEL // epoll에서 삭제
EPOLL_CTL_MOD // 파일 디스크립터의 이벤트 변경

int epoll_wait(int epfd, struct epoll_event* events, int maxevents, int timeout); // 관찰, timeout = -1이면 이벤트 발생 까지 무한 대기, 이벤트가 발생한 파일 디스크립터 수를 반환한다. 실패시 -1
```

## 레벨 트리거 & 엣지 트리거
- 레벨 트리거 : 활성화 상태(입력버퍼에 데이터가 있는 상태)면 이벤트 등록 => 상태 중심
- 엣지 트리거 : 비활성화 상태에서 활성화 상태가 되었을 때만 이벤트 등록 => 변화 중심
<br/>

소켓의 기본값은 레벨 트리거다. 엣지 트리거로 변경하면 두 가지를 알아야한다.
- errno를 이용한 오류 원인 파악 - ex: read가 읽을 것이 없다면 errno에는 EAGAIN이 저장된다.
- Non-blocking IO 소켓
```c++
int fcntl(int fildes, int cmd, ...); // cmd에 따라 값을 반환, 실패시 -1

// Non-blocking 설정
int flag = fcntl(fd, F_GETFL, 0);
fcntl(fd, F_SETFL, flag | O_NONBLOCK);

```

엣지 트리거를 사용할 경우 데이터 수신과 데이터 처리 시점을 분리할 수 있다.