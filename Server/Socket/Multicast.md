# Multicast

## Multicast
UDP를 기반으로 특정 그룹에 데이터를 전송하는 기술. 멀티캐스트는 라우터에서 데이터를 복제해주기 때문에 라우터를 거치지 않는 영역은 패킷의 크기가 줄어든다.
<br/>

**TTL** : Time to Live, 패킷의 거리, 라우터를 지나면 1이 감소하고 0이되면 패킷이 소멸한다.
```c++
//TTL 설정, time_live는 int다.
setsockopt(send_sock, IPPROTO_IP, IP_MULTICAST_TTL, (void*)&time_live, sizeof(time_live));

// 그룹 가입
struct ip_mreq join_adr;
join_adr.imr_multiaddr.s_addr = "멀티캐스트 그룹 주소";
join_adr.imr_interface.s_addr = "그룹의 호스트 주소";
setsockopt(recv_sock, IPPROTO_IP, IP_ADD_MEMBERSHIP, (void*)&join_adr, sizeof(join_adr));

```

### 멀티캐스트 전송의 특성
- 특정 멀티캐스트 그룹을 대상으로 데이터를 딱 한 번 전송한다
- 한 번의 전송으로 모든 클라이언트는 데이터를 수신한다
- IP 주소 범위 내라면 그룹의 수를 얼마든지 추가할 수 있다.
- 그룹에 참여해야 해당 그룹의 데이터를 수신할 수 있다.

### 멀티캐스트 송수신
**송신**   
UDP 소켓을 만들고 TTL을 설정한 뒤 대상을 멀티캐스트 그룹의 주소와 포트로 지정한다.   

**수신**   
멀티캐스트 그룹에 들어가고 수신

## Broadcast
동일한 네트워크의 모든 호스트에게 데이터를 전송하는 기술. IP주소의 형태에 따라 Directed, Local로 구분한다.

```c++
// so_brd = 1을 주면 브로드캐스트가된다. 클라이언트에는 별 다른 작업을 해줄 필요가 없다. 윈도우에서도 큰 차이는 없다.
setsockopt(send_sock, SOL_SOCKET, SO_BROADCAST, (void*)&so_brd, sizeof(so_brd));
```