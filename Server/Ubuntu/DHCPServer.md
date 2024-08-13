# DHCP Server

# DHCP 작동 순서
1. 클라이언트가 IP 주소를 요청
2. DHCP 서버에서 할당 전 IP를 할당상태로 변경 후 클라이언트에 주소를 할당
3. 클라이언트가 할당받은 IP로 인터넷 사용 후 반납
4. DHCP 서버에서 반납된 IP를 할당 전으로 변경

# 구현
1. apt -y install isc-dhcp-server
2. /etc/dhcp/dhcpd.conf 수정
```
//아래 내용 추가
subnet 네트워크 주소 netmask 넷마스크 {
    option routers 게이트웨이;
    option subnet-mask 넷마스크;
    range dynamic-bootp 시작IP 끝IP;
    option domain-name-servers 네임서버주소;
    default-lease-time 임대시간;
    max-lease-time 최대임대시간;
}

//특정 컴퓨터에 고정 IP 예약
host ns {
    hardware Ethernet MAC주소;
    fixed-address 고정IP주소;
}
```
3. /var/lib/dhcp/dhcpd.leases 존재 확인(이 파일에 IP 할당 로그가 기록된다)
4. systemctl restart isc-dhcp-server