# Command

## Docker 명령어
컨테이너를 다루는 모든 명령은 docker로 시작한다. 일반적으로 docker 커맨드 [옵션] 대상 [인자]순이다.

### container
컨테이너 조작 관련, 대부분 container를 생략할 수 있다.

- start : 컨테이너 실행
- stop : 컨테이너 정지
- create : 도커 이미지로 컨테이너 생성
- run : 도커 이미지를 내려받고 컨테이너를 생성해 실행
+ --name 컨테이너이름 : 컨테이너 이름지정
+ -p 호스트포트:컨테이너포트 : 포트 번호 지정
+ -v 호스트디스크:컨테이너디렉토리 : 볼륨 마운트
+ --net=네트워크이름 : 네트워크 연결
+ -e 환경변수이름=값 : 환경변수 설정
+ -d : 백그라운드 실행
+ -i : 컨테이너에 터미널 연결
+ -t : 특수키 사용 가능
+ -help : 사용방법 안내
- rm : 정지 상태 컨테이너 삭제
- exec : 실행 중인 컨테이너에서 프로그램 실행
- ls : 컨테이너 목록, ps로도 사용가능
+ -a : 모든 컨테이너 출력
- cp : 도커 컨테이너와 도커 호스트 간 파일 복사, docker cp 원본경로 복사경로로 사용하며, 컨테이너는 컨테이너이름:경로로 지정한다.
- commit : 도커 컨테이너를 이미지로 변환

### image
이미지 관련 기능

- pull : 도커 허브 등의 리포지토리에서 이미지 다운로드
- rm : 도커 이미지 삭제
- ls : 도커 이미지 목록
- build : 도커 이미지 생성

### volume
볼륨(컨테이너에 마운트 가능한 스토리지) 관련 기능

- create : 볼륨 생성
- inspect : 볼륨의 상세 정보 출력
- ls : 볼륨의 목록
- prune : 마운트 되지 않은 모든 볼륨 삭제
- rm : 지정 볼륨 삭제

### network
네트워크 관련 기능, 컨테이너와 인터넷 연결 뿐만 아니라 도커 간 연결인 도커 네트워크 구축에도 사용한다.

- connect : 컨테이너를 도커 네트워크에 연결
- disconnect : 컨테이너를 도커 네트워크에서 분리
- create : 도커 네트워크 생성
- inspect : 도커 네트워크 상세 정보 출력
- ls : 도터 네트워크 목록
- prune : 컨테이너가 접속하지 않은 도커 네트워크 전체 삭제
- rm : 특정 도커 네트워크 삭제

### 도커 스웜 관련
현재는 쿠버네티스를 더 많이 사용하기 때문에 있다 정도로만 알아도 된다.

- node : 도커 스웜의 노드 관리
- secret : 도커 스웜의 비밀값 정보 관리
- service : 도커 스웜의 서비스 관리
- stack : 도커 스웜 또는 쿠버네티스에서 여러 개의 서비스를 합쳐 구성한 스택을 관리
- swarm : 도커 스웜 관리

### 기타 상위 커맨드
- checkpoint : 현재 상태 저장, 나중에 저장한 시점으로 되돌릴 수 있다. 현재 실험적 기능
- system : 도커 엔진의 정보 확인
- plugin : 플러그인 관리
- login : 도커 레지스트리에 로그인
- logout : 도커 레지스트리에서 로그아웃
- search : 도커 레지스트리 검색
- version : 도커 버전 확인