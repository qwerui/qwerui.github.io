# Multi Thread

## 쓰레드 생성 및 실행
쓰레드를 사용하려면 컴파일 시 -lpthread를 추가해야한다.

```c++
// 생성
int pthread_create(pthread_t* restrict thread, const pthread_attr_t* restrict attr, void*(*start_routine)(void*), void* restrict arg);
// thread = 쓰레드 ID 반환
// attr = 쓰레드 특정 정보 전달, NULL이면 기본값
// start_routine : 쓰레드에서 실행할 함수
// arg : 쓰레드에 실행할 함수에 넣을 매개변수
// 성공시 0 반환 실패 시 0 이외의 값

int pthread_join(pthread_t thread, void** status); // 성공 시 0 반환, status에 thread의 반환 값이 들어간다.
```

일반적으로 쓰레드 안전한 표준 함수는 _r을 접미어로 가진다. 쓰레드 안전하지 않은 함수라도 _REENTRANT를 define하거나 컴파일 할 때 -D_REENTRANT를 추가하면 된다.

## 쓰레드 동기화
```c++
// 뮤텍스
// 성공 시 0, 실패 시 0 이외의 값, mutex에 뮤텍스가 할당/삭제된다.
int pthread_mutex_init(pthread_mutex_t* mutex, const pthread_mutexattr_t* attr);
int pthread_mutex_destroy(pthread_mutex_t* mutex);

int pthread_mutex_lock(pthread_mutex_t* mutex); //임계영역 시작
int pthread_mutex_unlock(pthread_mutex_t* mutex); //임계영역 종료

// 세마포어
// 성공 시 0, 실패 시 0 이외의 값, sem에 세마포어가 할당, pshared는 둘 이상의 프로세스에 의해 접근 가능한 세마포어 생성
int sem_init(sem_t* sem, int pshared, unsigned int value);
int sem_destroy(sem_t* sem);

int sem_wait(sem_t* sem); //임계영역 시작
int sem_post(sem_t* post); //임계영역 종료
```

## Windows 쓰레드
```c++
// 생성
HANDLE CreateThread(LPSECURITY_ATTRIBUTES lpThreadAttributes, SIZE_T dwStackSize, LPTHREAD_START_ROUTINE lpStartAddress, LPVOID lpParameter, DWORD dwCreationFlags, LPDWORD lpThreadId); // 성공시 쓰레드 핸들, 실패 시 NULL 반환
// lpThreadAttributes : 쓰레드 보안 관련 정보, NULL이면 기본설정
// dwStackSize : 쓰레드에 할당할 스택 크기, 0이면 기본 스택 크기
// lpStartAddress : 쓰레드의 main 함수
// lpParameter : 쓰레드의 main 함수에 전달할 인자 정보
// dwCreationFlags : 쓰레드 생성 이후 행동, 0이면 생성과 동시에 실행
// lpThreadId : 쓰레드 ID 저장

// 안전한 표준 함수 호출을 위한 쓰레드 생성
uintptr_t _beginthreadex(void* security, unsigned stack_size, unsigned (*start_address)(void*), void* arglist, unsigned initflag, unsigned* thrdaddr);
// CreateThread와 동일 구조, 메인 함수에 WINAPI(__stdcall) 지정 필요

// 쓰레드 상태 확인
DWORD WaitForSingleObject(HANDLE hHandle, DWORD dwMilliseconds);
DWORD WaitForMultipleObjects(DWORD nCount, const HANDLE* lpHandles, BOOL bWaitAll, DWORD dwMilliseconds);
// signaled : 어떠한 이벤트 발생, 대표적으로 쓰레드 종료
// signaled 상태라면 WAIT_OBJECT_0, 타임아웃이면 WAIT_TIMEOUT 반환
// bWaitAll가 TRUE면 모든 검사 대상이 signaled여야 반환

// 유저모드 동기화
void InitializeCritialSection(LPCRITICAL_SECTION lpCriticalSection); //객체 초기화
void DeleteCritialSection(LPCRITICAL_SECTION lpCriticalSection); // 자원반납
void EnterCritialSection(LPCRITICAL_SECTION lpCriticalSection); // 소유권 획득
void LeaveCritialSection(LPCRITICAL_SECTION lpCriticalSection); // 소유권 반납

// 커널모드 동기화
BOOL CloseHandle(HANDLE hObject); // 커널 오브젝트 소멸
// 뮤텍스 (쓰레드가 소유중이면 non-signaled 상태다)
HANDLE CreateMutex(LPSECURITY_ATTRIBUTES lpMutexAttributes, BOOL bInitialOwner, LPCTSTR lpName);
// bInitialOwner = TRUE => 소유권이 호출한 쓰레드, non-signaled 상태, FALSE => 소유권없음, signaled 상태
BOOL ReleaseMutex(HANDLE hMutex); // Mutex 반납, WaitFor-함수와 같이 사용된다.

// 세마포어
HANDLE CreateSemaphore(LPSECURITY_ATTRIBUTES lpSemaphoreAttributes, LONG lInitialCount, LONG lMaximumCount, LPCTSTR lpName);
// 세마포어의 값은 lInitialCount <= x <= lMaximumCount가 된다.
BOOL ReleaseSemaphore(HANDLE hSemaphore, LONG lReleaseCount, LPLONG lpPreviousCount); // 세마포어 반납, WaitFor-함수와 같이 사용된다.

// 이벤트
HANDLE CreateEvent(LPSECURITY_ATTRIBUTES lpEventAttributes, BOOL bManualReset, BOOL bInitialState, LPCTSTR lpName);
// bManualReset : manual-reset, auto-reset 설정, auto-reset이면 signaled 상태가 된 후 쓰레드 중 하나가 깨어난다. 그 후 non-signaled 상태로 전환, manual-reset이면 ResetEvent가 호출되기 전에는 signaled가 유지되기 때문에 여러개의 쓰레드를 깨울 수 있다.
// bInitialState : 초기 signal 상태

BOOL ResetEvent(HANDLE hEvent); // non-signaled로 전환
BOOL SetEvent(HANDLE hEvent); // signaled로 전환
```

### 연산 모드
- 유저모드 : 응용 프로그램이 실행되는 기본 모드, 물리/메모리 영역에 접근이 제한된다.
- 커널모드 : 운영체제가 실행될 때 모드, 물리/메모리 영역 접근에 제한이 없다.
<br/>

응용 프로그램은 유저모드와 커널모드를 번갈아 가며 실행한다. 쓰레드를 생성하는 것은 운영체제이기 때문에 커널모드로 전환해야한다. 2모드로 나뉘어져 있는 이유는 안전을 위함이다. 유저모드에서는 운영체제와 관련된 메모리 영역이 보호받는다.
<br/>

**모드 별 동기화**   
- 유저모드 동기화 : 속도가 빠르다
- 커널모드 동기화 : 기능이 많고, 타임아웃을 지정해 Dead-lock을 예방할 수 있다. (커널 오브젝트를 기반으로 해 서로 다른 프로세스 간 동기화도 가능하다.)