# IO

## 개요
OS는 IO의 작업을 제어한다. CPU는 PCie bus를 통해 각 컨트롤러에 명령을 내린다.

**IO의 타입**   
- polling : 반복적으로 읽기
- interrupt : wait-signal
- DMA(Direct Memory Access) : 대용량 데이터 전송에 사용

## Memory-mapped IO
IO address를 메모리와 매핑한다.

**컨트롤러가 가지는 요소**
- data-in register : 입력
- data-out register : 출력
- status register : 호스트가 읽을 수 있는 상태 비트
- control register : 호스트에 의한 디바이스 제어