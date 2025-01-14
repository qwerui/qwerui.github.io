# Smart Pointer

## 스마트 포인터 개요
Header : ```<memory>```   
C++에서 new로 할당한 객체는 delete를 호출해야한다. 프로그래머가 new로 할당하고 delete를 호출하지 않은 경우 메모리 누수로 이어진다. 하지만 프로그래머도 사람이기에 delete를 호출하지 않을 때도 있다.   
스마트 포인터는 더 이상 사용하지 않는 객체를 자동으로 해제해준다. RAII를 기반으로 구현되어있다.   
**RAII** : 스택에 선언된 개체가 리소스(ex: 힙)를 소유한다. 개체가 스택에 선언되면 생성자를 호출하고, 블록을 벗어나면 소멸자를 호출한다. 이에 따라 생성자에서 리소스를 소유하고, 소멸자에서 리소스를 반환하는 패턴이다.

## 스마트 포인터 종류
### unique_ptr
객체의 소유권을 1개만 인정한다. 공유나 복사는 불가능하지만 이전할 수 있다.

### shared_ptr
참조횟수가 계산되는 스마트 포인터다. 참조횟수가 0이되면 객체는 소멸한다. 포인터에 참조횟수를 카운팅하는 테이블의 포인터가 래핑되어있어 Raw 포인터보다는 크기가 크다.   
테이블은 대상 객체에 대해 첫 shared_ptr을 생성했을 때 같이 생성되며, 이후 선언된 shared_ptr은 그 테이블을 공유한다.   


### weak_ptr
참조횟수가 증가하지 않는 스마트 포인터다. 주로 shared_ptr과 함께 사용되며, 순환참조로 인한 메모리 누수를 방지하는데 사용한다. 포인터에 weak ref를 카운팅하는 테이블의 포인터가 래핑되어있어 Raw 포인터보다는 크기가 크다.   
weak ref의 존재의의는 참조횟수가 0이 되어 객체가 소멸했을 때, 참조횟수를 카운팅하던 테이블을 유지시켜 weak_ptr에서 객체가 소멸했는지 안전하게 판별할 수 있게해준다.