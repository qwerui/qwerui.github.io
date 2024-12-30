# PXE

## PXE
Preboot Execution Environment, OS가 설치되지 않은 컴퓨터가 네트워크를 통해 PXE 서버에 접속해서 부팅한다.   
DHCP+TFTP+FTP또는웹서버로 구성

## 구현
1. apt -y install isc-dhcp-server tftpd-hpa inetutils-inetd vsftpd pxelinux
2. iso파일을 저장할 디렉토리를 생성 후 777권한 부여
3. 다운로드한 iso 파일을 마운트 한뒤 부팅 관련 파일을 복사한다.
```
mount iso파일이름 /mnt
cp /mnt/casper/vm* srv/tftp
cp /mnt/casper/initrd* srv/tftp
cp /usr/lib/syslinux/modules/bios/ldlinux.c32 srv/tftp
umount /mnt
```
4. pxelinux.0를 다운로드한다.
5. /etc/dhcp/dhcpd.conf 수정
```
//아래 내용을 추가
subnet IP주소 netmask 넷마스크 {
    option routers 게이트웨이;
    option subnet-mask 넷마스크;
    range dynamic-bootp 시작IP 끝IP;
    option domain-name-servers 도메인서버IP;
    allow booting;
    allow bootp;
    next-server 서버IP;
    option bootfile-name "pxelinux.0"
}
```
6. srv/tftp/pxelinux.cfg/default 생성 후 작성
```
DEFAULT Ubuntu_Auto_Install
LABEL Ubuntu_Auto_Install
      kernel vmlinuz
      initrd initrd
      append ip=dhcp url=ftp://서버IP/iso파일경로
      //srv하위의 ftp폴더
```
7. systemctl restart isc-dhcp-server/vsftpd/tftpd-hpa