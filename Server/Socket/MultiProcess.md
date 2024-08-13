# Multi Process

## 멀티프로세스
프로세스 : 메모리 상에서 실행중인 프로그램, 고유한 ID(PID)를 가지고 있다.   
멀티프로세스는 실행 중인 프로그램과 전혀 다른 작업을 수행하고 싶을 때(execlp) 사용한다. 예를 들면 런처 프로그램

### 프로세스 생성
- fork() : 성공시 프로세스 ID를 반환한다. 실패시 -1을 반환한다. fork를 호출한 시점에서 프로세스를 복제한다. 자식 프로세스에서는 0으로 초기화된다.
<br/>

fork를 호출 했을 때 소켓의 파일디스크립터도 함께 복제된다. 클라이언트의 소켓을 생성하고 프로세스를 복제해 자식 프로세스에서 작업을 처리하기 때문에 부모 프로세스에서는 클라이언트 소켓을 close해야한다. 마찬가지로 자식 프로세스에서는 서버 소켓을 close해야한다. 그렇지 않으면 자식 프로세스에서 클라이언트 소켓을 소멸 시킬 수 없다.

### 프로세스 종료
좀비 프로세스 : 작업을 완료한 뒤에 종료되지 않고 메모리를 차지하고 있는 프로세스   
자식 프로세스는 인자를 전달하면서 exit를 호출하거나 main에서 return을 실행하면서 값을 반환하는 경우에만 종료된다. 그런데 운영체제는 이 반환한 값이 부모 프로세스에 전달되기 전 까지는 자식 프로세스를 소멸시키지 않는다. 따라서 부모 프로세스에서 반환값을 받는 함수를 호출할 필요가 있다.   
<br/>

**반환 함수**   
- wait(int* statloc) : 성공시 종료된 자식 프로세스의 PID를 반환한다. 자식 프로세스의 반환 값은 statloc에 들어간다. statloc를 WIFEXITED에 넣으면 정상 종료 여부, WEXITSTATUS에 넣으면 반환한 값을 얻을 수 있다. 이 함수는 블로킹 함수다.
- waitpid(pid_t pid, int* statloc, int options) : pid에 종료를 확인하고자 하는 자식 프로세스의 pid를 넣는다. -1을 넣으면 임의의 자식 프로세스를 대기한다. options에 WNOHANG을 인자로 전달하면 블로킹 상태가 되지 않는다. 반환값 등 나머지는 wait과 동일

### 시그널 핸들링
```c++
void(*signal(int signo, void (*func)(int)))(int);
// signal 함수는 int, void를 반환하고 int를 매개변수로 받는 함수 포인터를 인자를 갖는다.
// signal 함수의 반환 값은 void(*)(int)타입 즉 void를 반환하고 int를 매개변수로 받는 함수 포인터를 반환한다.

unsigned int alarm(unsigned int seconds);
// SIGALRM을 발생시키는 함수, seconds 이후에 발생하고 0을 넘기면 이전 예약이 취소된다, 남은 시간을 반환

int sigaction(int signo, const struct sigaction* act, struct sigaction* oldact);
// act는 호출될 함수의 구조체, oldact는 이전에 등록했던 함수의 정보다.
// sigaction는 sa_handler에 함수를 저장한다.
// sa_mask는 모든 비트를 0(sigemptyset), sa_flags는 0으로 초기화한다.
```
위 함수를 실행하면 자식 프로세스가 종료되는 등의 상황에 함수를 자동으로 호출하게 한다.   

**signo의 값**   
- SIGALRM : alarm함수를 통해 등록된 시간이 된 상황
- SIGINT : CTRL+C
- SIGCHLD : 자식 프로세스 종료
- SIGURG : 긴급 데이터 수신