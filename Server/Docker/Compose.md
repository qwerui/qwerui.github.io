# Docker Compose

## 개요
시스템 구축과 관련된 명령어를 하나의 텍스트 파일에 기재해 명령어 한 번으로 시스템 전체의 생성, 실행, 종료, 폐기를 가능하게 해준다. YAML 형식이며, 한 폴더에 하나만 존재한다. YAML은 공백 2개로 블록을 구분한다.

## 설치
1. sudo apt install python3 python3-pip
2. sudo pip3 install docker-compose

## 컴포즈 파일 작성 주의사항
- 첫 줄에 도커 컴포즈 버전 기재
- 주 항목 services(컨테이너), networks(네트워크), volumes(볼륨) 아래에 내용 기재
- 주 항목 하위에 이름 하위에 설정을 기재한다.
- 여러 항목을 기재하려면 줄 앞에 -를 기재한다.
- 콜론 뒤에는 줄바꿈 혹은 공백이 있어야한다.
- #뒤는 주석이다
- 문자열은 따옴표로 감싸야한다.

## 컴포즈 파일 정의
### 주 항목
- services : 컨테이너 정의
- networks : 네트워크 정의
- volumes : 볼륨 정의

### 정의 내용
- image : 사용할 이미지 지정
- networks : 접속할 네트워크 지정
- volumes : 스토리지 마운트 지정
- ports : 포트 지정
- environment : 환경변수 설정
- build : 이미지를 빌드할 폴더 지정
- depends_on : 다른 서비스에 대한 의존 관계 정의
- restart : 컨테이너 종료 시 재시작 여부
+ no : 재시작 하지 않음
+ always : 항상 재시작
+ on-failure : 프로세스가 0이외의 상태로 종료되면 재시작
+ unless-stopped : 종료 시 재시작하지 않음, 그 이외는 재시작

### 그 외 항목
- command : 컨테이너 시작 시 기존 커맨드 오버라이드
- container_name : 실행 컨테이너 이름 지정
- dns : DNS 서버 지정
- env_file : 환경변수 파일 로드
- entrypoint : 컨테이너 시작시 ENTRYPOINT 설정 오버라이드
- external_links : 외부 링크 설정
- extra_hosts : 외부 호스트의 IP 주소 지정
- logging : 로그 출력 대상 지정
- network_mode : 네트워크 모드 설정

## 실행
도커 컴포즈는 docker-compose 명령을 사용한다. 일반적으로 docker-compose -f 컴포즈파일경로 커맨드 옵션으로 사용한다.

**주요 커맨드**   
- up 커맨드 : docker run에 해당하는 명령
+ -d : 백그라운드 실행
+ --no-color : 화면 출력을 흑백으로 함
+ --no-deps : 링크된 서비스를 실행하지 않음
+ --force-recreate : 설정이나 이미지가 변경되지 않아도 컨테이너 재생성
+ --no-create : 컨테이너가 이미 존재할 경우 생성하지 않음
+ --no-build : 이미지가 없어도 이미지를 빌드하지 않음
+ --build : 컨테이너를 실행하기 전에 이미지를 빌드
+ --abort-on-container-exit : 컨테이너가 하나라도 종료되면 전체 컨테이너를 종료
+ -t, -timeout : 컨테이너를 종료할 때 타임아웃, 기본 10초
+ --remove-orphans : 컴포즈 파일에 정의되지 않은 서비스의 컨테이너 삭제
+ --scale : 컨테이너 수 변경
- down 커맨드 : 컨테이너와 네트워크를 정지 및 삭제하는 명령
+ --rmi all/local : 삭제 시 이미지도 삭제, all이면 모든 이미지, local이면 커스텀 태그가 없는 이미지만
+ -v, -volumes : 볼륨 제거, external로 지정된 볼륨은 삭제되지 않음
+ --remove-orphans : 컴포즈 파일에 정의되지 않은 서비스의 컨테이너 삭제
- stop 커맨드 : 삭제하지 않고 종료만 하는 명령
<br/>