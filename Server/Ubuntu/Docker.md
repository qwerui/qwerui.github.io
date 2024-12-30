# Docker

## 도커
작은 운영체제를 포함한 가상화 기술

## 구현
1. apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
2. curl -fsSL https://download.docker.com/linux/ubuntu/gpg \| apt-key add - (도커에서 배포하는 공식 GPG키 추가, curl = CLI기반 웹 업로드/다운로드 요청 명령, gpg = 암호화 기술)
3. add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" (도커 저장소 추가)
4. apt update 
5. apt -y install docker-ce docker-ce-cli containerd.io (libseccomp2 수동설치 필요 가능성 있음)
6. docker run hello-world로 설치 확인

## 도커 사용
다운로드 : docker pull 이미지 (이미지 별로 상이할 수 있음)
실행 : docker run -it
도커 목록 : docker images
도커 상세 정보 : docker container ls -a
도커 삭제 : docker rm 컨테이너D
