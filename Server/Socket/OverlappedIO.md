# Overlapped IO

## 개요
IO 중첩은 하나의 쓰레드 내에서 둘 이상의 영역으로 데이터를 송수신함으로써, 입출력이 중첩된 상황이다. 비동기 상태에서 하나를 전송하고 바로 다음을 전송 시도하게 된다면 두 전송이 동시에 진행되는 시간이 존재할 것이다.

## 함수
```c++
// Overlapped IO 소켓 생성
SOCKET WSASocket(int af, int type, int protocol, LPWSAPROTOCOL_INFO lpProtocolInfo, GROUP g, DWORD dwFlags);
// af : 프로토콜 체게 정보
// type : 소켓의 데이터 전송 방식
// protocol : 프로토콜
// lpProtocolInfo : 소켓의 특성을 담는 구조체
// g : 함수의 확장을 위해 예약 => 0 전달
// dwFlags : 소켓의 속성정보

// 아래와 같이 생성한다.
WSASocket(PF_INET, SOCK_STREAM, 0, NULL, 0, WSA_FLAG_OVERLAPPED);

// 송수신 (송수신이 완료되면 0을 반환하고, 그렇지 않으면 SOCKET_ERROR를 반환한다)
int WSASend(SOCKET s, LPWSABUF lpBuffers, DWORD dwBufferCount, LPDWORD lpNumberOfBytesSent, DWORD dwFlags, LPWSAOVERLAPPED lpOverlapped, LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine);
int WSARecv(SOCKET s, LPWSABUF lpBuffers, DWORD dwBufferCount, LPDWORD lpNumberOfBytesRecvd, LPDWORD lpFlags, LPWSAOVERLAPPED lpOverlapped, LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine);

// 데이터 입출력 정보 확인
BOOL WSAGetOverlappedResult(SOCKET s, LPWAOVERLAPPED lpOverlapped, LPDWORD lpcbTransfer, BOOL fWait, LPDWORD lpdwFlags);
```

## 입출력 완료 확인
### LPWSAOVERLAPPED, Event 기반
1. WSASend, WSARecv의 결과가 SOCKET_ERROR면 송수신이 완료되지 않았다.
2. WSAGetLastError() == WSA_IO_PENDING이면 송수신중이다.
3. WSAWaitForMultipleEvents를 이용해 signaled가 될 때 까지 대기한다.
4. WSAGetOverlappedResult를 이용해 전송 결과를 확인한다.

### Completion Routine 기반
1. WaitFor계열 함수나 SleepEx에 TRUE를 주면 alertable wait 상태가 된다. 이 때 이 함수들은 WAIT_IO_COMPLETION을 반환한다.
2. 송수신이 완료되면 등록해둔 함수가 실행된다.
<br/>

```c++
// Completion Routine은 반드시 아래의 원형을 가진다
void CALLBACK CompletionRoutine(DWORD dwError, DWORD cbTransferred, LPWSAOVERLAPPED lpOverlapped, DWORD dwFlags);
```