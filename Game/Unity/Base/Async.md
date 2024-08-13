# Asynchronous

## Thread
단순히 스레드를 하나 생성해서 처리한다. 수동으로 할당과 해제를 수행해야한다.

## Task
.Net에서 제공하는 비동기 프로그래밍이다. 작업을 ThreadPool에서 실행하도록 큐에 대기시키고, Task를 반환한다. 메인 스레드에서 작업하지 않기 때문에 Unity API에 직접 접근할 수 없다.

## UniTask
[UniTask Github](https://github.com/Cysharp/UniTask)   
UniTask의 대부분의 기능은 Player Loop에서 처리되어 코루틴을 대체하는 용도로 많이 쓰이지만, Unitask.SwithchToThreadPool이나 Unitask.RunOnThreadPool같은 메소드는 C#의 Task와 같은 기능을 수행한다. C#의 Task가 Unity에서 무겁다는 의견도 있으니 고려해볼만하다.

## Job System
Job을 통해 Unity에서 애니메이션이나 컬링 등을 처리하기 위해 할당해놓은 Worker Thread에 작업을 할당할 수 있게하는 시스템이다.   
IJob을 상속받은 구조체를 Schedule()을 통해 Worker Thread에서 처리하도록 등록하고 반환받은 Handle에 Complete()를 사용해서 작업이 완료될 때 까지 메인 스레드를 대기시킬 수 있다.   
메인 스레드는 NativeContainer를 통해서 Worker Thread의 처리 결과에 액세스 할 수 있다. 이 컨테이너는 대체로 4프레임 이하, 가능하면 1프레임 이하의 생명주기를 가질 것을 권장한다.   
