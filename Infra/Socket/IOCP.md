# IOCP

## Overlapped IO만 사용 시 단점
- accept함수와 alertable wait 상태로 진입하기 위한 함수가 번갈아 호출되는 것은 성능에 영향을 미칠 수 있다.
=> accept는 main에서, 별도의 쓰레드로 클라이언트의 입출력을 담당하는 것이 IOCP의 모델

## IOCP 사용 시 이점
1. Non-blocking IO로 인한 작업 지연 시간 감소
2. IO가 완료된 핸들을 찾기 위해 반복문을 사용할 필요가 없음
3. IO의 진행 대상인 핸들을 배열에 저장 및 관리할 필요가 없음
4. IO 처리에 사용할 쓰레드 수를 조절 할 수 있음

## Completion Port
완료된 IO의 정보가 등록되는 커널 오브젝트, 오브젝트를 생성 후 소켓과 연결을 해야하는데 이 때 소켓은 Overlapped 속성이 부여되어 있어야한다.   
IOCP는 입력과 출력을 구분시켜주지 않는다. 단지 완료되었다는 사실만 알려준다. 따라서 별도로 입력과 출력을 진행한 정보를 담아야한다.

```c++
// 생성
HANDLE CreateIoCompletionPort(HANDLE FileHandle, HANDLE ExistingCompletionPort, ULONG_PTR CompletionKey, DWORD NumberOfConcurrentThreads);
// 성공 시 CP 오브젝트의 핸들이 반환, 실패시 NULL
// 순서대로 INVALID_HANDLE_VALUE, NULL, 0, IO를 담당할 쓰레드 개수를 전달한다.

// 소켓 연결도 위의 함수를 사용한다, 소켓이 연결되면 연결되 소켓을 기반으로 하는 Overlapped IO가 완료되면 아래 함수가 반환된다.
// 순서대로 소켓 핸들, CP 오브젝트 핸들, 완료된 IO 정보를 담을 공간, 마지막은 무시된다.

// 완료된 IO 확인
BOOL GetQueuedCompletionStatus(HANDLE CompletionPort, LPDWORD lpNumberOfBytes, PULONG_PTR lpCompletionKey, LPOVERLAPPED* lpOverlapped, DWORD dwMilliseconds);
// lpCompletionKey가 Create-의 3번째에 들어간다.
// lpOverlapped는 WSASend, WSARecv에 전달되는 주소 값이다.
// 이 함수를 통해서 IO 완료를 확인하는데, 어떠한 쓰레드라도 호출할 수 있다. 다만 실제로 응답을 받는 쓰레드 수는 지정한 최대 쓰레드 수를 넘지 않는다.
```