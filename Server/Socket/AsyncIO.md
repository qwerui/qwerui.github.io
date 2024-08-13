# Asynchronous Notification IO

## Notification IO
입력버퍼에 데이터가 수신되어서 데이터의 수신이 필요하거나, 출력버퍼가 비어서 데이터 전송이 가능한 상황을 알림

## 비동기 구현
```c++
// 관찰 등록
int WSAEventSelect(SOCKET s, WSAEVENT hEventObject, long lNetworkEvents);
// s : 소켓, hEventObject : 이벤트 발생 핸들, lNetworkEvents : 이벤트 유형

// lNetworkEvents 값 (|를 이용해 여러개 지정 가능)
FD_READ : 수신 데이터 존재
FD_WRITE : 블로킹 없이 송신 가능
FD_OOB : Out-of-Band 수신
FD_ACCEPT : 연결 요청
FD_CLOSE : 연결 종료 요청

// WSAEVENT 생성/소멸 (WSAEVENT는 HANDLE을 define한 것)
WSAEVENT WSACreateEvent();
BOOL WSACloseEvent(WSAEVENT hEvent);

// 이벤트 발생 확인
DWORD WSAWaitForMultipleEvents(DWORD cEvents, const WSAEVENT* lphEvents, BOOL fWaitAll, DWORD dwTimeout, BOOL fAlertable); // 기존함수의 반환 값에 더해 signaled가 된 인덱스 중 가장 작은 인덱스를 반환한다.

// 이벤트 종류 확인
int WSAEnumNetworkEvents(SOCKET s, WSAEVENT hEventObject, LPWSANETWORKEVENTS lpNetworkEvents);
// 실제 이벤트 종류는 lpNetworkEvents에 담긴다. 에러도 같이 담기며 에러는 에러 배열에서 이벤트에 해당하는 비트가 0이 아니게된다.
```