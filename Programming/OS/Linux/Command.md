# Command

[Shell Script](https://wikidocs.net/book/2370)

## 시간
```sh
date # 현재 시간 출력
timedatectl # 현재 시간 설정 확인
cal # 달력 출력
```

## 사용자
```sh
logname # 로그인 네임 확인
users # 접속한 사용자의 로그인 네임 확인
who # 모든 사용자 계정
whoami # 현재 사용자 계정
groups # 현재 사용자 그룹 출력
su # 프롬프트 변경, 사용자 변경
useradd # 사용자 생성
passwd # 비밀번호 설정
usermod # 사용자 정보 수정
chage # 사용자 암호 기한 설정
userdel # 사용자 삭제
groupadd # 그룹 생성
gpasswd # 그룹 암호 설정
id # 사용자의 소속 그룹 출력
newgrp # 소속 그룹 변경
```

## 시스템
```sh
uname # 시스템 정보 출력
hostname # 호스트 네임 출력
arch # CPU 아키텍처 출력
env # 환경변수 출력
export # 환경변수 설정
unset # 환경변수 삭제
echo # 문자열 출력
which # 명령어의 위치 확인
history # 명령어 로그 확인
![숫자] # [숫자]번째 실행한 명령 실행
help # 도움말
shutdown # 시스템 종료
reboot # 재시작
```

## 파일 & 디렉토리
```sh
ls # 파일 조회
pwd # 현재 위치를 절대경로로 출력
cd # 디렉토리 이동
cp # 복사
mv # 이동
rm # 삭제
touch # 파일 생성
mkdir # 디렉토리 생성
rmdir # 디렉토리 삭제
cat # 파일 내용 출력
head # 첫 라인부터 크기 지정 출력
tail # 마지막 라인부터 크기 지정 출력
more # 파일 내용을 화면 단위로 출력
grep # 문자열 검색
whereis # 명령위치 확인
ln # 링크 생성
chmod # 접근 권한 설정
umask # 기본 접근 권한 설정, 파일은 666, 디렉토리는 777에서 umask의 값을 뺀 것과 같다.
chown # 소유자 변경
chgrp # 소유 그룹 변경
```

## 프로세스
```sh
ps # 현재 실행 중인 프로세스 확인
kill # 프로세스 종료
service # 서비스 관리 systemd에서 관리, systemctl과 비슷
```