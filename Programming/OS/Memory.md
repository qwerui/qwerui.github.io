# Memory

## 개요
- 메모리 : 주소를 가지는 커다란 바이트 배열, CPU는 프로그램 카운터를 사용해 메모리에서 명령을 가져온다. 명령은 메모리에서 로드하거나 저장한다.
- 메모리 공간 : base 레지스터, limit 레지스터에 의해 프로세스의 메모리 주소 범위를 나타낸다. 이 범위를 벗어나는 경우 c++에서 sagmentation fault를 발생시키며 종료된다.
- 주소 바인딩 : 소스코드에서는 symbolic 주소를 사용한다. 컴파일러는 이 symbolic 주소를 relocatable 주소로 변환하고, 링커가 relocatable 주소를 absolute 주소로 변환한다.
- MMU : 논리적 주소와 물리적 주소 간 변환장치

## 링크
- DLL(Dynamic Linked Libaraies) : 유저 프로그램이 실행되는 중에 링크되는 시스템 라이브러리
- static linking : 로더에 의해 이진 프로그램 코드에 결합되는 라이브러리
- dynamic linking : dynamic loading과 비슷, 실행 시 까지 링크를 지연
- shared library : 메모리에 하나의 DLL이 있어도 여러 프로세스에서 공유

## 메모리 할당
- 연속 메모리 할당 : 각 프로세스는 하나의 메모리 영역을 가지고, 물리 메모리 한 곳에 연속적으로 할당한다.
+ 고정분할 : 메모리를 일정 크기로 나눈다.
+ 가변분할 : 메모리를 프로세스의 크기만큼 나눈다.

### 동적할당의 할당 방법
- First-Fit : 프로세스를 적재할 수 있는 공간중 가장 먼저 발경한 공간에 적재
- Best-Fit : 프로세스를 적재할 수 있는 공간중 가장 작은 공간에 적재
- Worst-Fit : 프로세스를 적재할 수 있는 공간중 가장 큰 공간에 적재

### 메모리 단편화
- 내부 단편화 : 할당된 메모리보다 프로세스의 크기가 작은경우
- 외부 단편화 : 프로세스의 메모리 사이의 빈 공간

## 페이징
메모리 관리 체계, 프로세스의 물리 주소 공간을 연속적이지 않게한다.
- 외부 단편화 해결
- 메모리 컴팩션과 관련된 필요성 회피

**기본 원리**   
- 물리 메모리를 고정 크기로 나눈다.(Frame)
- 같은 크기로 논리 메모리를 나눈다.(Page)
이제 운영체제가 알아서 논리 메모리와 물리 메모리를 매핑한다. CPU가 생성한 모든 주소는 2개(페이지 번호, 페이지 오프셋)로 나눌 수 있다.
<br/>

- 페이지 번호 : 페이지 테이블의 인덱스
- 페이지 오프셋 : 시작 주소로부터의 오프셋
- 페이지 크기 : 하드웨어에 의해 정의, 2의 제곱수를 갖는다. 페이지 크기가 $$2^n$$이고 논리 주소 공간의 크기가 $$2^m$$일 때 페이지 번호는 m-n이고 페이지 오프셋은 n이다.
<br/>

컨텍스트 스위치가 일어날 때 페이지 테이블도 다시 불러와야한다.
- PTBR(Page Table Base Register) : 페이지 테이블을 가리키는 레지스터, 컨텍스트 스위치가 빨라지지만, 메모리 접근 시간은 느려진다. 2번의 메모리 접근이 필요하다.
- TLB(Translation Look-aside Buffer) : 페이지 테이블의 캐시
<br/>

**기타 배경지식**   
- 메모리 보호 : 비트 1개를 보호 비트로 둔다. 일반적으로 페이지 테이블의 각 엔트리에 부착된다.
- 계층적 페이징 : 페이지 테이블이 페이지 테이블을 참조한다. 대규모의 페이지 테이블을 관리하는데 사용된다.
- 해시 페이지 테이블 : 해시값을 이용해 페이지 테이블에 접근
- 역 페이지 테이블 : PID가 어떤 페이지를 가지고 있는지를 저장

## 스와핑
물리 메모리보다 큰 크기의 프로세스를 실행하기 위한 기술. 필요할 때만 메모리에 적재(page in)하고 그렇지 않으면 메모리에서 내린다(page out). 일반적으로 페이징과 같이 사용되어 페이징 자체를 swapping with paging이라고 하기도한다.

## 가상 메모리
메모리를 거대한 스토리지로 추상화해 필요할 때마다 페이지를 물리 메모리와 매핑하는 기술, 이 때 필요한 페이지를 Demand page라고 한다.

### Demand Page
**기초 이론**   
프로세스 실행 중에 몇몇 페이지는 메모리에 있고, 나머지는 스토리지에 있다. 이 두 상황을 구별하는 방법으로 유효-비유효 비트를 사용하는 것이 있다.
- valid : 메모리에 있고, 적법한 상태
- invalid : 적법하지 않거나, 스토리지에 있는 상태
<br/>

**Page Fault 처리**   
1. 내부 페이지 테이블을 확인해 유효한지 확인
2. 유효한 경우 프로세스를 처리
3. 유효하지 않다면 비어있는 프레임을 찾는다.
4. 스토리지에서 페이지를 읽는다.
5. 페이지 테이블을 유효로 변경한다.
6. 명령을 재실행한다.
<br/>

- Pure Demand Paging : 필요하기 전에는 절대 페이지를 가져오지 않음, 메모리에 페이지를 적재하지 않고도 프로세스를 실행할 수 있다.
- Locality of Reference : 짧은 시간동안 동일한 페이지에 액세스하는 경향

## 페이지 재배치 알고리즘
- FIFO : 먼저 적재한 것을 먼저 제거한다.
- OPT : 앞으로 가장 사용되지 않을 것 같은 페이지를 제거한다. 미래의 정보를 알아야 가능하다.
- LRU : 최근에 가장 적게 사용된 페이지를 제거한다. 하드웨어의 지원이 필요하다.

## 스래싱
Page Fault가 너무 많이 발생해 프로세스 처리 시간보다 페이지 교체 시간이 더 길어지는 현상.
- Working Set : 자주 사용하는 페이지의 집합