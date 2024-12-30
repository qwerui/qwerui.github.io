# NFS Server

## NFS
Network File System, 네트워크 상의 다른 컴퓨터의 저장 공간을 로컬 저장 공간 처럼 사용한다.

## 구현
**Server**
1. apt -y install nfs-kernel-server
2. /etc/exports 수정
```
공유 디렉토리 경로  IP또는도메인(접근권한)
//접근권한 ro(read only), rw(read write)
```
3. 공유 디렉토리 권한 707로 설정
4. systemctl restart nfs-server
5. exportfs -v로 작동 확인

**Client**
1. apt -y install nds-common
2. showmount -e 서버IP로 공유된 디렉토리 확인
3. mkdir로 마운트할 디렉토리 생성
4. mount -t nfs 서버IP:서버공유디렉토리 3번디렉토리
5. 부팅 시 자동 마운트는 /etc/fstab을 수정
```
서버IP:공유디렉토리 클라이언트디렉토리  nfs defaults 0 0
```