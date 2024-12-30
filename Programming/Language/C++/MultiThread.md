# Multi Thread

## 스레드 생성
1. CreateThread, _beginthread, pthread_create => 1개의 매개변수만 가능
2. std::thread => 2개 이상의 매개변수 가능

## Thread Local Storage
변수에 thread_local을 붙여 사용, 스레드 내에서만 유효한 변수가 되며, 각 스레드에서 1번만 초기화된다.

## Atomic Type
atomic<type>으로 사용, 뮤텍스나 세마포어 같은 동기화 기법을 적용하지 않고도 공유자원에 원자적 접근을 가능하게 해주는 타입이다. 다만 내부적으로 동기화 기법을 써야할 필요가 있는 경우도 있다. 사용자 정의 타입도 가능하지만 쉽게 복제가 가능해야한다. is_lock_free를 사용하면 lock이 필요한 변수인지 알 수 있다.   

## 뮤텍스
시간제약이 없는 뮤텍스 : mutex, recursive_mutex, shared_mutex   
시간제약이 있는 뮤텍스 : timed_mutex, recursive_timed_mutex, shared_timed_mutex   

**lock**   
RAII를 이용해 뮤텍스에 락을 걸거나 해제하기 쉽도록하는 클래스, smart pointer의 mutex 버전이라고 볼 수 있다.   
lock_guard : 기본적인 lock, 호출한 이후의 스코프 영역을 lock한다.   
unique_lock : 지연 등 고급 기능을 포함한다.   
shared_lock : 공유 뮤텍스에 대한 기능을 포함   
scoped_lock : lock_guard를 여러 뮤텍스에대해 사용할 수 있다   

**call_once, once_flag**   
여러 스레드가 호출하더라도 함수가 1번만 실행되도록하는 기법이다. call_once(once_flag, function pointer)로 사용할 수 있다. 공유 자원을 초기화하는 등의 상황에 유용하다.

## 조건 변수
조건을 만족하기 전까지 스레드를 정지하는 기능을 제공한다.   
**condition_variable** : unique_lock만 기다리는 조건변수   
**condition_variable_any** : 커스텀 락 타입 등 모든 객체를 기다릴 수 있는 조건변수   
<br/>

**메소드 목록**
notify_one : 조건 변수를 기다리는 스레드 중 하나를 깨운다   
notify_all : 조건 변수를 기다리는 스레드 모두를 깨운다   
wait : 알림이 올 때 까지 스레드를 블록   
wait_for : 알림이 오거나 지정된 시간이 만료되면 스레드를 깨움   
wait_until : wait_for의 절대 시간 버전   
<br/>

**주의사항**   
1. notify나 지정된 시간이 지나지 않아도 스레드가 깨어나는 문제가 있다. wait에 predicate를 지정해서 방지할 수 있다.
2. notify 이후에 wait이 걸리는 경우 스레드가 영원히 대기상태가 되는 문제가 발생한다.

## Promise/Future
스레드의 결과를 가져오고 싶을 때 사용한다. Promise가 입력, Future가 출력이다. 스레드에 Promise를 매개변수로 넘겨주고 스레드 내에서 Promise에 값을 set하면 나중에 Future에서 그 값을 얻을 수 있다.   

**packaged_task** : Promise 대신 사용하며 스레드 내에서의 return 값을 받아온다.   
**async** : 함수를 다른 스레드에서 실행할 지, future에서 get을 호출할 때 동기식으로 실행할 지를 결정할 수 있다.   
위 두 기능을 사용하거나 Promise에서 set_exception을 통해 스레드 내부의 익셉션을 다른 스레드로 넘길 수 있다.   

**shared_future**   
기본적으로 future는 이동 연산을 수행한다. 따라서 future 1개당 1번의 get만 호출할 수 있다. shared_future는 여러 스레드에서 get을 여러번 호출하고 싶을 때 사용한다.
