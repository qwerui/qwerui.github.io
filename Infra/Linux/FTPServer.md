# FTP Server

## vsftpd
1. apt -y install vsftpd
2. /etc/vsftpd 수정
3. cd /srv/ftp (기본 값)
4. 디렉토리 생성 후 777권한 부여
5. systemctl restart vsftpd
6. FileZilla, lftp 등 ftp 클라이언트로 접속

**/etc/vsftpd**   
- annonymous_enable : 익명 사용자 접속 허가 여부
- local_enable : 로컬 사용자 접속 허가 여부
- write_enable : 로컬 사용자 수정 허가 여부
- anon_upload_enable : 익명 사용자 업로드 허가 여부
- anon_mkdir_write_enable : 익명 사용자 디렉토리 생성 허가 여부
- dirlist_enable : 접속한 디렉토리 파일 리스트 출력 여부
- download_enable : 다운로드 허가 여부
- listen_port : FTP 포트 번호
- deny_file : 업로드 금지 파일 지정
- hide_file : 숨김 파일 지정
- max_clients : FTP 최대 접속자 수
- max_per_ip : 1대당 동시 접속자 수

## proftpd
1. apt -y install proftpd
2. etc/proftpd/profrtpd.conf 수정
3. systemctl restart proftpd

**etc/proftpd/profrtpd.conf**   
- ServerName : 서버 이름
- DefaultServer : 기본 FTP 서버로 사용할지 결정
- MaxInstances : 최대 생성 프로세스 수
- User/Group : 실행 시 실행되는 사용자
- Global 태그 : 접속된 모든 사용자에게 공통 적용 속성
- AllowOverride : 디렉토리 안에 같은 파일 존재시 덮어쓰기 여부
- Anonymous 태그 : 익명 사용자 접속 여부
- UserAlias anonymous ftp : 익명 사용자의 권한 부여
- MaxClients : 익명 사용자의 최대 접속 수
- Directory 태그 : 업로드 디렉토리 설정
- Limit READ 태그 : 읽기 허용 여부
- Limit STOR 태그 : 쓰기 허용 여부