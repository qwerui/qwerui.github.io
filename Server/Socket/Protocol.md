# Protocol

## TCP
### 연결
1. A에서 B로 SEQ를 보낸다
2. B가 A로 SEQ+ACK를 보낸다 ACK는 SEQ 번호+1이다.
3. A가 B로 SEQ+ACK를 보낸다

### 데이터 송수신
1. A에서 B로 SEQ와 데이터를 보낸다.
2. B가 A로 ACK를 보낸다. ACK는 SEQ의 번호+전송된 바이트 크기+1이다.
3. A는 수신받은 ACK를 토대로 다음 데이터를 보낸다.
4. 데이터가 유실된 경우, 응답이 돌아오는 시간이 time out length만큼 지나면 재전송한다.

### 연결 해제
1. A에서 B로 FIN+SEQ를 보낸다.
2. B가 A로 SEQ+ACK를 보낸다.
3. B에서 A로 FIN+SEQ+ACK를 보낸다. 2번 전송 이후 데이터 수신이 없어 ACK는 재전송된다.
4. A에서 B로 SEQ+ACK를 보낸다. 