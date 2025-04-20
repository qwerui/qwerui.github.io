# Multiplexing

## 개요
하나의 통신채널을 통해서 둘 이상의 데이터를 전송하는데 사용되는 기술

## Select
여러 개의 파일 디스크립터를 관찰할 수 있는 함수, 수신 데이터 여부, 블로킹 및 데이터 전송 가능 여부, 예외 발생 여부를 관찰할 수 있다.
```c++
int select(int maxfd, fd_set* readset, fd_set* writeset, fd_set* exceptset, const struct timeval* timeout);
// fd_set이 파일 디스크립터 목록을 담는 배열이다. timeval은 timeout 시간을 담는 구조체다. sec, micro-sec 단위가 존재한다.
// 반환 값으로 -1이면 예외 발생, 0이면 타임아웃, 0보다 크면 변화가 발생한 파일 디스크립터다.

fd_set set; // fd_set은 리눅스에서 비트 배열이고, 윈도우는 구조체다. 다만 아래의 함수들의 사용법은 같기에 신경쓰지 않아도 된다.
FD_ZERO(&set); // set의 값을 모두 0으로 초기화
FD_SET(1, &set); // set의 1번 파일 디스크립터를 관찰
FD_CLR(1, &set); // set의 1번 파일 디스크립터를 관찰 해제
FD_ISSET(1, &readset); // readset의 1번 파일 디스크립터에 변화가 있는지 확인
```