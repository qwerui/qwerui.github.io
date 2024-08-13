# Process

## 프로세스 상태
- 생성 : 프로세스 생성 상태
- 실행 : 작업을 처리 중인 상태
- 대기 : IO 등의 처리에 의한 대기 상태
- 준비 : 아직 CPU 자원이 할당되지 않은 상태
- 종료 : 프로세스 종료 상태

## PCB
운영체제에서 관리하는 프로세스의 정보, 이를 이용해 프로세스를 제어한다. PCB의 정보를 Context라고 볼 수 있다. 즉 Context Switch는 아래와 같은 3작업을 수행하는 것이다.
1. 다른 프로세스에 CPU 자원을 할당
2. 현재 프로세스의 상태 저장
3. 다른 프로세스의 상태 복구

### 정보 목록
- 프로세스 상태
- 프로그램 카운터 : 다음 실행될 명령어를 지정하는 카운터
- 레지스터
- CPU 스케줄링 정보
- 메모리 관리 정보
- 계정 정보
- I/O 상태 정보

### 스케줄링 큐
일반적으로 linked list로 구현한다.
- ready queue : 프로세스가 CPU 자원을 할당 받기위한 순서 큐
- wait queue : IO 작업을 수행하기 위한 큐

## IPC
프로세스간 통신, 공유 메모리(POSIX)를 사용하거나 메시지(PIPE)를 보내는 것으로 통신한다.

### 공유 메모리
```c
// 한 번 읽어오면 다시 써야한다.
// shm_open과 mmap의 권한은 일치해야한다.
fd = shm_open(name, O_CREAT | ORDWR, 0666); // 공유 메모리 오브젝트 생성
ftruncate(fd, 1024); // 오브젝트 크기 설정
mmap(0, SIZE, PROT_READ | PROT_WRITE, MAP_SHAREDM, fd, 0); // 메모리 매핑 파일
```

### pipe
```c
// pipe는 2개(송신, 수신)의 파일 디스크립터가 필요하다. 하나의 파이프만 사용하면 송신한 데이터를 자기 자신이 수신하게 되는 참사가 발생할 수 있다.
```

## 스케줄링
프로세스에 CPU 자원을 할당/해제할 지 방법을 정의, Context Switch 방법을 정의하는 것으로 볼 수 있다.   

- 스케줄러 : 어떤 프로세스에 할당할지 선택
- 디스패쳐 : 실제로 스위칭을 일으키는 모듈
<br/>

**상황에 따른 결정**
1. 실행->대기 : 비선점
2. 실행->준비 : 선점 or 비선점
3. 대기->준비 : 선점 or 비선점
4. 종료 : 비선점

### 종류
- FCFS : First-Come, First-Served - 먼저 요청한 것을 먼저 실행, 비선점
+ convoy effect : 하나의 거대한 프로세스에 의해 다른 프로세스가 실행되지 못하는 상태
- SJF : Shortest Job First - 짧은 작업을 먼저 실행, 선점/비선점 둘 다 가능
- SRTF : Shortest Remaining Time First - 남은 시간이 가장 짧은 작업을 먼저 실행, 선점
- RR : 라운드-로빈 - 시분할 실행, 각 프로세스가 정해진 시간만큼만 실행한다. 선점형 FCFS
- Prioity-based : 우선순위를 부여해 우선순위에 따라 작업을 실행
+ 기아 상태 : 우선순위가 높은 작업이 지속적으로 요청되어 우선순위가 낮은 작업이 실행되지 않는 상태
- MLQ : Multi-Level Queue, 우선순위에 따른 ready queue를 별도로 할당
- MLFQ : Multi-Level Feedback Queue, 단위 시간에 따른 ready queue를 별도로 할당

## 동기화
### Peterson's Algorithm
두 프로세스를 번갈아 실행하도록 제한, critical section과 remainder section을 번갈아 실행
```c
while(true)
{
    flag[i] = true;
    turn = j;
    while(flag[j] && turn==j);
    // critical section
    flag[i] = false;
    //remainder section
}
// 위 코드는 사실 정상적으로 동기화가 일어나지 않는다.
// 하드웨어 수준에서 원자성을 보장하는 코드를 사용해야한다.
// 아래 코드를 의사코드이며 사용자 수준에서 직접 입력해도 원자성을 보장하기 어렵다.
bool test_and_set(bool* target)
{
    bool rv = *target;
    *target = true;
    return rv;
}
int compare_and_swap(int* value, int expected, int new_value)
{
    int temp = *value;
    if(*value == expected)
    {
        *value = new_value;
    }
    return temp;
}

```

## 동기화 관련 문제
아래의 문제들(데드락, 기아상태)를 해결하기 위한 방법으로 Transaction Memory, OpenMP, 함수형 프로그래밍등을 사용하기도 한다.
### Bounded Buffer
한정된 버퍼를 어떻게 동기화 할 것인가?
- 생산자와 소비자가 배타적으로 접근
- 생산자는 버퍼에 데이터를 넣을 수 있을 때 생산하고, 소비자는 버퍼에 데이터가 존재할 때 소비한다.
```c
//생산자
whlie(true)
{
    //생산
    wait(empty);
    wait(mutex);
    //버퍼에 추가
    signal(mutex);
    signal(full);
}

//소비자
while(true)
{
    wait(full);
    wait(mutex);
    //버퍼에서 획득
    signal(mutex);
    signal(empty);
    //소비
}
```

### Reader-Writer
다수의 Reader와 Writer들이 하나의 버퍼를 공유하며 이를 접근할 때 발생하는 문제, 기아 상태가 발생할 수 있는 상황이다.
- Reader는 다른 Reader가 읽기를 끝내지 않아도 대기하지 않는다.
- Writer가 접근할 때에는 Reader가 접근해선 안된다.
```c
//Writer
while(true)
{
    wait(rw_mutex);
    // writing...
    signal(rw_mutex);
}

//Reader
while(true)
{
    wait(mutex);
    read_count++;
    if(read_count == 1)
    {
        wait(rw_mutex);
    }
    signal(mutex);
    //reading...
    wait(mutex);
    read_count--;
    if(read_count == 0)
    {
        signal(rw_mutex);
    }
    signal(mutex);
}
```

### 식사하는 철학자 문제
![출처](https://ko.wikipedia.org/wiki/%EC%8B%9D%EC%82%AC%ED%95%98%EB%8A%94_%EC%B2%A0%ED%95%99%EC%9E%90%EB%93%A4_%EB%AC%B8%EC%A0%9C)   
다섯 명의 철학자가 원탁에 앉아 있고, 각자의 앞에는 스파게티가 있고 양옆에 포크가 하나씩 있다. 그리고 각각의 철학자는 다른 철학자에게 말을 할 수 없다. 이때 철학자가 스파게티를 먹기 위해서는 양 옆의 포크를 동시에 들어야 한다. 어떤 철학자도 굶지 않고 식사할 수 있는 방법은?
<br/>

**해결법**   
다만 데드락만 해결할 수 있고, 기아상태는 해결하지 못한다.
1. 철학자를 4명으로 줄인다.
2. 양쪽의 포크가 사용 가능할 때만 포크를 집는다.
1) 철학자는 생각, 배고픔, 식사 3가지 상태를 갖는다
2) 양쪽의 철학자가 식사상태가 아닌 경우에만 식사상태로 전환할 수 있다.
3. 홀수번의 철학자는 왼쪽부터 잡고, 짝수번의 철학자는 오른쪽부터 잡는다.