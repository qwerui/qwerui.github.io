# Firewall Server

## 방화벽 서버 구현
1. 서버에 2개의 랜카드를 설치하고(외부 1개, 내부 1개), 내부 네트워크를 설정한다.
2. /etc/sysctl.conf의 net.ipv4.ip_forward=1의 주석을 해제 후, echo 1 > /proc/sys/net/ipv4/ip_forward로 설정이 바로 포워딩 될 수 있도록한다.
3. iptables를 이용해 방화벽 설정
4. iptables-save > /etc/iptabls.ruels로 저장